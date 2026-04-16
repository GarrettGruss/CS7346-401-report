# Risks and Gaps

## Architecture Gaps

### GAP-01: No Formal Protocol Specification
**Severity:** High
**Requirement:** NFR-Interoperability — "The system shall expose a well-defined, documented communication protocol that module vendors can implement independently" and "accommodate new device types without requiring changes to the core platform architecture."

The current architecture selects MQTT as the transport layer but does not define the protocol surface that hardware vendors will implement against. Missing elements include the MQTT topic naming convention and hierarchy, the message envelope schema (payload format, required fields, versioning), the device capability manifest format (how a new module type declares its supported commands and telemetry), and the handshake sequence for registration and policy delivery. Without this specification, the electrical engineering teams have no contract to implement against, and the architecture cannot claim extensibility to new device types.

**Resolution required:** Define and document the MQTT topic schema, message envelope format, device capability advertisement model, and registration handshake protocol as a first-class architecture artifact.

---

### GAP-02: Mobile Application Distribution Path Not Represented
**Severity:** Medium
**Requirement:** FR-UI — "The system shall provide a consumer-facing application, accessible via mobile and/or web."

The architecture represents the frontend exclusively as a web application served via CloudFront and S3. No distribution path exists for native iOS or Android applications, nor is a Progressive Web App (PWA) strategy stated. Given that the primary use case involves remote access — consumers checking cameras or issuing commands from outside the home — a mobile-native experience is commercially important and technically distinct in terms of push notification delivery, background connectivity, and app store distribution.

**Resolution required:** Either extend the architecture to include a native mobile app distribution path (App Store / Google Play) with a corresponding push notification channel via SNS, or explicitly justify why a PWA served through CloudFront is sufficient to meet the mobile requirement.

---

### GAP-03: 99.9% Availability SLA Not Traced to Design Decisions
**Severity:** Medium
**Requirement:** NFR-Reliability — "Cloud-side platform services shall maintain a minimum availability of 99.9%."

The architecture relies on AWS managed services that individually meet or exceed 99.9% availability, but the design descriptions do not explicitly state the multi-AZ deployment configuration of Aurora, the ECS task placement constraints, the IoT Core regional redundancy posture, or the defined Recovery Time Objective (RTO) and Recovery Point Objective (RPO). Without tracing the SLA to specific architectural decisions, the requirement is assumed rather than demonstrated.

**Resolution required:** Document explicit multi-AZ deployment decisions for Aurora and ECS, state the RTO/RPO targets, and map the 99.9% SLA to the composed availability of the critical-path services.

---

### GAP-04: End-to-End Command Latency Not Analyzed
**Severity:** Medium
**Requirement:** NFR-Performance — "The system shall execute remote control commands with end-to-end latency not exceeding two seconds under normal operating conditions."

The two-second latency budget spans the full chain from consumer action to device acknowledgment: CloudFront → API Gateway → ECS Fargate → IoT Core → MQTT delivery → device → acknowledgment → return path. No latency breakdown or budget allocation has been performed across these hops, and the most variable leg — MQTT delivery over the consumer's home WiFi — has not been characterized. The architecture cannot be said to satisfy this requirement without analysis.

**Resolution required:** Perform a latency budget analysis across the command path, identify the dominant latency contributors, and confirm or adjust the architecture if any segment is likely to exceed its allocation under normal conditions.

---

### GAP-05: MFA Enforcement Policy Not Stated
**Severity:** Low
**Requirement:** NFR-Security — "Remote access shall support strong authentication mechanisms, including multi-factor authentication."

Amazon Cognito supports MFA, but the architecture does not state whether MFA is required for all consumer accounts, optional but encouraged, or conditionally required based on access scope (e.g., required for access delegation but optional for read-only monitoring). Leaving MFA as an unconfigured Cognito capability rather than a stated policy decision means the requirement is technically present but not enforced.

**Resolution required:** State the MFA policy explicitly — required for all accounts, required for administrative operations, or opt-in — and reflect that decision in the Cognito and API Gateway descriptions.

---

## Risks

### RISK-01: IoT Core MQTT Topic Collision Across Households
**Likelihood:** Medium | **Impact:** High
If the MQTT topic hierarchy is not carefully namespaced by household or device identity, a misconfigured or malicious controller could publish commands to topics belonging to another household's devices. IoT Core policies enforce topic-level access control, but those policies are only as correct as the topic schema they reference. An under-specified schema creates a window where policy definitions lag behind device enrollment, leaving devices temporarily accessible across household boundaries.

**Mitigation:** Define the topic hierarchy to include household ID and device ID as mandatory path segments (e.g., `home/{householdId}/device/{deviceId}/cmd`), and make IoT Core policy templates reference these segments by variable substitution so they are generated correctly and consistently at registration time.

---

### RISK-02: EventBridge Rule Volume at Scale
**Likelihood:** Medium | **Impact:** Medium
Each automation rule defined by a consumer with a time-based trigger creates a corresponding EventBridge schedule. At thousands of households with multiple rules each, the total schedule count could become significant. EventBridge Scheduler has service limits on the number of schedules per account, and a high schedule density introduces management complexity around rule lifecycle (creation, modification, deletion) that must be kept in sync with the Aurora rule definitions.

**Mitigation:** Evaluate whether a single recurring EventBridge schedule that polls Aurora for due rules is more operationally sustainable than one schedule per consumer rule. Apply account-level limit monitoring via CloudWatch to detect schedule count approaching service thresholds.

---

### RISK-03: Kinesis Video Streams Egress Cost at Scale
**Likelihood:** High | **Impact:** Medium
Kinesis Video Streams charges both for ingestion and for data retrieval (GetMedia API calls). If a large proportion of households have active cameras streaming continuously, and consumers frequently access live or recorded footage, egress costs can grow nonlinearly with the user base. This is compounded by S3 storage costs for archived footage if lifecycle policies are not aggressively tuned.

**Mitigation:** Impose a default retention window on video segments (e.g., 7 days in Kinesis, then S3 Glacier transition) and make extended retention a paid tier feature. Evaluate per-household streaming quotas to prevent any single unit from disproportionately driving egress costs.

---

### RISK-04: Aurora Cold-Start Latency with Serverless Scaling
**Likelihood:** Medium | **Impact:** Medium
If Aurora Serverless v2 is chosen to reduce standing compute costs, scaling events triggered by sudden traffic spikes — such as a large number of households all issuing commands simultaneously — can introduce transient latency as Aurora adds capacity. For read-heavy operations this may be tolerable, but for the command path (UC-01) where the two-second SLA applies, Aurora scaling latency could contribute to SLA breaches.

**Mitigation:** Provision a minimum Aurora capacity unit floor that keeps the cluster warm for expected baseline traffic. Reserve Aurora Serverless auto-scaling for the upper range rather than allowing it to scale from zero.

---

### RISK-05: Consumer Push Notification Deliverability Dependence on SNS
**Likelihood:** Low | **Impact:** Medium
The event notification use case (UC-05) depends on SNS successfully delivering push notifications, SMS, or email to consumers within a time window that makes the alert actionable. SNS delivery is subject to third-party carrier delays for SMS, platform-specific throttling for push notifications (APNs, FCM), and email filtering. A consumer who has configured only one notification channel has no fallback if that channel is delayed or blocked.

**Mitigation:** Support multiple simultaneous notification channels per consumer and surface delivery failure feedback (via SNS delivery status logs in CloudWatch) so the platform can detect persistently undeliverable endpoints and prompt the consumer to update their preferences.

---

### RISK-06: Device Registration Race Condition on Initial Power-On
**Likelihood:** Low | **Impact:** Medium
If a consumer powers on a new module before completing account creation, the device's registration handshake will arrive at IoT Core with no matching household to associate it with. Depending on how Device Management handles unmatched registration events, the device could be left in an orphaned state, requiring manual intervention or a re-pairing process that degrades the turnkey onboarding experience.

**Mitigation:** Design the registration flow to queue unmatched device registration events for a bounded window (e.g., 10 minutes) and complete the association when the consumer's account setup confirms a matching household. Pair this with clear UI guidance that account creation should precede device power-on.
