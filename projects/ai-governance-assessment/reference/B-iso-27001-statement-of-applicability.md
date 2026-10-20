# Reference B: ISO/IEC 27001:2022 Statement of Applicability (design-level)

All 93 Annex A controls, scoped to **Halden Assist** (the customer service agent described in [G](./G-full-system-profile.md)). A real Statement of Applicability covers the whole ISMS scope and is approved by management; this one shows how the same exercise looks for one AI system.

- **Included: agent-specific** means at least one guardrail contributes to the control (see the guardrail IDs).
- **Included: baseline ISMS** means the organisation's existing control applies to the system, with no guardrail written specially for the agent.
- **Inherited from provider** means the cloud or model provider operates the control and the evidence is their attestation (for example an ISO 27001 certificate or SOC 2 report), which the firm must review.
- **Excluded** means not relevant to this system, with the reason.

Control titles follow the official ISO list (checked against a public listing). The "Why it matters here" column is my own wording; ISO's control text is copyrighted and is not reproduced. Buy the standard to read the real requirements.

The ISMS also requires the management-system clauses (4 to 10: context, leadership, planning including risk assessment and the SoA itself, support, operation, performance evaluation, improvement). This document covers Annex A only.

| Status | Count |
|---|---|
| Included: agent-specific | 74 |
| Included: baseline ISMS | 8 |
| Inherited from provider | 8 |
| Excluded | 3 |


## 5. Organisational controls

| Control | Title | Status | Why it matters here | Guardrails |
|---|---|---|---|---|
| 5.1 | Policies for information security | Included: agent-specific | Security policy set is extended to name AI agents, their owners and acceptable behaviour. | G-40 |
| 5.2 | Information security roles and responsibilities | Included: agent-specific | Owner, approver and oversight roles for the agent are named. | G-03, G-09, G-43 |
| 5.3 | Segregation of duties | Included: agent-specific | Tier 3 approver is a different person from whoever can change policy or prompts. | G-05, G-09 |
| 5.4 | Management responsibilities | Included: agent-specific | Management accepts residual risk and backs oversight resourcing. | G-42 |
| 5.5 | Contact with authorities | Included: agent-specific | Incident runbook lists who contacts which authority and when. | G-33 |
| 5.6 | Contact with special interest groups | Included: baseline ISMS | Staff follow OWASP, CSA and regulator updates on agent threats. | - |
| 5.7 | Threat intelligence | Included: agent-specific | Agent-specific threat reports (injection techniques, malicious tool servers) feed detection rules. | G-31 |
| 5.8 | Information security in project management | Included: agent-specific | Security and risk steps are part of the agent's delivery plan and gate. | G-42 |
| 5.9 | Inventory of information and other associated assets | Included: agent-specific | Agents, identities, prompts, tools, models and stores are inventoried. | G-40 |
| 5.10 | Acceptable use of information and other associated assets | Included: agent-specific | Rules for what the agent may be asked and may do; rules for staff use of the console. | G-16 |
| 5.11 | Return of assets | Included: agent-specific | Applies when staff or contractors leave, and at decommissioning. | G-39 |
| 5.12 | Classification of information | Included: agent-specific | Data in the agent's context is classified so redaction and logging rules can follow. | G-22 |
| 5.13 | Labelling of information | Included: baseline ISMS | Handled by the firm's existing labelling scheme; no agent-specific guardrail. | - |
| 5.14 | Information transfer | Included: agent-specific | Data sent to the model provider and to customers follows approved transfer rules. | G-18 |
| 5.15 | Access control | Included: agent-specific | Access rules are set per tool and per tier, enforced in C5. | G-02, G-06, G-07 |
| 5.16 | Identity management | Included: agent-specific | Every agent has a managed non-human identity with a lifecycle. | G-01, G-03 |
| 5.17 | Authentication information | Included: agent-specific | Tokens and secrets for agents are issued, rotated and protected. | G-01, G-04 |
| 5.18 | Access rights | Included: agent-specific | Rights are granted, reviewed and removed for agents and for staff. | G-02 |
| 5.19 | Information security in supplier relationships | Included: agent-specific | Model, cloud and tool providers are assessed for security. | G-26 |
| 5.20 | Addressing information security within supplier agreements | Included: agent-specific | Contract terms for the model provider cover data use, retention and incidents. | G-26 |
| 5.21 | Managing information security in the ICT supply chain | Included: agent-specific | Models, tool servers and libraries are pinned and verified. | G-25 |
| 5.22 | Monitoring, review and change management of supplier services | Included: agent-specific | Provider changes (including model updates) are reviewed before they reach customers. | G-26 |
| 5.23 | Information security for use of cloud services | Included: agent-specific | Agent runtime, store and model gateway sit on cloud services; shared responsibility is documented. | G-26 |
| 5.24 | Information security incident management planning and preparation | Included: agent-specific | Agent-specific incident plan and roles. | G-33 |
| 5.25 | Assessment and decision on information security events | Included: agent-specific | Alerts from agent monitoring are triaged against defined criteria. | G-31, G-33 |
| 5.26 | Response to information security incidents | Included: agent-specific | Containment includes suspending agents and revoking credentials. | G-32, G-33 |
| 5.27 | Learning from information security incidents | Included: agent-specific | Incidents and near misses feed new tests, rules and guardrails. | G-33, G-34 |
| 5.28 | Collection of evidence | Included: agent-specific | Logs are kept in a form that supports investigation. | G-29 |
| 5.29 | Information security during disruption | Included: agent-specific | Fallback keeps security controls when the model or a provider is unavailable. | G-28 |
| 5.30 | ICT readiness for business continuity | Included: agent-specific | Recovery of agent configuration and service continuity are tested. | G-28, G-38 |
| 5.31 | Legal, statutory, regulatory and contractual requirements | Included: agent-specific | Register covers AI Act, GDPR, DORA, NIS2 and consumer rules. | G-14, G-15, G-30, G-41 |
| 5.32 | Intellectual property rights | Included: agent-specific | Licences for models, tools and training material in use are respected. | G-41 |
| 5.33 | Protection of records | Included: agent-specific | Logs and memory are retained, protected and disposed of on schedule. | G-22, G-29, G-30 |
| 5.34 | Privacy and protection of personal identifiable information (PII) | Included: agent-specific | Redaction, minimisation and provider terms protect customer data. | G-20, G-21 |
| 5.35 | Independent review of information security | Included: agent-specific | Independent review of agent controls at least yearly. | G-45 |
| 5.36 | Compliance with policies, rules and standards for information security | Included: agent-specific | Regular checks that agent controls follow policy. | G-41, G-45 |
| 5.37 | Documented operating procedures | Included: agent-specific | Runbooks for approval, handoff, kill switch and change. | G-19, G-36, G-44 |


## 6. People controls

| Control | Title | Status | Why it matters here | Guardrails |
|---|---|---|---|---|
| 6.1 | Screening | Included: agent-specific | Staff and contractors with access to the agent system are screened. | G-46 |
| 6.2 | Terms and conditions of employment | Included: agent-specific | Employment terms include security duties for those operating the agent. | G-46 |
| 6.3 | Information security awareness, education and training | Included: agent-specific | Training covers how agents fail and how to oversee them. | G-43 |
| 6.4 | Disciplinary process | Included: agent-specific | Misuse of approver or administrator rights has consequences. | G-46 |
| 6.5 | Responsibilities after termination or change of employment | Included: agent-specific | Access and duties end or change when roles do. | G-46 |
| 6.6 | Confidentiality or non-disclosure agreements | Included: agent-specific | Applies to staff and contractors who see customer conversations. | G-46 |
| 6.7 | Remote working | Included: agent-specific | Approvers and engineers working remotely use managed devices and secure access. | G-46 |
| 6.8 | Information security event reporting | Included: agent-specific | Staff report suspicious agent behaviour through one channel. | G-33 |


## 7. Physical controls

| Control | Title | Status | Why it matters here | Guardrails |
|---|---|---|---|---|
| 7.1 | Physical security perimeters | Inherited from provider | Hosting is in provider data centres; evidence is the provider's attestation. | - |
| 7.2 | Physical entry | Inherited from provider | Provider data centre entry controls. | - |
| 7.3 | Securing offices, rooms and facilities | Included: baseline ISMS | Firm offices where approvers and engineers work. | - |
| 7.4 | Physical security monitoring | Inherited from provider | Provider data centre monitoring; firm offices under the existing scheme. | - |
| 7.5 | Protecting against physical and environmental threats | Inherited from provider | Provider responsibility. | - |
| 7.6 | Working in secure areas | Excluded | No secure-area working is part of the agent's operation; staff work in standard office or remote settings. | - |
| 7.7 | Clear desk and clear screen | Included: baseline ISMS | Approvers see customer data on screen; clear-screen rules apply. | - |
| 7.8 | Equipment siting and protection | Inherited from provider | Provider responsibility for servers; firm devices follow the existing scheme. | - |
| 7.9 | Security of assets off-premises | Included: baseline ISMS | Staff laptops used for approvals and administration. | - |
| 7.10 | Storage media | Excluded | No removable media is used to operate or support the agent; the organisation-wide policy covers the rest. | - |
| 7.11 | Supporting utilities | Inherited from provider | Provider responsibility. | - |
| 7.12 | Cabling security | Excluded | No firm-owned cabling is in scope; the provider covers its own. | - |
| 7.13 | Equipment maintenance | Inherited from provider | Provider responsibility for hosting equipment. | - |
| 7.14 | Secure disposal or re-use of equipment | Inherited from provider | Provider handles hosting equipment; firm devices follow the existing scheme. | - |


## 8. Technological controls

| Control | Title | Status | Why it matters here | Guardrails |
|---|---|---|---|---|
| 8.1 | User endpoint devices | Included: agent-specific | Approver and engineer devices are managed and protected. | G-46 |
| 8.2 | Privileged access rights | Included: agent-specific | Administration of agents, prompts and approval rules is privileged and controlled. | G-03, G-04, G-05, G-32 |
| 8.3 | Information access restriction | Included: agent-specific | Access to data is limited by tool, tier and user. | G-02, G-06, G-07, G-21, G-22 |
| 8.4 | Access to source code | Included: agent-specific | Prompts, tool code and policies are in access-controlled repositories. | G-05, G-37 |
| 8.5 | Secure authentication | Included: agent-specific | Customers and staff use MFA; step-up for sensitive actions. | G-01 |
| 8.6 | Capacity management | Included: agent-specific | Rate limits and budgets protect capacity. | G-27 |
| 8.7 | Protection against malware | Included: baseline ISMS | Endpoint and upload scanning under the existing scheme; relevant if customers attach files. | - |
| 8.8 | Management of technical vulnerabilities | Included: agent-specific | Agent stack is scanned and patched. | G-37 |
| 8.9 | Configuration management | Included: agent-specific | Agent, tool and network configuration is baselined and under version control. | G-07, G-36 |
| 8.10 | Information deletion | Included: agent-specific | Memory and logs are deleted on schedule; customer deletion requests are honoured. | G-22, G-39 |
| 8.11 | Data masking | Included: agent-specific | Tokenised identifiers before models and in logs. | G-20 |
| 8.12 | Data leakage prevention | Included: agent-specific | Output filtering and egress limits reduce leakage. | G-18, G-20 |
| 8.13 | Information backup | Included: agent-specific | Configuration and memory backups with restore tests. | G-38 |
| 8.14 | Redundancy of information processing facilities | Included: agent-specific | Fallback path keeps customers served when a model path fails. | G-28 |
| 8.15 | Logging | Included: agent-specific | Every agent action logged with enough detail to reconstruct it. | G-29, G-30 |
| 8.16 | Monitoring activities | Included: agent-specific | Detection rules watch agent behaviour, not just infrastructure. | G-31 |
| 8.17 | Clock synchronization | Included: agent-specific | One trusted time source across components so logs correlate. | G-30 |
| 8.18 | Use of privileged utility programs | Included: agent-specific | Administrative tools used on the agent platform are restricted and logged. | G-05 |
| 8.19 | Installation of software on operational systems | Included: agent-specific | Only approved, verified software, models and tools go to production. | G-11, G-25 |
| 8.20 | Networks security | Included: agent-specific | Segmentation and filtering around agents, tool servers and backends. | G-12, G-27 |
| 8.21 | Security of network services | Included: agent-specific | Network services such as the model gateway have agreed security terms. | G-12 |
| 8.22 | Segregation of networks | Included: agent-specific | Agents, tool servers and backends sit in separate segments. | G-11, G-12, G-24 |
| 8.23 | Web filtering | Included: agent-specific | Egress allow-list also blocks unapproved web destinations from the agent runtime. | G-12 |
| 8.24 | Use of cryptography | Included: agent-specific | Encryption in transit and at rest; managed keys. | G-04, G-23 |
| 8.25 | Secure development life cycle | Included: agent-specific | Secure development steps for agent and tool code. | G-35, G-37 |
| 8.26 | Application security requirements | Included: agent-specific | Requirements for validation, authorisation and message handling in the agent application. | G-08, G-10, G-13, G-24 |
| 8.27 | Secure system architecture and engineering principles | Included: baseline ISMS | The architecture follows least privilege, separation and defence in depth (see 01-system-and-scope). | - |
| 8.28 | Secure coding | Included: agent-specific | Coding standards and review for tool servers and agent code. | G-08, G-13, G-37 |
| 8.29 | Security testing in development and acceptance | Included: agent-specific | Injection, scope and abuse tests before release. | G-13, G-17, G-35 |
| 8.30 | Outsourced development | Included: agent-specific | Applies if any agent or tool code is built by contractors. | G-25 |
| 8.31 | Separation of development, test and production environments | Included: agent-specific | Separate environments; no production data in test. | G-11, G-12, G-35 |
| 8.32 | Change management | Included: agent-specific | Reviewed, tested, reversible changes to prompts, models and tools. | G-36 |
| 8.33 | Test information | Included: agent-specific | Synthetic data is used for evaluation instead of real customer data. | G-37 |
| 8.34 | Protection of information systems during audit testing | Included: baseline ISMS | Audits and red-team tests of the agent are planned so they do not harm production. | - |
