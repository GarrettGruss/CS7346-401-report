# Lights, Please — Key IoT Architecture Components

Based on the WebbyLab broker-based IoT architecture diagram.

---

## Load Balancer / WAF

Sits at the edge; routes MQTT (from controllers/hubs) and HTTP (from end users) traffic into the platform. The WAF filters malicious requests before they reach internal services.

| Provider | Load Balancer | WAF |
|---|---|---|
| AWS | Elastic Load Balancing (ALB/NLB) | AWS WAF |
| GCP | Cloud Load Balancing | Google Cloud Armor |
| Azure | Azure Load Balancer / Application Gateway | Azure WAF |

---

## Connectivity — MQTT Broker

The central broker that receives MQTT messages from IoT controllers/hubs and routes them to backend services.

| Provider | Offering |
|---|---|
| AWS | AWS IoT Core |
| GCP | Google Cloud IoT Core (deprecated) → Pub/Sub with MQTT bridge |
| Azure | Azure IoT Hub |
| Self-hosted | Eclipse Mosquitto, HiveMQ, EMQX |

---

## Services — Backend

Core application logic: processes device events, executes automation rules, serves API requests from the frontend.

| Provider | Offering |
|---|---|
| AWS | ECS / EKS (containers), Lambda (serverless), Elastic Beanstalk |
| GCP | Cloud Run, GKE, App Engine |
| Azure | Azure App Service, AKS, Azure Functions |

---

## Services — Frontend

Serves the web/mobile UI to end users (dashboards, device control, rule configuration).

| Provider | Offering |
|---|---|
| AWS | S3 + CloudFront (static), Amplify |
| GCP | Firebase Hosting, Cloud CDN |
| Azure | Azure Static Web Apps, Azure CDN |

---

## Services — Device Management

Handles device provisioning, registration, configuration, and OTA firmware updates.

| Provider | Offering |
|---|---|
| AWS | AWS IoT Device Management, AWS IoT Device Defender |
| GCP | Google Cloud IoT Device Manager |
| Azure | Azure IoT Hub Device Provisioning Service (DPS) |

---

## Services — Video Stream

Ingests and distributes live video feeds from cameras; supports recording and playback.

| Provider | Offering |
|---|---|
| AWS | Amazon Kinesis Video Streams |
| GCP | Google Cloud Video Intelligence API + Pub/Sub |
| Azure | Azure Media Services, Azure Video Indexer |

---

## Services — Media Collector

Collects and stores media assets (snapshots, video clips, motion-triggered recordings).

| Provider | Offering |
|---|---|
| AWS | S3 + Lambda trigger |
| GCP | Cloud Storage + Cloud Functions |
| Azure | Azure Blob Storage + Azure Functions |

---

## Services — Backups

Automated backups of databases, configuration, and stored media.

| Provider | Offering |
|---|---|
| AWS | AWS Backup, S3 Lifecycle policies, RDS automated backups |
| GCP | Cloud Storage with retention policies, Cloud SQL backups |
| Azure | Azure Backup, Azure Site Recovery |

---

## SQL (Relational Database)

Stores structured data: user accounts, device registry, automation rules, event history.

| Provider | Offering |
|---|---|
| AWS | Amazon RDS (PostgreSQL/MySQL), Aurora |
| GCP | Cloud SQL, AlloyDB |
| Azure | Azure SQL Database, Azure Database for PostgreSQL |

---

## File Storage

Stores unstructured data: video recordings, camera snapshots, logs, firmware binaries.

| Provider | Offering |
|---|---|
| AWS | Amazon S3 |
| GCP | Google Cloud Storage |
| Azure | Azure Blob Storage |

---

## Monitoring System — Metrics Collection

Scrapes and stores time-series metrics from all platform services (equivalent to Prometheus in the diagram).

| Provider | Offering |
|---|---|
| AWS | Amazon Managed Service for Prometheus, CloudWatch Metrics |
| GCP | Cloud Monitoring (formerly Stackdriver) |
| Azure | Azure Monitor, Azure Managed Grafana |
| Self-hosted | Prometheus |

---

## Monitoring System — Dashboards / Visualization

Visualizes platform health, throughput, and business metrics (equivalent to Grafana in the diagram).

| Provider | Offering |
|---|---|
| AWS | Amazon Managed Grafana, CloudWatch Dashboards |
| GCP | Cloud Monitoring dashboards, Looker |
| Azure | Azure Managed Grafana, Azure Monitor Workbooks |
| Self-hosted | Grafana |

---

## Monitoring System — Alert Manager

Routes alerts from metric thresholds or anomalies to on-call channels (email, Slack, PagerDuty, etc.).

| Provider | Offering |
|---|---|
| AWS | Amazon SNS + CloudWatch Alarms |
| GCP | Cloud Monitoring Alerting Policies |
| Azure | Azure Monitor Action Groups |
| Self-hosted | Prometheus Alertmanager |

---

## Monitoring System — Business Metrics

Tracks product-level KPIs: active devices, daily active users, module adoption, revenue signals.

| Provider | Offering |
|---|---|
| AWS | Amazon QuickSight, CloudWatch custom metrics |
| GCP | Looker, BigQuery + Data Studio |
| Azure | Power BI, Azure Synapse Analytics |
| Self-hosted | Grafana + custom Prometheus metrics |