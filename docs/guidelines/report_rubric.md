# Project Overview
[Architectural Katas](https://www.architecturalkatas.com/kata.html?kata=all) are small-group design exercises in which each team works on a different real-world problem scenario (“kata”). During the Discussion Phase, your project team will select a kata from this [list](https://www.architecturalkatas.com/kata.html?kata=all), analyze its requirements, and develop a high-level vision for a cloud-based architecture that satisfies those requirements.

You are required to design a cloud-native solution and be prepared to justify and defend your architectural decisions during the Presentation Phase. The goal of this project is not to produce a single “correct” architecture, but to demonstrate sound cloud computing reasoning, tradeoff analysis, and alignment with industry best practices.

You may safely make assumptions about technologies or workload characteristics that you do not know well. However, all assumptions must be explicitly stated and justified, and your solution must remain internally consistent with those assumptions.

## Project Deliverable - Report Format and Submission Requirements
The architectural report must be submitted as a single PDF document via Canvas by the due date.

The report must adhere to the following formatting requirements:

- **Length:** The report must be at least 15 pages and no more than 20 pages, excluding the cover page, abstract, table of contents, lists of figures and tables, and references.
- **Font and Size**: Use Calibri (Body) font, 12-point size for all body text and 14-point size for section header
- **Spacing**: Use 1.5 line spacing throughout the document.
- **Margins**: Use standard margins (approximately 1 inch on all sides).
- **Figures and Tables**: All figures and tables must be clearly labeled and referenced in the text.
- **File Format**: The final submission must be uploaded as a PDF file. Other file formats will not be accepted.

## Writing Style and Expectations
This report is a professional architectural design document. As such, sections should read as cohesive explanations rather than isolated responses to prompts.

- Each required section must include explanatory paragraphs that describe your reasoning, design decisions, and tradeoffs.
- Bullet points may be used sparingly to summarize key ideas, list assumptions, or highlight decisions, but bullet points alone are not sufficient.
- Sections composed primarily of bullet lists without supporting narrative will not demonstrate adequate architectural reasoning.
- Section headers may be used to organize content, but each section must contain substantive narrative discussion.

**Your writing should explain:**
- Why architectural decisions were made
- How the design satisfies requirements and quality attributes
- What tradeoffs were considered

The emphasis is on **clarity of reasoning and justification**, not on producing a checklist of concepts. Submissions that rely primarily on bullet points in place of explanatory narrative may receive reduced credit.

## Required Report Sections
Your report must include all of the following sections.

### 1. Introduction and Solution Overview
- Brief description of the selected kata and its problem domain
- Summary of the functional and non-functional requirements
- High-level overview of the proposed cloud-based solution

### 2. Assumptions and Constraints
- Explicit assumptions (e.g., expected traffic, data volume, geographic scope)
- Constraints such as budget, latency, compliance, or timeline
- Justification for why these assumptions are reasonable

### 3. Use Cases and Actors
- Identification of primary and secondary actors
- Key system use cases and interactions
- Explanation of how the architecture supports these use cases

### 4. System Architecture Diagram
- A clear and readable architecture diagram
- Cloud services labeled and logically grouped
- Major data flows and integrations shown
The diagram should reflect design decisions related to scalability, availability, and fault tolerance.

### 5. Cloud Architecture Quality Attributes (“-ilities”)
Identify and prioritize the most relevant quality attributes for your system, such as:
- Scalability
- Availability
- Fault tolerance
- Durability
- Security
- Performance
- Cost efficiency

For additional examples of architectural quality attributes (often referred to as “-ilities”), see the Wikipedia page on [system quality attributes](https://en.wikipedia.org/wiki/List_of_system_quality_attributes). This list can help you identify relevant quality attributes for your project beyond those explicitly mentioned in the assignment.

For each selected attribute:
- Explain why it is important for this kata
- Describe how your architecture supports it
- Discuss any tradeoffs involved

### 6. Cloud-Native Application Design (Twelve-Factor App)
This section evaluates how your proposed solution aligns with the [Twelve-Factor App methodology](https://12factor.net/) for cloud-native applications.

You are not required to implement all twelve factors. Instead, you should:

Identify the factors most relevant to your design
Explain how your architecture aligns with those factors
Clearly state which factors are not applicable and justify why
At minimum, your discussion should address several of the following (as applicable):

**Twelve-Factor App Principles to Consider**

1. Codebase – A single, version-controlled codebase that supports multiple deployments
2. Dependencies – Dependencies are explicitly declared and isolated from the system
3. Config – Configuration is stored in the environment rather than in code
44. Backing Services – External resources (e.g., databases, queues, storage) are treated as attached services
5. Build, Release, Run – Clear separation between build, release, and runtime stages
6. Processes – The application runs as one or more stateless processes that can scale horizontally
7. Port Binding – Services are exposed via explicit port binding
8. Concurrency – The application scales out by running multiple processes/instances rather than scaling up a single instance
9. Disposability – Fast startup and graceful shutdown to improve resilience and fault tolerance
10. Dev/Prod Parity – Development, staging, and production environments are kept as similar as possible
11. Logs – Logs are treated as event streams rather than local files
12. Admin Processes – Administrative/management tasks are run as one-off, separate processes

**Important**: Your evaluation should focus on architectural reasoning and tradeoffs, not implementation details.

### 7. Well-Architected Framework Alignment
Discuss how your solution aligns with an industry well-architected framework (e.g., AWS Well-Architected FrameworkLinks to an external site. or an equivalent, based on your assumed cloud provider).

At a minimum, address:

- Operational excellence
- Security
- Reliability
- Performance efficiency
- Cost optimization
- Sustainability
You are not expected to optimize all pillars equally. Clear prioritization and justification of tradeoffs are expected.

### 8. Cloud Services Selection and Justification
- List the major cloud services used in your architecture
- Explain why each service was chosen over alternatives
- Discuss advantages, limitations, and managed vs. self-managed tradeoffs

### 9. Scalability, Availability, and Fault Tolerance
Explain how your architecture:

- Scales as demand increases
- Handles component, service, or infrastructure failures
- Protects data availability and durability
Include examples such as redundancy strategies, stateless components, or backup and recovery approaches.

### 10. Cost Considerations
Provide a high-level discussion of cost, including:

- Major cost drivers in your architecture
- How costs are expected to scale with usage
- Tradeoffs between cost, performance, and reliability

Exact pricing accuracy is not required; sound reasoning is. You may use the AWS Pricing Calculator to guide your estimates, but you are not required to produce an exact quote.

### 11. Risks and Mitigation Strategies
- Identification of key technical or operational risks
- Mitigation strategies for each risk
- Discussion of remaining (residual) risk

### 12. Conclusion
- Summary of your architectural approach
- Key strengths of the proposed solution
- Known limitations and potential future improvements

### 13. References
- List all references used (cloud documentation, frameworks, articles, etc.)
- Include references on a separate page at the end of the report

Important Notes
- This is an architecture and reasoning exercise, not an implementation or deployment project.
- You are not required to build or deploy the system.
- There is no single correct solution. Clear justification and defensible tradeoffs are the primary evaluation criteria.