# Reference F: Risk Register and Threat Model

Twenty risks for Halden Assist. Ten come from the OWASP Top 10 for Agentic Applications 2026 (a published taxonomy of agent risks, not a standard); ten are additional risks I judged relevant to a customer-facing service agent in a regulated firm.

## How the scores work

Likelihood and impact are each scored 1 to 5 (1 very low, 5 very high). The score is likelihood times impact: **Low** up to 5, **Medium** 6 to 11, **High** 12 to 19, **Critical** 20 and above. These are my judgements for a hypothetical system, not measurements. **Residual** scores assume the mapped guardrails are implemented and working as specified. Since nothing is implemented, treat residual scores as targets to be proved by the tests in [04](./E-guardrails-specification.md), not as achieved results.

Machine-readable copy: [`data/risk-register.csv`](../data/risk-register.csv). Chart: [images/03-risk-heatmap.png](../images/03-risk-heatmap.png).

![Inherent and residual risk heat maps](../images/03-risk-heatmap.png)

## Summary

| ID | Risk | OWASP | Inherent | Residual (target) | Owner (role) |
|---|---|---|---|---|---|
| R-01 | Goal hijack by direct prompt injection | ASI01 | 4x4 = 16 (High) | 2x3 = 6 (Medium) | Product security lead |
| R-02 | Goal hijack by indirect injection | ASI01 / ASI06 | 4x4 = 16 (High) | 2x3 = 6 (Medium) | Product security lead |
| R-03 | Tool misuse inside granted permissions | ASI02 | 3x4 = 12 (High) | 2x3 = 6 (Medium) | Service owner |
| R-04 | Cross-customer access through the agent (confused deputy) | ASI03 | 3x5 = 15 (High) | 1x5 = 5 (Low) | Head of engineering |
| R-05 | Stolen or over-broad agent credentials | ASI03 | 3x5 = 15 (High) | 2x4 = 8 (Medium) | Head of engineering |
| R-06 | Compromised model, tool server or prompt template | ASI04 | 3x5 = 15 (High) | 2x4 = 8 (Medium) | Head of engineering |
| R-07 | Unexpected code execution | ASI05 | 2x5 = 10 (Medium) | 1x4 = 4 (Low) | Head of engineering |
| R-08 | Memory or context poisoning | ASI06 | 3x4 = 12 (High) | 2x3 = 6 (Medium) | Product security lead |
| R-09 | Spoofed or manipulated inter-agent messages | ASI07 | 2x4 = 8 (Medium) | 1x3 = 3 (Low) | Head of engineering |
| R-10 | Cascading failure, runaway loop or cost spike | ASI08 | 3x3 = 9 (Medium) | 2x2 = 4 (Low) | Service owner |
| R-11 | Over-trust by customers or approvers | ASI09 | 4x4 = 16 (High) | 3x3 = 9 (Medium) | Service owner |
| R-12 | Agent drift or rogue behaviour | ASI10 | 3x4 = 12 (High) | 2x3 = 6 (Medium) | Service owner |
| R-13 | Personal or financial data leaks to the model provider, logs or replies | (OWASP LLM sensitive-information disclosure) | 3x5 = 15 (High) | 2x4 = 8 (Medium) | Data protection officer |
| R-14 | Wrong financial information stated as fact | (hallucination) | 4x3 = 12 (High) | 2x3 = 6 (Medium) | Compliance lead |
| R-15 | Scope creep into regulated decisions | (regulatory) | 3x5 = 15 (High) | 1x4 = 4 (Low) | Compliance lead |
| R-16 | Missing or weak AI disclosure | (regulatory) | 2x3 = 6 (Medium) | 1x2 = 2 (Low) | Compliance lead |
| R-17 | Model provider outage or terms change | (resilience) | 3x3 = 9 (Medium) | 2x2 = 4 (Low) | Head of engineering |
| R-18 | Insufficient evidence to reconstruct an incident | (governance) | 3x4 = 12 (High) | 1x3 = 3 (Low) | Head of security |
| R-19 | Unfair or poor service for some customers | (fairness) | 3x3 = 9 (Medium) | 3x3 = 9 (Medium) | Compliance lead |
| R-20 | Unauthorised production change | ASI10 / ASI04 | 2x4 = 8 (Medium) | 1x3 = 3 (Low) | Head of engineering |


| Rating | Inherent | Residual (target) |
|---|---|---|
| Critical | 0 | 0 |
| High | 13 | 0 |
| Medium | 7 | 11 |
| Low | 0 | 9 |


## Risk detail

### R-01: Goal hijack by direct prompt injection

- **OWASP:** ASI01
- **What could happen:** A customer types instructions that override the agent's purpose and make it reveal data or take unintended actions.
- **Why:** Instructions and data share one text channel; the model follows persuasive text.
- **Components:** C1, C7
- **Inherent:** likelihood 4, impact 4, score 16 (High)
- **Treated by:** G-13, G-14, G-18, G-35, G-07, G-02
- **Residual (target):** likelihood 2, impact 3, score 6 (Medium)
- **Owner (role):** Product security lead


### R-02: Goal hijack by indirect injection

- **OWASP:** ASI01 / ASI06
- **What could happen:** Hidden instructions in a document, email text or tool result steer the agent while the customer sees nothing odd.
- **Why:** Agent reads untrusted content (retrieved documents, tool output, attachments).
- **Components:** C8, C9, C11
- **Inherent:** likelihood 4, impact 4, score 16 (High)
- **Treated by:** G-13, G-18, G-24, G-31, G-35, G-02
- **Residual (target):** likelihood 2, impact 3, score 6 (Medium)
- **Owner (role):** Product security lead


### R-03: Tool misuse inside granted permissions

- **OWASP:** ASI02
- **What could happen:** The agent calls an allowed tool with harmful parameters, for example freezing the wrong card or sending a large number of requests.
- **Why:** Tool scope is broad; parameters are decided by the model.
- **Components:** C8, C9
- **Inherent:** likelihood 3, impact 4, score 12 (High)
- **Treated by:** G-06, G-07, G-08, G-09, G-10, G-27
- **Residual (target):** likelihood 2, impact 3, score 6 (Medium)
- **Owner (role):** Service owner


### R-04: Cross-customer access through the agent (confused deputy)

- **OWASP:** ASI03
- **What could happen:** The agent uses its own broad rights to fetch another customer's data because the request looks valid.
- **Why:** Authorisation is checked against the agent, not the end user.
- **Components:** C5, C9, C10
- **Inherent:** likelihood 3, impact 5, score 15 (High)
- **Treated by:** G-02, G-01, G-03, G-04, G-31
- **Residual (target):** likelihood 1, impact 5, score 5 (Low)
- **Owner (role):** Head of engineering
- **Note:** Low likelihood is conditional on G-02 being enforced at the tool server, not only at the edge


### R-05: Stolen or over-broad agent credentials

- **OWASP:** ASI03
- **What could happen:** A long-lived token for a tool server leaks and is used outside the agent.
- **Why:** Standing secrets, shared service accounts, secrets in config or logs.
- **Components:** C9, C10, C15
- **Inherent:** likelihood 3, impact 5, score 15 (High)
- **Treated by:** G-03, G-04, G-05, G-23, G-37, G-31, G-46, G-39
- **Residual (target):** likelihood 2, impact 4, score 8 (Medium)
- **Owner (role):** Head of engineering


### R-06: Compromised model, tool server or prompt template

- **OWASP:** ASI04
- **What could happen:** A tool server, MCP server, library or hosted model changes behaviour through a malicious or careless update.
- **Why:** Dynamic components are pulled in without pinning or verification; provider changes are not noticed.
- **Components:** C9, C12
- **Inherent:** likelihood 3, impact 5, score 15 (High)
- **Treated by:** G-25, G-26, G-36, G-35, G-12, G-07
- **Residual (target):** likelihood 2, impact 4, score 8 (Medium)
- **Owner (role):** Head of engineering


### R-07: Unexpected code execution

- **OWASP:** ASI05
- **What could happen:** A tool or generated output causes code or commands to run in the agent's environment.
- **Why:** Tools that evaluate text or run code; weak input validation.
- **Components:** C8, C9
- **Inherent:** likelihood 2, impact 5, score 10 (Medium)
- **Treated by:** G-11, G-12, G-08, G-37
- **Residual (target):** likelihood 1, impact 4, score 4 (Low)
- **Owner (role):** Head of engineering


### R-08: Memory or context poisoning

- **OWASP:** ASI06
- **What could happen:** False or malicious content is stored in session memory or shared state and later treated as trusted.
- **Why:** Memory writes from untrusted content; no isolation or expiry.
- **Components:** C11
- **Inherent:** likelihood 3, impact 4, score 12 (High)
- **Treated by:** G-22, G-13, G-21, G-24, G-29
- **Residual (target):** likelihood 2, impact 3, score 6 (Medium)
- **Owner (role):** Product security lead


### R-09: Spoofed or manipulated inter-agent messages

- **OWASP:** ASI07
- **What could happen:** A forged or altered message between the coordinator and a specialist triggers an action.
- **Why:** Agents trust each other's text; no sender authentication.
- **Components:** C7, C8, C11
- **Inherent:** likelihood 2, impact 4, score 8 (Medium)
- **Treated by:** G-24, G-03, G-12, G-31
- **Residual (target):** likelihood 1, impact 3, score 3 (Low)
- **Owner (role):** Head of engineering


### R-10: Cascading failure, runaway loop or cost spike

- **OWASP:** ASI08
- **What could happen:** An error in one agent spreads to others, or a loop consumes budget and capacity.
- **Why:** No step or budget limits; shared state spreads errors.
- **Components:** C7, C8, C14
- **Inherent:** likelihood 3, impact 3, score 9 (Medium)
- **Treated by:** G-27, G-28, G-32, G-24, G-38, G-31
- **Residual (target):** likelihood 2, impact 2, score 4 (Low)
- **Owner (role):** Service owner


### R-11: Over-trust by customers or approvers

- **OWASP:** ASI09
- **What could happen:** Customers act on a confident wrong answer, or staff approve Tier 3 requests without looking (approval fatigue, automation bias).
- **Why:** Fluent output; many routine approvals; summaries written by the agent.
- **Components:** C1, C16
- **Inherent:** likelihood 4, impact 4, score 16 (High)
- **Treated by:** G-09, G-10, G-15, G-16, G-17, G-19, G-43, G-44
- **Residual (target):** likelihood 3, impact 3, score 9 (Medium)
- **Owner (role):** Service owner
- **Note:** Hardest to reduce by technology alone; depends on training and sampling


### R-12: Agent drift or rogue behaviour

- **OWASP:** ASI10
- **What could happen:** Behaviour moves away from the intended scope over time, for example after a model update, without anyone noticing.
- **Why:** Provider model changes; no behavioural monitoring.
- **Components:** C7, C8, C12
- **Inherent:** likelihood 3, impact 4, score 12 (High)
- **Treated by:** G-29, G-31, G-32, G-34, G-36, G-35, G-03, G-40
- **Residual (target):** likelihood 2, impact 3, score 6 (Medium)
- **Owner (role):** Service owner


### R-13: Personal or financial data leaks to the model provider, logs or replies

- **OWASP:** (OWASP LLM sensitive-information disclosure)
- **What could happen:** Account details are sent to a third party, written to logs in clear, or appear in another person's reply.
- **Why:** Raw text sent to models; verbose logging; weak output checks.
- **Components:** C6, C12, C15
- **Inherent:** likelihood 3, impact 5, score 15 (High)
- **Treated by:** G-20, G-21, G-18, G-26, G-23, G-22, G-39
- **Residual (target):** likelihood 2, impact 4, score 8 (Medium)
- **Owner (role):** Data protection officer


### R-14: Wrong financial information stated as fact

- **OWASP:** (hallucination)
- **What could happen:** The agent invents a fee, a right or a product term and the customer relies on it, leading to complaints or mis-selling findings.
- **Why:** Model generates plausible text without a source.
- **Components:** C7, C8
- **Inherent:** likelihood 4, impact 3, score 12 (High)
- **Treated by:** G-17, G-35, G-34, G-19, G-44
- **Residual (target):** likelihood 2, impact 3, score 6 (Medium)
- **Owner (role):** Compliance lead


### R-15: Scope creep into regulated decisions

- **OWASP:** (regulatory)
- **What could happen:** Product teams add credit-limit, loan-eligibility or insurance features, making the system high-risk under Annex III without a new assessment.
- **Why:** Business pressure; classification treated as a one-time memo.
- **Components:** ORG, C7
- **Inherent:** likelihood 3, impact 5, score 15 (High)
- **Treated by:** G-14, G-40, G-41, G-42, G-36
- **Residual (target):** likelihood 1, impact 4, score 4 (Low)
- **Owner (role):** Compliance lead
- **Note:** Impact is regulatory: obligations of Arts 8-27 apply from the Annex III date


### R-16: Missing or weak AI disclosure

- **OWASP:** (regulatory)
- **What could happen:** Customers are not clearly told they are talking to AI, or the agent behaves as if it were a person.
- **Why:** Design choices that favour a human feel; disclosure removed in a UI change.
- **Components:** C1, C7
- **Inherent:** likelihood 2, impact 3, score 6 (Medium)
- **Treated by:** G-15, G-36, G-35
- **Residual (target):** likelihood 1, impact 2, score 2 (Low)
- **Owner (role):** Compliance lead


### R-17: Model provider outage or terms change

- **OWASP:** (resilience)
- **What could happen:** The hosted model is unavailable, degraded or its terms change, interrupting customer service.
- **Why:** Single-provider dependency.
- **Components:** C12
- **Inherent:** likelihood 3, impact 3, score 9 (Medium)
- **Treated by:** G-28, G-26, G-38, G-36
- **Residual (target):** likelihood 2, impact 2, score 4 (Low)
- **Owner (role):** Head of engineering


### R-18: Insufficient evidence to reconstruct an incident

- **OWASP:** (governance)
- **What could happen:** After an incident the firm cannot show what the agent did, for whom, or why, and cannot answer a regulator.
- **Why:** Missing or inconsistent logs; short retention; clocks out of sync.
- **Components:** C15
- **Inherent:** likelihood 3, impact 4, score 12 (High)
- **Treated by:** G-29, G-30, G-33, G-45
- **Residual (target):** likelihood 1, impact 3, score 3 (Low)
- **Owner (role):** Head of security


### R-19: Unfair or poor service for some customers

- **OWASP:** (fairness)
- **What could happen:** Service quality is worse for certain languages, accents, accessibility needs or vulnerable customers.
- **Why:** Model performance differs across groups; no testing for it.
- **Components:** C7, C13
- **Inherent:** likelihood 3, impact 3, score 9 (Medium)
- **Treated by:** G-16, G-19, G-34, G-35, G-43
- **Residual (target):** likelihood 3, impact 3, score 9 (Medium)
- **Owner (role):** Compliance lead
- **Note:** No fairness or bias evaluation is designed (NIST MEASURE 2.11 is a stated gap), so residual stays medium until one is added


### R-20: Unauthorised production change

- **OWASP:** ASI10 / ASI04
- **What could happen:** A prompt, tool list or approval rule is changed without review, silently altering behaviour.
- **Why:** Weak change control; excessive privileged access.
- **Components:** C7, C9, C16
- **Inherent:** likelihood 2, impact 4, score 8 (Medium)
- **Treated by:** G-05, G-36, G-29, G-46
- **Residual (target):** likelihood 1, impact 3, score 3 (Low)
- **Owner (role):** Head of engineering

