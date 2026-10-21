# Reference I: ISO/IEC 42001:2023 Applicability (design-level)

ISO/IEC 42001 is the standard for an **AI management system (AIMS)**: how an organisation governs the AI it builds or uses. This project uses 42001 as its ISO standard. ISO/IEC 27001 (information security) is a separate standard that can be used alongside it; 42001 Annex D notes this. The 27001 mapping is kept as an optional appendix in [B](./B-iso-27001-statement-of-applicability.md) and is not part of the core design.

This table covers the 38 Annex A controls for **Halden Assist**. Control numbers and titles come from public listings and should be checked against the standard; the standard's text is copyrighted, so "Why it matters here" is my own wording. 42001 also has management-system clauses 4 to 10, not assessed here, except that the risk register and guardrail G-42 are my design-level answer to the AI risk assessment (clause 6.1.2), AI risk treatment (6.1.3) and AI system impact assessment (6.1.4) requirements.

- **Included: agent-specific:** at least one guardrail contributes to the control.
- **Included: organisation-level AIMS:** an organisation-wide AIMS process applies, with no guardrail written specially for the agent.
- **Excluded:** not relevant to this system, with the reason.

| Status | Count |
|---|---|
| Included: agent-specific | 35 |
| Included: organisation-level AIMS | 2 |
| Excluded | 1 |


**Guardrails with no 42001 mapping.** 10 guardrails have no direct 42001 Annex A counterpart (G-01, G-02, G-04, G-05, G-12, G-23, G-24, G-38, G-39, G-45). They are classic security controls (authentication, authorisation, network segmentation, message integrity), which 42001 Annex A does not address directly. They still map to NIST AI RMF, the EU AI Act (mainly Article 15) and OWASP. The other 36 map to 42001, mostly in AI life cycle (A.6), use of AI (A.9), data (A.7) and impact assessment (A.5).


## A.2. Policies related to AI

| Control | Title | Status | Why it matters here | Guardrails |
|---|---|---|---|---|
| A.2.2 | AI policy | Included: organisation-level AIMS | The firm's AI policy sets principles for developing and using AI; this agent must be covered by it. | - |
| A.2.3 | Alignment with other organizational policies | Included: agent-specific | The AI policy is kept consistent with security, privacy and conduct policies; the legal register supports this. | G-41 |
| A.2.4 | Review of the AI policy | Included: organisation-level AIMS | The AI policy is reviewed on a schedule and after significant change. | - |


## A.3. Internal organization

| Control | Title | Status | Why it matters here | Guardrails |
|---|---|---|---|---|
| A.3.2 | AI roles and responsibilities | Included: agent-specific | Owner, approver and oversight roles for the agent are assigned. | G-09, G-43 |
| A.3.3 | Reporting of concerns | Included: agent-specific | Staff and customers can raise concerns about the agent's behaviour. | G-33, G-34 |


## A.4. Resources for AI systems

| Control | Title | Status | Why it matters here | Guardrails |
|---|---|---|---|---|
| A.4.2 | Resource documentation | Included: agent-specific | The agent's components, identities, models, tools and data sources are documented. | G-03, G-25, G-40, G-44 |
| A.4.3 | Data resources | Included: agent-specific | Data the agent uses (knowledge sources, session memory, evaluation data) is identified and controlled. | G-21, G-22 |
| A.4.4 | Tooling resources | Included: agent-specific | Tools and tool servers the agent can call are identified and controlled. | G-25 |
| A.4.5 | System and computing resources | Included: agent-specific | Compute and capacity are planned, and limits protect them. | G-27, G-28 |
| A.4.6 | Human resources | Included: agent-specific | People who build, approve and oversee the agent have the competence and trust the role needs. | G-43, G-46 |


## A.5. Assessing impacts of AI systems

| Control | Title | Status | Why it matters here | Guardrails |
|---|---|---|---|---|
| A.5.2 | AI system impact assessment process | Included: agent-specific | A defined process assesses what the agent could do to people and society. | G-42 |
| A.5.3 | Documentation of AI system impact assessments | Included: agent-specific | Impact assessment results are recorded. | G-42 |
| A.5.4 | Assessing AI system impact on individuals or groups of individuals | Included: agent-specific | Effects on customers (wrong information, privacy, manipulation, unequal service) are assessed. | G-16, G-42 |
| A.5.5 | Assessing societal impacts of AI systems | Included: agent-specific | Wider effects are considered; for a narrow service agent they are small, and the assessment says so. | G-42 |


## A.6. AI system life cycle

| Control | Title | Status | Why it matters here | Guardrails |
|---|---|---|---|---|
| A.6.1.2 | Objectives for responsible development of AI system | Included: agent-specific | Stated objectives (for example fairness, safety, transparency) guide how the agent is built. | G-16 |
| A.6.1.3 | Processes for responsible AI system design and development | Included: agent-specific | Design and build steps include the guardrails and reviews. | G-37 |
| A.6.2.2 | AI system requirements and specification | Included: agent-specific | What the agent must and must not do is specified, including limits and the refusal scope. | G-06, G-08, G-11, G-13, G-14, G-40 |
| A.6.2.3 | Documentation of AI system design and development | Included: agent-specific | Design choices and changes are recorded. | G-36, G-37, G-44 |
| A.6.2.4 | AI system verification and validation | Included: agent-specific | Evaluation and tests show the agent behaves as specified before release. | G-13, G-17, G-35 |
| A.6.2.5 | AI system deployment | Included: agent-specific | Release follows a gated, documented process. | G-35, G-36 |
| A.6.2.6 | AI system operation and monitoring | Included: agent-specific | Behaviour is monitored in production, with ability to intervene. | G-09, G-28, G-31, G-32, G-34 |
| A.6.2.7 | AI system technical documentation | Included: agent-specific | Technical documentation is kept current. | G-44 |
| A.6.2.8 | AI system recording of event logs | Included: agent-specific | Events are logged so behaviour can be reconstructed. | G-29, G-30 |


## A.7. Data for AI systems

| Control | Title | Status | Why it matters here | Guardrails |
|---|---|---|---|---|
| A.7.2 | Data for development and enhancement of AI system | Included: agent-specific | No model is trained or fine-tuned here; this applies to evaluation data and prompts used to improve the agent. | G-20, G-35 |
| A.7.3 | Acquisition of data | Excluded | No training data is acquired. Knowledge sources the agent answers from are covered under data provenance (A.7.5). | - |
| A.7.4 | Quality of data for AI systems | Included: agent-specific | Knowledge sources the agent relies on are accurate and current. | G-17 |
| A.7.5 | Data provenance | Included: agent-specific | The origin of the knowledge sources and evaluation data is recorded. | G-17 |
| A.7.6 | Data preparation | Included: agent-specific | Data is cleaned and redacted before it reaches the model. | G-20, G-21 |


## A.8. Information for interested parties of AI systems

| Control | Title | Status | Why it matters here | Guardrails |
|---|---|---|---|---|
| A.8.2 | System documentation and information for users | Included: agent-specific | Customers are told what the agent is and what it can do. | G-15, G-44 |
| A.8.3 | External reporting | Included: agent-specific | Obligations to report to regulators or other external parties are known and followed. | G-33 |
| A.8.4 | Communication of incidents | Included: agent-specific | Incidents are communicated to affected parties and authorities in time. | G-33 |
| A.8.5 | Information for interested parties | Included: agent-specific | Customers, staff and regulators get the information they need about the agent. | G-15, G-34 |


## A.9. Use of AI systems

| Control | Title | Status | Why it matters here | Guardrails |
|---|---|---|---|---|
| A.9.2 | Processes for responsible use of AI systems | Included: agent-specific | Rules and processes govern how the agent is used, including human approval and handoff. | G-09, G-10, G-18, G-19, G-32, G-43 |
| A.9.3 | Objectives for responsible use of AI systems | Included: agent-specific | Objectives for responsible use are set and measured. | G-16, G-34 |
| A.9.4 | Intended use of the AI system | Included: agent-specific | The agent is used only for its stated purpose; out-of-scope requests are refused. | G-06, G-07, G-14 |


## A.10. Third-party and customer relationships

| Control | Title | Status | Why it matters here | Guardrails |
|---|---|---|---|---|
| A.10.2 | Allocating responsibilities | Included: agent-specific | Responsibilities between the firm, the model provider and other suppliers are agreed. | G-26 |
| A.10.3 | Suppliers | Included: agent-specific | Suppliers of models, tools and services are assessed and monitored. | G-25, G-26 |
| A.10.4 | Customers | Included: agent-specific | The firm meets its responsibilities to the customers who use the agent. | G-15, G-19 |
