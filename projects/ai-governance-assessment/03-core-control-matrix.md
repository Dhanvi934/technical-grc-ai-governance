# 03: Core Control Matrix

The full design has 46 guardrails. These 12 are the core: if you read only these, you understand the design. Each maps to ISO/IEC 42001 Annex A controls, EU AI Act articles, NIST AI RMF subcategories and an OWASP agent risk. **"Maps to" means contributes evidence toward, not satisfies.** The mapping is my interpretation, not an official crosswalk.

★ marks the guardrails I plan to implement as tested code in Project 2. A "+" means more mappings are listed in [reference A](./reference/A-full-guardrail-crosswalk.md).

| ID | Guardrail | What it does | ISO 42001 | EU AI Act | NIST AI RMF | OWASP |
|---|---|---|---|---|---|---|
| ★ G-02 | Authorise against the user, not just the agent | Every tool call carries the end-user's identity. | - | Art 15 | MEASURE 2.7, MAP 3.3 | ASI03, ASI02 |
| G-03 | One workload identity per agent, no shared credentials | The coordinator and each specialist run under their own non-human identity. | A.4.2 | Art 15 | GOVERN 1.6, GOVERN 2.1 | ASI03, ASI10 |
| ★ G-06 | Action risk tiering | Every tool is assigned a tier (0 to 3, plus Never). | A.6.2.2, A.9.4 | Art 14, Art 9 | MAP 3.5, MAP 5.1 + | ASI02 |
| ★ G-09 | Human approval for Tier 3 actions | Tier 3 actions are held until a trained staff member approves the exact action shown to them (not a summary written by the agent). | A.9.2, A.3.2, A.6.2.6 | Art 14 | GOVERN 3.2, MAP 3.5 + | ASI09, ASI02 |
| G-13 | Prompt-injection and untrusted-content defences | User text, retrieved documents and tool output are treated as data, never as instructions. | A.6.2.2, A.6.2.4 | Art 15 | MEASURE 2.7, MEASURE 2.6 | ASI01, ASI06 |
| G-14 | Scope boundary and refusal of regulated decisions | The agent refuses credit, lending, insurance-pricing, investment and legal-advice requests and hands over to a person. | A.9.4, A.6.2.2 | Art 6, Art 5 | MAP 1.1, MAP 3.3 + | ASI01 |
| G-15 | Disclose that the customer is talking to AI | The chat window and the first message state that this is an AI system, and the agent says so when asked. | A.8.2, A.8.5, A.10.4 | Art 50(1) | GOVERN 1.4, MEASURE 2.8 | ASI09 |
| G-20 | PII redaction before model and in logs | Account numbers, card numbers and national identifiers are replaced with tokens before the text reaches a model or a log. | A.7.6, A.7.2 | Art 10 | MEASURE 2.10 | ASI01 |
| G-25 | Agentic supply-chain control | Models, prompts, tool servers, MCP servers and libraries are pinned to versions and verified (hash or signature). | A.10.3, A.4.2, A.4.4 | Art 25, Art 15 | GOVERN 6.1, MAP 4.1 + | ASI04 |
| ★ G-29 | Structured, tamper-evident action log | Every agent step is logged: time, customer (tokenised), agent identity, tool, parameters (redacted), tier, approval, outcome. | A.6.2.8 | Art 12, Art 19 | MEASURE 2.4, MANAGE 4.1 | ASI10 |
| G-31 | SIEM detection and SOAR response for agent behaviour | Detection rules for: unusual tool-call volume, cross-customer access attempts, repeated authorisation denials, instruction-like text in data, new tools or identities, off-hours Tier 3 requests. | A.6.2.6 | Art 72, Art 26 | MEASURE 2.4, MANAGE 4.1 | ASI10, ASI03 + |
| G-32 | Kill switch and agent suspension | Staff can suspend one agent, one tool or the whole agent system within minutes, and revoke credentials at the same time. | A.6.2.6, A.9.2 | Art 14, Art 26 | MANAGE 2.4, MANAGE 2.3 | ASI10, ASI08 |


## Coverage

| Framework | Items | Mapped to a guardrail | Other |
|---|---|---|---|
| ISO/IEC 42001:2023 Annex A | 38 | 35 | 2 organisation-level, 1 excluded (with reason) |
| NIST AI RMF 1.0 | 72 | 64 | 7 stated gaps, 1 organisation-level |
| OWASP Agentic Top 10 | 10 | 10 | Each has a guardrail and a risk-register entry |


A high count shows the design was checked against the frameworks, not that the controls work. Everything is at status *Designed*.

## Why ISO 42001

ISO 42001 is the standard for managing AI systems, so it is the ISO standard used here. 10 guardrails (authentication, authorisation, network segmentation and similar security controls) have no direct 42001 counterpart; they map to NIST AI RMF, the EU AI Act and OWASP instead. ISO 27001 is kept only as an optional appendix.

## Go deeper

[A: all guardrails](./reference/A-full-guardrail-crosswalk.md) · [B: ISO 27001 mapping (optional appendix)](./reference/B-iso-27001-statement-of-applicability.md) · [I: ISO 42001 applicability](./reference/I-iso-42001-applicability.md) · [C: EU AI Act applicability](./reference/C-eu-ai-act-applicability.md) · [D: NIST AI RMF applicability](./reference/D-nist-ai-rmf-applicability.md) · [E: guardrails specification](./reference/E-guardrails-specification.md)
