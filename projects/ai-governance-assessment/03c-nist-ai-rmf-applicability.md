# 03c: NIST AI RMF 1.0 Applicability

All 72 subcategories across Govern, Map, Measure and Manage. Subcategory topics are my short paraphrases. NIST AI RMF is voluntary, so the question here is not "does it apply" but "does the design address it, and where does it not".

| Status | Count |
|---|---|
| Mapped to guardrails | 64 |
| Not covered (stated gap) | 7 |
| Organisation-level, no runtime guardrail | 1 |


The rows marked **not covered** are honest gaps in this design, not oversights hidden by a mapping. They are listed again in the README under limitations.


## Govern

| Subcategory | Topic | Status | Guardrails / note |
|---|---|---|---|
| GOVERN 1.1 | Legal and regulatory requirements are understood and managed | Mapped to guardrails | G-14, G-41 |
| GOVERN 1.2 | Trustworthy-AI characteristics are built into policy | Mapped to guardrails | G-41 |
| GOVERN 1.3 | Level of risk-management effort is set by risk | Mapped to guardrails | G-42 |
| GOVERN 1.4 | Risk management runs through transparent policies and controls | Mapped to guardrails | G-15, G-41, G-42 |
| GOVERN 1.5 | Ongoing monitoring and periodic review are planned | Mapped to guardrails | G-45 |
| GOVERN 1.6 | Inventory of AI systems | Mapped to guardrails | G-03, G-40 |
| GOVERN 1.7 | Safe decommissioning | Mapped to guardrails | G-39 |
| GOVERN 2.1 | Roles and responsibilities are documented | Mapped to guardrails | G-03, G-05, G-43, G-46 |
| GOVERN 2.2 | Staff are trained for their AI risk duties | Mapped to guardrails | G-43 |
| GOVERN 2.3 | Executive leadership owns AI risk decisions | Mapped to guardrails | G-42 |
| GOVERN 3.1 | Decision-making involves diverse expertise | Mapped to guardrails | G-43 |
| GOVERN 3.2 | Roles for human and AI work are defined | Mapped to guardrails | G-06, G-09 |
| GOVERN 4.1 | Culture favours safety and critical thinking | Organisation-level, no runtime guardrail | Organisational; not testable in a design document. |
| GOVERN 4.2 | Teams document risks and impacts | Mapped to guardrails | G-42 |
| GOVERN 4.3 | Testing, incident identification and information sharing are practised | Mapped to guardrails | G-33 |
| GOVERN 5.1 | Input from outside the team is collected | Mapped to guardrails | G-34 |
| GOVERN 5.2 | That input is acted on | Mapped to guardrails | G-34 |
| GOVERN 6.1 | Third-party AI risks are managed by policy | Mapped to guardrails | G-25, G-26 |
| GOVERN 6.2 | Contingency plans exist for third-party failure | Mapped to guardrails | G-26, G-28 |


## Map

| Subcategory | Topic | Status | Guardrails / note |
|---|---|---|---|
| MAP 1.1 | Intended purpose, context and laws are understood | Mapped to guardrails | G-14, G-16, G-40 |
| MAP 1.2 | People from different disciplines take part | Mapped to guardrails | G-43 |
| MAP 1.3 | Organisation's mission and goals for the system are stated | Mapped to guardrails | G-40 |
| MAP 1.4 | Business value is defined | Not covered (stated gap) | Not covered. The business case is a commercial document outside this compliance design. |
| MAP 1.5 | Risk tolerances are set | Mapped to guardrails | G-42 |
| MAP 1.6 | System requirements reflect social and technical considerations | Mapped to guardrails | G-44 |
| MAP 2.1 | Tasks and methods are defined | Mapped to guardrails | G-44 |
| MAP 2.2 | Knowledge limits and oversight information are documented | Mapped to guardrails | G-44 |
| MAP 2.3 | Scientific integrity and test-and-evaluation considerations are recorded | Mapped to guardrails | G-35 |
| MAP 3.1 | Benefits are examined | Not covered (stated gap) | Not covered. Benefits analysis belongs to the business case, outside this design. |
| MAP 3.2 | Costs of errors are examined | Mapped to guardrails | G-44 |
| MAP 3.3 | Application scope is specified | Mapped to guardrails | G-02, G-07, G-14, G-40 |
| MAP 3.4 | Operator proficiency is addressed | Mapped to guardrails | G-43 |
| MAP 3.5 | Human oversight processes are defined | Mapped to guardrails | G-06, G-09, G-10, G-19, G-43 |
| MAP 4.1 | Legal risks of components and third-party items are mapped | Mapped to guardrails | G-25, G-26 |
| MAP 4.2 | Internal controls for those components exist | Mapped to guardrails | G-25 |
| MAP 5.1 | Likelihood and magnitude of impacts are assessed | Mapped to guardrails | G-06, G-42 |
| MAP 5.2 | Regular engagement with relevant AI actors and use of their feedback on impacts | Not covered (stated gap) | Not covered as a structured practice. G-34 collects customer complaints and feedback (GOVERN 5.1 and 5.2), but no regular engagement with affected customers or staff on impacts is designed. |


## Measure

| Subcategory | Topic | Status | Guardrails / note |
|---|---|---|---|
| MEASURE 1.1 | Metrics are selected | Mapped to guardrails | G-35 |
| MEASURE 1.2 | Metrics and controls are reassessed regularly | Mapped to guardrails | G-35 |
| MEASURE 1.3 | Independent assessors are involved | Mapped to guardrails | G-45 |
| MEASURE 2.1 | Test sets and tools are documented | Mapped to guardrails | G-35 |
| MEASURE 2.2 | Evaluations involving human subjects follow ethics rules | Not covered (stated gap) | No study with human subjects is planned. Staff scenario tests are quality checks. If a customer pilot is run, consent and ethics review should be added. |
| MEASURE 2.3 | Performance is measured | Mapped to guardrails | G-17, G-35 |
| MEASURE 2.4 | Behaviour is monitored in production | Mapped to guardrails | G-29, G-31 |
| MEASURE 2.5 | Validity and reliability are shown | Mapped to guardrails | G-17 |
| MEASURE 2.6 | Safety risks are evaluated | Mapped to guardrails | G-13, G-18, G-35 |
| MEASURE 2.7 | Security and resilience are evaluated | Mapped to guardrails | G-01, G-02, G-04, G-07, G-08, G-11, G-12, G-13, G-22, G-23, G-24, G-27, G-35, G-37 |
| MEASURE 2.8 | Transparency and accountability risks are examined | Mapped to guardrails | G-15, G-44 |
| MEASURE 2.9 | Outputs are explained and validated | Mapped to guardrails | G-17 |
| MEASURE 2.10 | Privacy risk is examined | Mapped to guardrails | G-18, G-20, G-21, G-22 |
| MEASURE 2.11 | Fairness and bias are evaluated | Not covered (stated gap) | Not covered. The design tests manipulation and vulnerable-customer scenarios (G-16) but includes no fairness or bias evaluation; R-19 is monitored through complaints only. Add language, accessibility and demographic-parity style tests. |
| MEASURE 2.12 | Environmental impact is assessed | Not covered (stated gap) | Not assessed in this design. Token and compute use are tracked for cost (C14) and could serve as a proxy. Stated as a gap. |
| MEASURE 2.13 | The measurement approach is itself reviewed | Mapped to guardrails | G-35, G-45 |
| MEASURE 3.1 | Risks are tracked over time | Mapped to guardrails | G-34 |
| MEASURE 3.2 | Hard-to-measure risks are tracked in other ways | Mapped to guardrails | G-34 |
| MEASURE 3.3 | Feedback and appeal processes exist | Mapped to guardrails | G-10, G-19 |
| MEASURE 4.1 | Measurements fit the deployment context | Mapped to guardrails | G-35 |
| MEASURE 4.2 | Trustworthiness results are informed by domain experts and AI actors, and documented | Mapped to guardrails | G-34 |
| MEASURE 4.3 | Performance changes over time are identified | Mapped to guardrails | G-34, G-36 |


## Manage

| Subcategory | Topic | Status | Guardrails / note |
|---|---|---|---|
| MANAGE 1.1 | Decision on whether the system meets its purpose | Mapped to guardrails | G-42 |
| MANAGE 1.2 | Treatment of risks is prioritised | Mapped to guardrails | G-42 |
| MANAGE 1.3 | High-priority risks have responses | Mapped to guardrails | G-33, G-42 |
| MANAGE 1.4 | Residual risks are documented | Mapped to guardrails | G-42 |
| MANAGE 2.1 | Resources and non-AI alternatives are considered | Mapped to guardrails | G-19 |
| MANAGE 2.2 | Mechanisms sustain the system's value | Not covered (stated gap) | Ownership and budget for monitoring and upkeep are not defined in this design. |
| MANAGE 2.3 | Responses exist for unknown risks | Mapped to guardrails | G-27, G-28, G-32, G-38 |
| MANAGE 2.4 | Mechanisms to supersede, disengage or deactivate | Mapped to guardrails | G-09, G-32 |
| MANAGE 3.1 | Third-party resources are monitored | Mapped to guardrails | G-26 |
| MANAGE 3.2 | Pre-trained models are monitored | Mapped to guardrails | G-25 |
| MANAGE 4.1 | Post-deployment monitoring is planned | Mapped to guardrails | G-29, G-30, G-31, G-34 |
| MANAGE 4.2 | Improvement is built in | Mapped to guardrails | G-34, G-36 |
| MANAGE 4.3 | Incidents are communicated | Mapped to guardrails | G-33 |
