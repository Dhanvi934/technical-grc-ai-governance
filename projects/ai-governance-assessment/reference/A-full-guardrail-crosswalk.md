# Reference A: Full Guardrail Crosswalk

One crosswalk from **guardrails** (things the system does) to the three frameworks. Each row is a guardrail with an ID. The columns show which ISO/IEC 27001:2022 Annex A controls, EU AI Act articles and NIST AI RMF subcategories it helps to meet, and which OWASP Agentic Top 10 risk it reduces.

> **How to read the mapping.** "Maps to" means the guardrail *contributes evidence toward* that control or requirement. It does not mean the guardrail alone satisfies it. ISO 27001 controls are met by the management system as a whole; the EU AI Act articles marked *design target* are voluntary here (see [02](../02-eu-ai-act-classification.md)); NIST AI RMF is a voluntary framework. The mappings are my reading, not an official crosswalk.

Machine-readable copy: [`data/control-matrix.csv`](../data/control-matrix.csv). Components (C1 to C16) are defined in [01-system-profile.md](./G-full-system-profile.md).

## Coverage at a glance

| Framework | Items in framework | Mapped to a guardrail | Other handling |
|---|---|---|---|
| ISO/IEC 27001:2022 Annex A | 93 | 74 | 8 baseline ISMS control with no agent-specific guardrail; 8 inherited from providers; 3 excluded with reasons |
| NIST AI RMF 1.0 subcategories | 72 | 64 | 7 not covered (stated gaps); 1 organisation-level |
| EU AI Act (articles considered) | 25 | 21 | See [03b](./C-eu-ai-act-applicability.md) for which are applicable, design targets, conditional or not applicable |
| OWASP Agentic Top 10 (2026) | 10 | 10 | Every risk has at least one guardrail and one entry in the risk register |


**Reading the numbers.** A high mapped count in a design document shows the design has been thought through against the frameworks. It does not show the controls work. Every guardrail is at status *Designed*: nothing here is implemented, tested or audited. The `test` and `evidence` columns say what would have to be shown for the status to change.


## OWASP Agentic Top 10 coverage

| Risk | Name | Guardrails | Risks in register |
|---|---|---|---|
| ASI01 | Agent Goal Hijack | G-13, G-14, G-18, G-20, G-35 | R-01, R-02 |
| ASI02 | Tool Misuse and Exploitation | G-02, G-06, G-07, G-08, G-09, G-10, G-35 | R-03 |
| ASI03 | Identity & Privilege Abuse | G-01, G-02, G-03, G-04, G-05, G-31, G-46 | R-04, R-05 |
| ASI04 | Agentic Supply Chain Vulnerabilities | G-07, G-12, G-25, G-26, G-36, G-37 | R-06, R-20 |
| ASI05 | Unexpected Code Execution | G-08, G-11, G-12, G-37 | R-07 |
| ASI06 | Memory & Context Poisoning | G-13, G-21, G-22 | R-02, R-08 |
| ASI07 | Insecure Inter-Agent Communication | G-24 | R-09 |
| ASI08 | Cascading Failures | G-24, G-27, G-28, G-31, G-32, G-38 | R-10 |
| ASI09 | Human-Agent Trust Exploitation | G-09, G-10, G-15, G-16, G-17, G-19, G-43, G-44 | R-11 |
| ASI10 | Rogue Agents | G-03, G-05, G-29, G-30, G-31, G-32, G-33, G-34, G-36, G-39, G-40, G-41, G-42, G-45 | R-12, R-20 |


## Identity and access

| ID | Guardrail | Components | ISO 27001 Annex A | EU AI Act | NIST AI RMF | OWASP |
|---|---|---|---|---|---|---|
| G-01 | Authenticate the human at the edge | C2, C3 | 5.16, 5.17, 8.5 | Art 15 | MEASURE 2.7 | ASI03 |
| G-02 | Authorise against the user, not just the agent | C5, C9 | 5.15, 5.18, 8.3 | Art 15 | MEASURE 2.7, MAP 3.3 | ASI03, ASI02 |
| G-03 | One workload identity per agent, no shared credentials | C7, C8, C9 | 5.16, 5.2, 8.2 | Art 15 | GOVERN 1.6, GOVERN 2.1 | ASI03, ASI10 |
| G-04 | Short-lived, scoped credentials per tool server | C9, C10 | 5.17, 8.2, 8.24 | Art 15 | MEASURE 2.7 | ASI03 |
| G-05 | Privileged access to agent configuration | C7, C13, C16 | 8.2, 5.3, 8.18, 8.4 | Art 14 | GOVERN 2.1 | ASI03, ASI10 |


## Tools and actions

| ID | Guardrail | Components | ISO 27001 Annex A | EU AI Act | NIST AI RMF | OWASP |
|---|---|---|---|---|---|---|
| G-06 | Action risk tiering | C5, C8, C9, C16 | 5.15, 8.3 | Art 14, Art 9 | MAP 3.5, MAP 5.1, GOVERN 3.2 | ASI02 |
| G-07 | Tool allow-list per agent, deny by default | C8, C9 | 5.15, 8.3, 8.9 | Art 15 | MAP 3.3, MEASURE 2.7 | ASI02, ASI04 |
| G-08 | Server-side parameter validation and limits | C9, C10 | 8.26, 8.28 | Art 15 | MEASURE 2.7 | ASI02, ASI05 |
| G-09 | Human approval for Tier 3 actions | C16, C9 | 5.3, 5.2 | Art 14 | GOVERN 3.2, MAP 3.5, MANAGE 2.4 | ASI09, ASI02 |
| G-10 | Explicit customer confirmation for Tier 2 actions | C4, C8 | 8.26 | Art 14 | MAP 3.5, MEASURE 3.3 | ASI09, ASI02 |
| G-11 | No code execution by default | C8, C9 | 8.19, 8.22, 8.31 | Art 15 | MEASURE 2.7 | ASI05 |
| G-12 | Egress allow-list and network segmentation | C2, C9, C12 | 8.20, 8.21, 8.22, 8.23, 8.31 | Art 15 | MEASURE 2.7 | ASI05, ASI04 |


## Content and model

| ID | Guardrail | Components | ISO 27001 Annex A | EU AI Act | NIST AI RMF | OWASP |
|---|---|---|---|---|---|---|
| G-13 | Prompt-injection and untrusted-content defences | C7, C8, C11 | 8.26, 8.28, 8.29 | Art 15 | MEASURE 2.7, MEASURE 2.6 | ASI01, ASI06 |
| G-14 | Scope boundary and refusal of regulated decisions | C7, C8 | 5.31 | Art 6, Art 5 | MAP 1.1, MAP 3.3, GOVERN 1.1 | ASI01 |
| G-15 | Disclose that the customer is talking to AI | C1, C7 | 5.31 | Art 50(1) | GOVERN 1.4, MEASURE 2.8 | ASI09 |
| G-16 | No manipulation or pressure | C7, C13 | 5.10 | Art 5, Art 50(3) | MAP 1.1 | ASI09 |
| G-17 | Grounded answers and confident handoff | C7, C8, C13 | 8.29 | Art 15, Art 13 | MEASURE 2.5, MEASURE 2.9, MEASURE 2.3 | ASI09 |
| G-18 | Output filtering | C7, C6 | 8.12, 5.14 | Art 15 | MEASURE 2.10, MEASURE 2.6 | ASI01 |
| G-19 | Human handoff at any time | C16, C7 | 5.37 | Art 14 | MEASURE 3.3, MAP 3.5, MANAGE 2.1 | ASI09 |


## Data

| ID | Guardrail | Components | ISO 27001 Annex A | EU AI Act | NIST AI RMF | OWASP |
|---|---|---|---|---|---|---|
| G-20 | PII redaction before model and in logs | C6, C12, C15 | 5.34, 8.11, 8.12 | Art 10 | MEASURE 2.10 | ASI01 |
| G-21 | Data minimisation for the context window | C4, C8, C11 | 5.34, 8.3 | Art 10 | MEASURE 2.10 | ASI06 |
| G-22 | Session and memory hygiene | C11 | 8.3, 8.10, 5.12, 5.33 | Art 15 | MEASURE 2.7, MEASURE 2.10 | ASI06 |
| G-23 | Cryptography for data in transit and at rest | C2, C11, C12, C15 | 8.24 | Art 15 | MEASURE 2.7 | - |


## Agents and supply chain

| ID | Guardrail | Components | ISO 27001 Annex A | EU AI Act | NIST AI RMF | OWASP |
|---|---|---|---|---|---|---|
| G-24 | Authenticated, validated inter-agent messages | C7, C8, C11 | 8.26, 8.22 | Art 15 | MEASURE 2.7 | ASI07, ASI08 |
| G-25 | Agentic supply-chain control | C7, C9, C12 | 5.21, 8.19, 8.30 | Art 25, Art 15 | GOVERN 6.1, MAP 4.1, MAP 4.2, MANAGE 3.2 | ASI04 |
| G-26 | Third-party model provider management | C12, ORG | 5.19, 5.20, 5.22, 5.23 | Art 25 | GOVERN 6.1, GOVERN 6.2, MANAGE 3.1, MAP 4.1 | ASI04 |
| G-27 | Cost, loop and rate limits | C2, C14 | 8.6, 8.20 | Art 15 | MEASURE 2.7, MANAGE 2.3 | ASI08 |
| G-28 | Fallback when the model fails | C7, C12, C16 | 5.29, 5.30, 8.14 | Art 15 | GOVERN 6.2, MANAGE 2.3 | ASI08 |


## Monitoring and response

| ID | Guardrail | Components | ISO 27001 Annex A | EU AI Act | NIST AI RMF | OWASP |
|---|---|---|---|---|---|---|
| G-29 | Structured, tamper-evident action log | C15 | 8.15, 5.28, 5.33 | Art 12, Art 19 | MEASURE 2.4, MANAGE 4.1 | ASI10 |
| G-30 | Log retention and clock synchronisation | C15 | 8.15, 8.17, 5.33, 5.31 | Art 26, Art 19 | MANAGE 4.1 | ASI10 |
| G-31 | SIEM detection and SOAR response for agent behaviour | C15, C16 | 8.16, 5.7, 5.25 | Art 72, Art 26 | MEASURE 2.4, MANAGE 4.1 | ASI10, ASI03, ASI08 |
| G-32 | Kill switch and agent suspension | C16, C15, C3 | 5.26, 8.2 | Art 14, Art 26 | MANAGE 2.4, MANAGE 2.3 | ASI10, ASI08 |
| G-33 | Incident response runbook for agent incidents | ORG, C15 | 5.24, 5.25, 5.26, 5.27, 5.5, 6.8 | Art 73, Art 26 | MANAGE 4.3, GOVERN 4.3, MANAGE 1.3 | ASI10 |
| G-34 | Post-deployment monitoring and customer feedback | C14, C15, C16 | 5.27 | Art 72 | MANAGE 4.1, MANAGE 4.2, MEASURE 3.1, MEASURE 4.3, MEASURE 3.2, MEASURE 4.2, GOVERN 5.1, GOVERN 5.2 | ASI10 |


## Lifecycle

| ID | Guardrail | Components | ISO 27001 Annex A | EU AI Act | NIST AI RMF | OWASP |
|---|---|---|---|---|---|---|
| G-35 | Pre-release evaluation gate | C13 | 8.29, 8.25, 8.31 | Art 9, Art 15 | MEASURE 1.1, MEASURE 1.2, MEASURE 2.1, MEASURE 2.3, MEASURE 2.6, MEASURE 2.7, MEASURE 2.13, MAP 2.3, MEASURE 4.1 | ASI01, ASI02 |
| G-36 | Change management for prompts, models and tools | C7, C9, C12, C13 | 8.32, 8.9, 5.37 | Art 17 | MANAGE 4.2, MEASURE 4.3 | ASI10, ASI04 |
| G-37 | Secure development and vulnerability management | C7, C9, C13 | 8.8, 8.25, 8.28, 8.4, 8.33 | Art 15 | MEASURE 2.7 | ASI05, ASI04 |
| G-38 | Backup and recovery of agent configuration and memory | C11, C7 | 8.13, 5.30 | Art 15 | MANAGE 2.3 | ASI08 |
| G-39 | Decommissioning | ORG, C11 | 8.10, 5.11 | Art 18 | GOVERN 1.7 | ASI10 |


## Governance

| ID | Guardrail | Components | ISO 27001 Annex A | EU AI Act | NIST AI RMF | OWASP |
|---|---|---|---|---|---|---|
| G-40 | AI system inventory and classification record | ORG | 5.9, 5.1 | Art 6, Art 49 | GOVERN 1.6, MAP 1.1, MAP 1.3, MAP 3.3 | ASI10 |
| G-41 | Legal and regulatory register | ORG | 5.31, 5.36, 5.32 | Art 4, Art 95 | GOVERN 1.1, GOVERN 1.2, GOVERN 1.4 | ASI10 |
| G-42 | Risk assessment, treatment and acceptance | ORG | 5.4, 5.8 | Art 9, Art 27 | MAP 1.5, MAP 5.1, MANAGE 1.1, GOVERN 4.2, MANAGE 1.2, MANAGE 1.3, MANAGE 1.4, GOVERN 1.3, GOVERN 1.4, GOVERN 2.3 | ASI10 |
| G-43 | Roles, human-oversight competence and AI literacy | ORG, C16 | 5.2, 6.3 | Art 4, Art 14, Art 26 | GOVERN 2.1, GOVERN 2.2, GOVERN 3.1, MAP 3.4, MAP 3.5, MAP 1.2 | ASI09 |
| G-44 | Documentation and transparency to the people who operate it | ORG | 5.37 | Art 11, Art 13 | MAP 2.1, MAP 2.2, MEASURE 2.8, MAP 1.6, MAP 3.2 | ASI09 |
| G-45 | Independent review and internal audit | ORG | 5.35, 5.36 | Art 17 | MEASURE 1.3, MEASURE 2.13, GOVERN 1.5 | ASI10 |
| G-46 | Staff and contractor access | ORG | 6.1, 6.2, 6.4, 6.5, 6.6, 8.1, 6.7 | Art 15 | GOVERN 2.1 | ASI03 |


## Detailed tables

- [B: ISO/IEC 27001 Statement of Applicability (all 93 controls)](./B-iso-27001-statement-of-applicability.md)
- [C: EU AI Act applicability](./C-eu-ai-act-applicability.md)
- [D: NIST AI RMF applicability (all 72 subcategories)](./D-nist-ai-rmf-applicability.md)
