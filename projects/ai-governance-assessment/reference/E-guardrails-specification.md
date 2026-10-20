# Reference E: Guardrails Specification

What Halden Assist must do, in testable terms. Forty-six guardrails in seven layers. Each has an ID that appears in the [control matrix](./A-full-guardrail-crosswalk.md) and the [risk register](./F-risk-register-detail.md), the component where it is enforced, the evidence that would show it works, and a test.

**Status of everything here: Designed.** Nothing is built. The configuration snippets are illustrative shapes, not tested code.

## Design rules behind the guardrails

1. **Enforce outside the model.** A prompt can ask the model to behave. Only code outside the model (authorisation service, tool servers, network policy) can make it. Where a guardrail says "enforced in" a component, that component is not the LLM.
2. **Authorise against the user.** The agent never has more rights than the customer it is serving.
3. **One identity per agent, short-lived credentials, scoped per tool server.** Revoking one agent must not break the others.
4. **Tier every action.** What the agent may do alone, with the customer's confirmation, or with a person's approval is a table, not a sentence in a prompt.
5. **Log so an incident can be reconstructed.** If it is not logged with who, for whom, what and outcome, it did not happen as far as an auditor is concerned.

## Action tiers (G-06)

| Tier | Meaning | Examples | Needs | Human approval |
|---|---|---|---|---|
| Tier 0 | General information | Opening hours, product FAQs, how-to answers | No customer data touched | None |
| Tier 1 | Read the customer's own data | Balance, recent transactions, statements, card status | Authenticated user; authorisation check against the user | None (logged) |
| Tier 2 | Reversible change | Log a dispute, request a statement, update contact preferences, temporarily freeze a card | Authenticated user plus explicit in-chat confirmation of the exact action | None, but customer notified by out-of-band message |
| Tier 3 | Sensitive or hard to reverse | Change postal address or phone, replace a card, change payee or limits, close a service | Step-up authentication plus human approval of the exact action | Required: staff approver in C16 |
| Never | Outside the agent's remit | Credit or lending decisions, insurance pricing, investment advice, legal advice, moving money to a new payee | Refused and handed to a person | n/a |


## Illustrative configuration

### Tool register (G-06, G-07, G-14)

```yaml
# tool-register.yaml : illustrative
tools:
  - name: get_balance
    server: accounts-mcp
    agent: specialist-a
    tier: 1
    scopes: [accounts:read]
    limits: {max_accounts: 5}
  - name: freeze_card
    server: cards-mcp
    agent: specialist-b
    tier: 2
    requires: [user_confirmation]
  - name: change_postal_address
    server: requests-mcp
    agent: specialist-c
    tier: 3
    requires: [step_up_auth, human_approval]
    approval: {queue: staff-approvals, expires_minutes: 30, show: exact_change}
# Deliberately absent: any tool for credit, lending, insurance pricing or investment decisions (G-14).
# A tool with no tier cannot be registered.
```

### Authorisation rules (G-02, G-04, G-10)

```yaml
# policy: tool_call_authorisation : illustrative
inputs: [end_user_id, agent_id, tool, record_owner_id, tier, auth_level, user_confirmed, approval_id]
rules:
  - deny if: record_owner_id != end_user_id          # confused-deputy check
  - deny if: tool not in allowlist[agent_id]         # G-07
  - deny if: tier >= 2 and not user_confirmed        # G-10
  - deny if: tier == 3 and (auth_level != step_up or approval_id is null)   # G-01, G-09
  - otherwise: allow, and write the decision to the audit log
```

### Audit log record (G-29, G-20)

```json
{
  "ts": "2026-11-03T09:14:22.381Z",
  "session": "s_8f21",
  "customer": "cust_tok_4b7a",
  "agent_id": "specialist-b",
  "tool": "freeze_card",
  "tier": 2,
  "params": {"card": "card_tok_91c2"},
  "authz": {"decision": "allow", "policy_version": "2026.11.1"},
  "user_confirmed": true,
  "approval_id": null,
  "outcome": "success",
  "model": {"name": "third-party-llm", "version": "pinned-2026-10"},
  "prompt_version": "p-0042"
}
```

### Detection rules for SIEM (G-31)

| Rule | Fires when | Response |
|---|---|---|
| Cross-customer attempt | Authorisation denies a call where the record owner differs from the end user | Alert; after 3 in a session, suspend session and review |
| Tool-call burst | An agent's calls per minute exceed its baseline by a set factor | Throttle; alert |
| Instruction in data | Injection detector flags instruction-like text in a retrieved document or tool result | Log; strip; alert if repeated |
| New identity or tool | An agent identity or tool appears that is not in the register | Block; page security |
| Tier 3 anomaly | Tier 3 requests out of hours, or a spike in approvals per approver | Hold; second reviewer |
| Repeated denials | The same agent is denied repeatedly on the same tool | Investigate for injection or misconfiguration |
| Cost runaway | Tokens or steps for a session exceed the cap | Stop session; alert |


### Kill switch levels (G-32)

| Level | Effect | Who can trigger |
|---|---|---|
| Session | End one conversation and hand to a person | Agent, staff, SOAR |
| Tool | Disable one tool for everyone | On-call engineer, security |
| Agent | Suspend one specialist and revoke its credentials | Security lead |
| System | Route all customers to static FAQ and human handoff | Incident commander |


## Identity and access

### G-01: Authenticate the human at the edge

**What it does.** Customers authenticate with MFA before any personal data is reachable. Tier 3 actions need step-up authentication. Unauthenticated sessions only reach Tier 0.

- **Enforced in:** C2, C3 (Edge layer; Identity provider)
- **Maps to:** ISO 27001 5.16, 5.17, 8.5 · EU AI Act Art 15 · NIST AI RMF MEASURE 2.7 · OWASP ASI03
- **Reduces risk:** R-04
- **Evidence:** IdP configuration export; authentication logs; step-up policy
- **Test:** Attempt Tier 1 and Tier 3 calls with no token, an expired token and a low-assurance token: all denied

### G-02: Authorise against the user, not just the agent

**What it does.** Every tool call carries the end-user's identity. C5 checks that this user may do this thing to this record. The agent's own permissions never widen what the user may do (prevents the confused deputy).

- **Enforced in:** C5, C9 (Authorisation service; MCP tool servers (A, B, C))
- **Maps to:** ISO 27001 5.15, 5.18, 8.3 · EU AI Act Art 15 · NIST AI RMF MEASURE 2.7, MAP 3.3 · OWASP ASI03, ASI02
- **Reduces risk:** R-01, R-02, R-04
- **Evidence:** Policy-as-code repository; decision logs from C5
- **Test:** Ask the agent for another customer's balance using valid IDs: denied at C5 and alert raised

### G-03: One workload identity per agent, no shared credentials

**What it does.** The coordinator and each specialist run under their own non-human identity. No shared service account. Every identity has a named human owner and a documented purpose.

- **Enforced in:** C7, C8, C9 (Coordinator agent; Specialist agents (A, B, C); MCP tool servers (A, B, C))
- **Maps to:** ISO 27001 5.16, 5.2, 8.2 · EU AI Act Art 15 · NIST AI RMF GOVERN 1.6, GOVERN 2.1 · OWASP ASI03, ASI10
- **Reduces risk:** R-04, R-05, R-09, R-12
- **Evidence:** Non-human identity inventory with owner and purpose; IdP / cloud IAM export
- **Test:** Reconcile the inventory against IAM monthly: no identity without an owner; no identity used by two agents

### G-04: Short-lived, scoped credentials per tool server

**What it does.** Agents obtain a token for one tool server at a time through token exchange. Tokens expire in minutes and are scoped to the tool and tier. No long-lived secrets in prompts, config or the session store.

- **Enforced in:** C9, C10 (MCP tool servers (A, B, C); Backend systems)
- **Maps to:** ISO 27001 5.17, 8.2, 8.24 · EU AI Act Art 15 · NIST AI RMF MEASURE 2.7 · OWASP ASI03
- **Reduces risk:** R-04, R-05
- **Evidence:** Token-exchange policy; secrets-scanner results; token lifetime settings
- **Test:** Replay an expired token and a token for tool server A against server B: both rejected

### G-05: Privileged access to agent configuration

**What it does.** Changing prompts, tool lists, policies or approval rules needs privileged access, with MFA, just-in-time elevation and a second person for production changes.

- **Enforced in:** C7, C13, C16 (Coordinator agent; Evaluation suite; Human approval and handoff console)
- **Maps to:** ISO 27001 8.2, 5.3, 8.18, 8.4 · EU AI Act Art 14 · NIST AI RMF GOVERN 2.1 · OWASP ASI03, ASI10
- **Reduces risk:** R-05, R-20
- **Evidence:** Privileged-access review records; change approvals
- **Test:** Attempt a production prompt edit as a single engineer: blocked pending second approval


## Tools and actions

### G-06: Action risk tiering

**What it does.** Every tool is assigned a tier (0 to 3, plus Never). The tier decides authentication strength, confirmation and approval. The tier table is data held in C5, not text in a prompt.

- **Enforced in:** C5, C8, C9, C16 (Authorisation service; Specialist agents (A, B, C); MCP tool servers (A, B, C); Human approval and handoff console)
- **Maps to:** ISO 27001 5.15, 8.3 · EU AI Act Art 14, Art 9 · NIST AI RMF MAP 3.5, MAP 5.1, GOVERN 3.2 · OWASP ASI02
- **Reduces risk:** R-03
- **Evidence:** Tool register with tier per tool
- **Test:** Every tool in the register has a tier; a tool with no tier cannot be called

### G-07: Tool allow-list per agent, deny by default

**What it does.** Each specialist can call only the tools listed for it. New tools are denied until reviewed. The coordinator has no direct backend access.

- **Enforced in:** C8, C9 (Specialist agents (A, B, C); MCP tool servers (A, B, C))
- **Maps to:** ISO 27001 5.15, 8.3, 8.9 · EU AI Act Art 15 · NIST AI RMF MAP 3.3, MEASURE 2.7 · OWASP ASI02, ASI04
- **Reduces risk:** R-01, R-03, R-06
- **Evidence:** Allow-list files in version control; tool-server access policy
- **Test:** Instruct specialist B to call a tool owned by A: denied and logged

### G-08: Server-side parameter validation and limits

**What it does.** Tool servers validate inputs against a strict schema and hard limits (record count, date range, amounts). Limits live in the tool server, not in the prompt.

- **Enforced in:** C9, C10 (MCP tool servers (A, B, C); Backend systems)
- **Maps to:** ISO 27001 8.26, 8.28 · EU AI Act Art 15 · NIST AI RMF MEASURE 2.7 · OWASP ASI02, ASI05
- **Reduces risk:** R-03, R-07
- **Evidence:** Schemas and limit configuration; unit tests
- **Test:** Send out-of-range and malformed parameters: rejected with an error, no backend call

### G-09: Human approval for Tier 3 actions

**What it does.** Tier 3 actions are held until a trained staff member approves the exact action shown to them (not a summary written by the agent). The approver is a different person from anyone who can change the policy. Approval time and reasons are recorded.

- **Enforced in:** C16, C9 (Human approval and handoff console; MCP tool servers (A, B, C))
- **Maps to:** ISO 27001 5.3, 5.2 · EU AI Act Art 14 · NIST AI RMF GOVERN 3.2, MAP 3.5, MANAGE 2.4 · OWASP ASI09, ASI02
- **Reduces risk:** R-03, R-11
- **Evidence:** Approval-queue logs; sampling of approvals by a second reviewer
- **Test:** Run 50 seeded Tier 3 requests including 5 deliberately wrong ones: all wrong ones caught

### G-10: Explicit customer confirmation for Tier 2 actions

**What it does.** Before a Tier 2 action the agent shows the exact action in plain language and needs a clear confirmation. An out-of-band notification (email or app push) follows.

- **Enforced in:** C4, C8 (Application API; Specialist agents (A, B, C))
- **Maps to:** ISO 27001 8.26 · EU AI Act Art 14 · NIST AI RMF MAP 3.5, MEASURE 3.3 · OWASP ASI09, ASI02
- **Reduces risk:** R-03, R-11
- **Evidence:** UI confirmation logs; notification logs
- **Test:** Attempt Tier 2 call without a confirmation event: denied

### G-11: No code execution by default

**What it does.** Agents cannot generate and run code, shell commands or arbitrary queries. If a future tool needs this, it runs in an isolated sandbox with no network and no credentials.

- **Enforced in:** C8, C9 (Specialist agents (A, B, C); MCP tool servers (A, B, C))
- **Maps to:** ISO 27001 8.19, 8.22, 8.31 · EU AI Act Art 15 · NIST AI RMF MEASURE 2.7 · OWASP ASI05
- **Reduces risk:** R-07
- **Evidence:** Runtime configuration; container security policy
- **Test:** Ask the agent to run a command: refused; scan runtime for interpreter access

### G-12: Egress allow-list and network segmentation

**What it does.** Agents and tool servers can reach only named destinations (model gateway, approved backends). Production, test and development are separate networks.

- **Enforced in:** C2, C9, C12 (Edge layer; MCP tool servers (A, B, C); Model layer)
- **Maps to:** ISO 27001 8.20, 8.21, 8.22, 8.23, 8.31 · EU AI Act Art 15 · NIST AI RMF MEASURE 2.7 · OWASP ASI05, ASI04
- **Reduces risk:** R-06, R-07, R-09
- **Evidence:** Network policy export; firewall rules; egress logs
- **Test:** From the agent runtime, attempt a connection to an unlisted host: blocked and alerted


## Content and model

### G-13: Prompt-injection and untrusted-content defences

**What it does.** User text, retrieved documents and tool output are treated as data, never as instructions. Retrieved content is labelled and cannot change the tier, tool list or scope. Instructions in data are logged as an injection signal.

- **Enforced in:** C7, C8, C11 (Coordinator agent; Specialist agents (A, B, C); Session and memory store)
- **Maps to:** ISO 27001 8.26, 8.28, 8.29 · EU AI Act Art 15 · NIST AI RMF MEASURE 2.7, MEASURE 2.6 · OWASP ASI01, ASI06
- **Reduces risk:** R-01, R-02, R-08
- **Evidence:** Prompt architecture document; injection test results
- **Test:** Run the injection test set (direct, indirect, multi-turn): attack success rate under the agreed threshold

### G-14: Scope boundary and refusal of regulated decisions

**What it does.** The agent refuses credit, lending, insurance-pricing, investment and legal-advice requests and hands over to a person. A classifier plus tool design (no tool exists for these decisions) enforces it. A scope change triggers re-classification under the EU AI Act (G-40).

- **Enforced in:** C7, C8 (Coordinator agent; Specialist agents (A, B, C))
- **Maps to:** ISO 27001 5.31 · EU AI Act Art 6, Art 5 · NIST AI RMF MAP 1.1, MAP 3.3, GOVERN 1.1 · OWASP ASI01
- **Reduces risk:** R-01, R-15
- **Evidence:** Scope statement; refusal test results; tool register showing no decision tools
- **Test:** Run 100 prompts asking for credit, insurance and investment decisions: all refused and handed over

### G-15: Disclose that the customer is talking to AI

**What it does.** The chat window and the first message state that this is an AI system, and the agent says so when asked. The agent never claims to be a person and never uses a staff member's name or photo.

- **Enforced in:** C1, C7 (Chat UI; Coordinator agent)
- **Maps to:** ISO 27001 5.31 · EU AI Act Art 50(1) · NIST AI RMF GOVERN 1.4, MEASURE 2.8 · OWASP ASI09
- **Reduces risk:** R-11, R-16
- **Evidence:** UI screenshots; conversation-start template; test results
- **Test:** Ask 'are you a person?' in 20 phrasings and 5 languages: always answers truthfully

### G-16: No manipulation or pressure

**What it does.** The agent must not use urgency, fear, flattery or emotional pressure to steer customers, and must not exploit signs of vulnerability. Signs of vulnerability or distress route to a person. No emotion inference is used.

- **Enforced in:** C7, C13 (Coordinator agent; Evaluation suite)
- **Maps to:** ISO 27001 5.10 · EU AI Act Art 5, Art 50(3) · NIST AI RMF MAP 1.1 · OWASP ASI09
- **Reduces risk:** R-11, R-19
- **Evidence:** Content policy; red-team results on pressure and vulnerability scenarios
- **Test:** Run vulnerable-customer scenarios: handed to a person; no manipulative phrasing found in review

### G-17: Grounded answers and confident handoff

**What it does.** Factual answers about fees, rights and products come from approved sources with a citation. If there is no source or confidence is low, the agent says it does not know and offers a person.

- **Enforced in:** C7, C8, C13 (Coordinator agent; Specialist agents (A, B, C); Evaluation suite)
- **Maps to:** ISO 27001 8.29 · EU AI Act Art 15, Art 13 · NIST AI RMF MEASURE 2.5, MEASURE 2.9, MEASURE 2.3 · OWASP ASI09
- **Reduces risk:** R-11, R-14
- **Evidence:** Source register; grounding test results; hallucination rate on the evaluation set
- **Test:** Run 200 questions with known answers and 50 with no answer: error and fabrication rates under the agreed threshold

### G-18: Output filtering

**What it does.** Replies are checked before display for personal data of other people, internal prompts, credentials and unsupported financial advice.

- **Enforced in:** C7, C6 (Coordinator agent; PII redaction service)
- **Maps to:** ISO 27001 8.12, 5.14 · EU AI Act Art 15 · NIST AI RMF MEASURE 2.10, MEASURE 2.6 · OWASP ASI01
- **Reduces risk:** R-01, R-02, R-13
- **Evidence:** Filter configuration; blocked-output logs
- **Test:** Seed the model with other customers' data and a system prompt in context: nothing reaches the reply

### G-19: Human handoff at any time

**What it does.** The customer can ask for a person at any point and the agent hands over with the conversation context. Handoff is also triggered by distress, repeated failure and out-of-scope requests.

- **Enforced in:** C16, C7 (Human approval and handoff console; Coordinator agent)
- **Maps to:** ISO 27001 5.37 · EU AI Act Art 14 · NIST AI RMF MEASURE 3.3, MAP 3.5, MANAGE 2.1 · OWASP ASI09
- **Reduces risk:** R-11, R-14, R-19
- **Evidence:** Handoff logs and queue times
- **Test:** Request a person in 20 phrasings: handoff offered every time


## Data

### G-20: PII redaction before model and in logs

**What it does.** Account numbers, card numbers and national identifiers are replaced with tokens before the text reaches a model or a log. Real values are re-inserted only inside tool servers.

- **Enforced in:** C6, C12, C15 (PII redaction service; Model layer; Observability pipeline and SIEM)
- **Maps to:** ISO 27001 5.34, 8.11, 8.12 · EU AI Act Art 10 · NIST AI RMF MEASURE 2.10 · OWASP ASI01
- **Reduces risk:** R-13
- **Evidence:** Redaction rule set; samples of logs and model requests
- **Test:** Send known PII test strings through the pipeline and search model-gateway logs: none found

### G-21: Data minimisation for the context window

**What it does.** Only the fields needed for the current task are placed in context. Full statements and unrelated history are not loaded.

- **Enforced in:** C4, C8, C11 (Application API; Specialist agents (A, B, C); Session and memory store)
- **Maps to:** ISO 27001 5.34, 8.3 · EU AI Act Art 10 · NIST AI RMF MEASURE 2.10 · OWASP ASI06
- **Reduces risk:** R-08, R-13
- **Evidence:** Data-flow diagram; context-assembly code review
- **Test:** Inspect a sample of model requests: only task-relevant fields present

### G-22: Session and memory hygiene

**What it does.** Memory is isolated per customer and per session, expires on a fixed schedule, and cannot be written from untrusted content. Entries are integrity-checked. Nothing a customer says is used to train or tune a model.

- **Enforced in:** C11 (Session and memory store)
- **Maps to:** ISO 27001 8.3, 8.10, 5.12, 5.33 · EU AI Act Art 15 · NIST AI RMF MEASURE 2.7, MEASURE 2.10 · OWASP ASI06
- **Reduces risk:** R-08, R-13
- **Evidence:** Store configuration; retention schedule; deletion logs
- **Test:** Plant an instruction in session A and open session B as the same and another customer: it never appears

### G-23: Cryptography for data in transit and at rest

**What it does.** TLS between all components; encryption at rest with managed keys; key rotation and access limited to named roles.

- **Enforced in:** C2, C11, C12, C15 (Edge layer; Session and memory store; Model layer; Observability pipeline and SIEM)
- **Maps to:** ISO 27001 8.24 · EU AI Act Art 15 · NIST AI RMF MEASURE 2.7 · OWASP -
- **Reduces risk:** R-05, R-13
- **Evidence:** TLS and KMS configuration; key rotation records
- **Test:** Scan endpoints for weak protocols; verify encryption flags on stores


## Agents and supply chain

### G-24: Authenticated, validated inter-agent messages

**What it does.** Messages between the coordinator and specialists are signed by the sender identity, follow a fixed schema and are checked on receipt. Free-form text between agents cannot carry instructions that change tier or scope.

- **Enforced in:** C7, C8, C11 (Coordinator agent; Specialist agents (A, B, C); Session and memory store)
- **Maps to:** ISO 27001 8.26, 8.22 · EU AI Act Art 15 · NIST AI RMF MEASURE 2.7 · OWASP ASI07, ASI08
- **Reduces risk:** R-02, R-08, R-09, R-10
- **Evidence:** Message schema; signature verification logs
- **Test:** Inject a forged specialist message: rejected and alerted

### G-25: Agentic supply-chain control

**What it does.** Models, prompts, tool servers, MCP servers and libraries are pinned to versions and verified (hash or signature). A bill of materials lists them. New items need review before use.

- **Enforced in:** C7, C9, C12 (Coordinator agent; MCP tool servers (A, B, C); Model layer)
- **Maps to:** ISO 27001 5.21, 8.19, 8.30 · EU AI Act Art 25, Art 15 · NIST AI RMF GOVERN 6.1, MAP 4.1, MAP 4.2, MANAGE 3.2 · OWASP ASI04
- **Reduces risk:** R-06
- **Evidence:** AI bill of materials; signature checks in the pipeline
- **Test:** Swap a tool server image for an unsigned one: deployment fails

### G-26: Third-party model provider management

**What it does.** Contract with the model provider covers: no training on the firm's data, data location, retention, sub-processors, security attestations, incident notification and change notice. Provider reviews happen at least yearly and on major changes.

- **Enforced in:** C12, ORG (Model layer; Organisation (not a runtime component))
- **Maps to:** ISO 27001 5.19, 5.20, 5.22, 5.23 · EU AI Act Art 25 · NIST AI RMF GOVERN 6.1, GOVERN 6.2, MANAGE 3.1, MAP 4.1 · OWASP ASI04
- **Reduces risk:** R-06, R-13, R-17
- **Evidence:** Signed contract and data-processing terms; supplier review records
- **Test:** Check contract clauses against the checklist; test that data-retention settings are applied

### G-27: Cost, loop and rate limits

**What it does.** Per-user and per-agent rate limits, token budgets and a maximum number of steps per request. Repeated retries and loops stop automatically.

- **Enforced in:** C2, C14 (Edge layer; Cost and usage tracker)
- **Maps to:** ISO 27001 8.6, 8.20 · EU AI Act Art 15 · NIST AI RMF MEASURE 2.7, MANAGE 2.3 · OWASP ASI08
- **Reduces risk:** R-03, R-10
- **Evidence:** Limit configuration; cost-tracker alerts
- **Test:** Trigger a loop and a burst of requests: stopped at the limit and alert raised

### G-28: Fallback when the model fails

**What it does.** If the model or a provider is unavailable or misbehaving, the system drops to static FAQ and human handoff. It never falls back to an unreviewed model.

- **Enforced in:** C7, C12, C16 (Coordinator agent; Model layer; Human approval and handoff console)
- **Maps to:** ISO 27001 5.29, 5.30, 8.14 · EU AI Act Art 15 · NIST AI RMF GOVERN 6.2, MANAGE 2.3 · OWASP ASI08
- **Reduces risk:** R-10, R-17
- **Evidence:** Fallback runbook; failover test records
- **Test:** Disable the model gateway: customers see the fallback within the agreed time


## Monitoring and response

### G-29: Structured, tamper-evident action log

**What it does.** Every agent step is logged: time, customer (tokenised), agent identity, tool, parameters (redacted), tier, approval, outcome. Logs go to write-once or integrity-checked storage.

- **Enforced in:** C15 (Observability pipeline and SIEM)
- **Maps to:** ISO 27001 8.15, 5.28, 5.33 · EU AI Act Art 12, Art 19 · NIST AI RMF MEASURE 2.4, MANAGE 4.1 · OWASP ASI10
- **Reduces risk:** R-08, R-12, R-18, R-20
- **Evidence:** Log schema; integrity-check results
- **Test:** Reconstruct a seeded incident end to end from logs alone

### G-30: Log retention and clock synchronisation

**What it does.** Logs are kept for a stated period (design target: at least six months, in line with Art 26(6) for high-risk deployers; longer where sector rules require) and all components use one trusted time source.

- **Enforced in:** C15 (Observability pipeline and SIEM)
- **Maps to:** ISO 27001 8.15, 8.17, 5.33, 5.31 · EU AI Act Art 26, Art 19 · NIST AI RMF MANAGE 4.1 · OWASP ASI10
- **Reduces risk:** R-18
- **Evidence:** Retention configuration; NTP configuration
- **Test:** Check oldest retrievable record; compare timestamps across components

### G-31: SIEM detection and SOAR response for agent behaviour

**What it does.** Detection rules for: unusual tool-call volume, cross-customer access attempts, repeated authorisation denials, instruction-like text in data, new tools or identities, off-hours Tier 3 requests. SOAR playbooks can suspend an agent or revoke its tokens.

- **Enforced in:** C15, C16 (Observability pipeline and SIEM; Human approval and handoff console)
- **Maps to:** ISO 27001 8.16, 5.7, 5.25 · EU AI Act Art 72, Art 26 · NIST AI RMF MEASURE 2.4, MANAGE 4.1 · OWASP ASI10, ASI03, ASI08
- **Reduces risk:** R-02, R-04, R-05, R-09, R-10, R-12
- **Evidence:** Rule set in version control; alert and playbook run records
- **Test:** Replay seeded attack traces: each triggers the expected alert within the agreed time

### G-32: Kill switch and agent suspension

**What it does.** Staff can suspend one agent, one tool or the whole agent system within minutes, and revoke credentials at the same time. Customers are routed to humans while suspended.

- **Enforced in:** C16, C15, C3 (Human approval and handoff console; Observability pipeline and SIEM; Identity provider)
- **Maps to:** ISO 27001 5.26, 8.2 · EU AI Act Art 14, Art 26 · NIST AI RMF MANAGE 2.4, MANAGE 2.3 · OWASP ASI10, ASI08
- **Reduces risk:** R-10, R-12
- **Evidence:** Kill-switch runbook; drill records
- **Test:** Quarterly drill: time from decision to full stop and credential revocation is within the target

### G-33: Incident response runbook for agent incidents

**What it does.** A runbook covers detection, triage, containment, customer communication, evidence preservation and regulator notification. It names who decides whether an event is a personal-data breach, a major ICT incident or an AI serious incident, and by which clock.

- **Enforced in:** ORG, C15 (Organisation (not a runtime component); Observability pipeline and SIEM)
- **Maps to:** ISO 27001 5.24, 5.25, 5.26, 5.27, 5.5, 6.8 · EU AI Act Art 73, Art 26 · NIST AI RMF MANAGE 4.3, GOVERN 4.3, MANAGE 1.3 · OWASP ASI10
- **Reduces risk:** R-18
- **Evidence:** Runbook; tabletop exercise report
- **Test:** Tabletop: a prompt-injection data leak. Roles, clocks and decisions are clear

### G-34: Post-deployment monitoring and customer feedback

**What it does.** Quality, safety and complaint measures are tracked over time: handoff rate, refusal rate, fabrication rate, complaint themes, drift after model updates. Findings feed improvement.

- **Enforced in:** C14, C15, C16 (Cost and usage tracker; Observability pipeline and SIEM; Human approval and handoff console)
- **Maps to:** ISO 27001 5.27 · EU AI Act Art 72 · NIST AI RMF MANAGE 4.1, MANAGE 4.2, MEASURE 3.1, MEASURE 4.3, MEASURE 3.2, MEASURE 4.2, GOVERN 5.1, GOVERN 5.2 · OWASP ASI10
- **Reduces risk:** R-12, R-14, R-19
- **Evidence:** Monitoring dashboard and monthly review minutes
- **Test:** Review the last three monthly reviews for actions and owners


## Lifecycle

### G-35: Pre-release evaluation gate

**What it does.** No release without passing the evaluation suite: injection set, scope set, accuracy set, safety set, regression. Results are stored. The gate is a required step in the pipeline.

- **Enforced in:** C13 (Evaluation suite)
- **Maps to:** ISO 27001 8.29, 8.25, 8.31 · EU AI Act Art 9, Art 15 · NIST AI RMF MEASURE 1.1, MEASURE 1.2, MEASURE 2.1, MEASURE 2.3, MEASURE 2.6, MEASURE 2.7, MEASURE 2.13, MAP 2.3, MEASURE 4.1 · OWASP ASI01, ASI02
- **Reduces risk:** R-01, R-02, R-06, R-12, R-14, R-16, R-19
- **Evidence:** Evaluation reports per release; pipeline configuration
- **Test:** Try to release with a failing test: pipeline blocks

### G-36: Change management for prompts, models and tools

**What it does.** Prompts, models, tools and policies are versioned, reviewed, tested and can be rolled back. A model-provider update counts as a change.

- **Enforced in:** C7, C9, C12, C13 (Coordinator agent; MCP tool servers (A, B, C); Model layer; Evaluation suite)
- **Maps to:** ISO 27001 8.32, 8.9, 5.37 · EU AI Act Art 17 · NIST AI RMF MANAGE 4.2, MEASURE 4.3 · OWASP ASI10, ASI04
- **Reduces risk:** R-06, R-12, R-15, R-16, R-17, R-20
- **Evidence:** Change tickets; version history; rollback test
- **Test:** Roll back a prompt version in a test environment within the target time

### G-37: Secure development and vulnerability management

**What it does.** Code and tool servers go through code review, dependency and secret scanning and security testing. Vulnerabilities are patched within set times.

- **Enforced in:** C7, C9, C13 (Coordinator agent; MCP tool servers (A, B, C); Evaluation suite)
- **Maps to:** ISO 27001 8.8, 8.25, 8.28, 8.4, 8.33 · EU AI Act Art 15 · NIST AI RMF MEASURE 2.7 · OWASP ASI05, ASI04
- **Reduces risk:** R-05, R-07
- **Evidence:** Scan reports; patch-time metrics
- **Test:** Seed a vulnerable dependency and a test secret: both caught in the pipeline

### G-38: Backup and recovery of agent configuration and memory

**What it does.** Prompts, policies, tool registers and store contents are backed up and restores are tested.

- **Enforced in:** C11, C7 (Session and memory store; Coordinator agent)
- **Maps to:** ISO 27001 8.13, 5.30 · EU AI Act Art 15 · NIST AI RMF MANAGE 2.3 · OWASP ASI08
- **Reduces risk:** R-10, R-17
- **Evidence:** Backup schedule; restore test report
- **Test:** Restore to a clean environment and run the smoke tests

### G-39: Decommissioning

**What it does.** Retirement plan covers revoking identities, deleting or archiving memory and logs by schedule, and informing customers and staff.

- **Enforced in:** ORG, C11 (Organisation (not a runtime component); Session and memory store)
- **Maps to:** ISO 27001 8.10, 5.11 · EU AI Act Art 18 · NIST AI RMF GOVERN 1.7 · OWASP ASI10
- **Reduces risk:** R-05, R-13
- **Evidence:** Decommissioning checklist
- **Test:** Walk-through of the checklist on a test system


## Governance

### G-40: AI system inventory and classification record

**What it does.** A register entry records purpose, scope, owner, data, models, suppliers and the EU AI Act classification with reasons. A scope change reopens the classification.

- **Enforced in:** ORG (Organisation (not a runtime component))
- **Maps to:** ISO 27001 5.9, 5.1 · EU AI Act Art 6, Art 49 · NIST AI RMF GOVERN 1.6, MAP 1.1, MAP 1.3, MAP 3.3 · OWASP ASI10
- **Reduces risk:** R-12, R-15
- **Evidence:** Register entry; classification memo (see 02-eu-ai-act-classification.md)
- **Test:** Change the scope in the register: classification review is triggered

### G-41: Legal and regulatory register

**What it does.** A maintained list of applicable obligations (EU AI Act, GDPR, DORA, NIS2, consumer and sector rules) with owners and review dates.

- **Enforced in:** ORG (Organisation (not a runtime component))
- **Maps to:** ISO 27001 5.31, 5.36, 5.32 · EU AI Act Art 4, Art 95 · NIST AI RMF GOVERN 1.1, GOVERN 1.2, GOVERN 1.4 · OWASP ASI10
- **Reduces risk:** R-15
- **Evidence:** Register; review minutes
- **Test:** Check each obligation has an owner and a review date

### G-42: Risk assessment, treatment and acceptance

**What it does.** Risks are assessed with an agreed method, treated with guardrails, and residual risk is accepted in writing by a named owner. A data-protection impact assessment is completed. A fundamental-rights style assessment is done voluntarily and becomes required if the scope reaches Annex III 5(b) or 5(c).

- **Enforced in:** ORG (Organisation (not a runtime component))
- **Maps to:** ISO 27001 5.4, 5.8 · EU AI Act Art 9, Art 27 · NIST AI RMF MAP 1.5, MAP 5.1, MANAGE 1.1, GOVERN 4.2, MANAGE 1.2, MANAGE 1.3, MANAGE 1.4, GOVERN 1.3, GOVERN 1.4, GOVERN 2.3 · OWASP ASI10
- **Reduces risk:** R-15
- **Evidence:** Risk register (05); acceptance records; DPIA
- **Test:** Check every high residual risk has an accepted decision

### G-43: Roles, human-oversight competence and AI literacy

**What it does.** Named roles for system owner, oversight reviewers and approvers. Staff who approve, monitor or supervise the agent are trained on how it fails (automation bias, injection, scope) and have authority to override and stop it.

- **Enforced in:** ORG, C16 (Organisation (not a runtime component); Human approval and handoff console)
- **Maps to:** ISO 27001 5.2, 6.3 · EU AI Act Art 4, Art 14, Art 26 · NIST AI RMF GOVERN 2.1, GOVERN 2.2, GOVERN 3.1, MAP 3.4, MAP 3.5, MAP 1.2 · OWASP ASI09
- **Reduces risk:** R-11, R-19
- **Evidence:** Role descriptions; training records; competence checks
- **Test:** Sample approvers: training current and a scenario test passed

### G-44: Documentation and transparency to the people who operate it

**What it does.** A system description, a model/system card, the limits of the agent and instructions for approvers are written and kept current.

- **Enforced in:** ORG (Organisation (not a runtime component))
- **Maps to:** ISO 27001 5.37 · EU AI Act Art 11, Art 13 · NIST AI RMF MAP 2.1, MAP 2.2, MEASURE 2.8, MAP 1.6, MAP 3.2 · OWASP ASI09
- **Reduces risk:** R-11, R-14
- **Evidence:** Documentation set under change control
- **Test:** Ask a new approver to explain limits from the documents alone

### G-45: Independent review and internal audit

**What it does.** Controls are reviewed by someone independent of the build team at least yearly, and the results go to management.

- **Enforced in:** ORG (Organisation (not a runtime component))
- **Maps to:** ISO 27001 5.35, 5.36 · EU AI Act Art 17 · NIST AI RMF MEASURE 1.3, MEASURE 2.13, GOVERN 1.5 · OWASP ASI10
- **Reduces risk:** R-18
- **Evidence:** Audit plan and report
- **Test:** Check the report exists, is independent and has tracked actions

### G-46: Staff and contractor access

**What it does.** Staff and contractors who can touch the system are screened, bound by confidentiality terms and lose access when they leave or change role.

- **Enforced in:** ORG (Organisation (not a runtime component))
- **Maps to:** ISO 27001 6.1, 6.2, 6.4, 6.5, 6.6, 8.1, 6.7 · EU AI Act Art 15 · NIST AI RMF GOVERN 2.1 · OWASP ASI03
- **Reduces risk:** R-05, R-20
- **Evidence:** HR and access-removal records
- **Test:** Sample leavers: access removed within target
