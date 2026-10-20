# Project 1: AI Governance Assessment for a Customer-Facing AI Agent

A design-level compliance assessment for an AI agent system: **what would it take for an agentic customer-service system to line up with ISO/IEC 27001, the EU AI Act and the NIST AI RMF, and how would you prove it?**


## The scenario

**Halden Assist** is a customer-facing chat agent at **Halden Financial Services (fictional)**, a regulated firm with EU customers. It answers questions, reads the customer's own data and performs bounded service actions through specialist agents and tool servers. Full profile in [01-system-profile.md](./01-system-profile.md).

![Architecture](./images/01-architecture-component-ids.png)

## What is in this project

| File | What it is |
|---|---|
| [01-system-profile.md](./01-system-profile.md) | Scope, roles, data, components (C1 to C16), data flows, trust boundaries, assumptions |
| [02-eu-ai-act-classification.md](./02-eu-ai-act-classification.md) | Why the agent is not high-risk under the EU AI Act, which duties still bind it, and what would change the answer |
| [03-control-matrix.md](./03-control-matrix.md) | The matrix: 46 guardrails mapped to ISO 27001 Annex A, EU AI Act articles, NIST AI RMF subcategories and the OWASP Agentic Top 10 |
| [03a ISO 27001 Statement of Applicability](./03a-iso-27001-statement-of-applicability.md) | All 93 Annex A controls: included, inherited or excluded, with reasons |
| [03b EU AI Act applicability](./03b-eu-ai-act-applicability.md) | Article-by-article: applies, design target, conditional, not applicable |
| [03c NIST AI RMF applicability](./03c-nist-ai-rmf-applicability.md) | All 72 subcategories, with stated gaps |
| [04-guardrails-specification.md](./04-guardrails-specification.md) | Each guardrail: what it does, where it is enforced, evidence, test; illustrative configuration |
| [05-risk-register.md](./05-risk-register.md) | 20 risks and threat model with inherent and residual scores and heat maps |
| [data/](./data) | The same tables as CSV |



## Results at a glance

| Measure | Result |
|---|---|
| Guardrails | 46, in 8 layers, each with evidence and a test |
| ISO/IEC 27001:2022 Annex A | 74 of 93 controls have agent-specific guardrails; 8 apply as baseline ISMS; 8 inherited from providers; 3 excluded with reasons |
| NIST AI RMF 1.0 | 64 of 72 subcategories mapped; 7 stated gaps; 1 organisation-level |
| EU AI Act | Not high-risk (scope excludes Annex III 5(b) and 5(c)); Art 50(1) disclosure, Art 4 literacy and Art 5 checks apply; Arts 9 to 15 and deployer practice adopted as design targets |
| OWASP Agentic Top 10 | 10 of 10 risks covered by guardrails and by the risk register |
| Risk register | 20 risks: inherent 13 high and 7 medium; residual target 11 medium and 9 low if guardrails work as specified |


A high mapped count in a design document shows the design was thought through against the frameworks. It does not show the controls work. The test column in each guardrail says what would have to be demonstrated.

## How I built it

1. **Start from the architecture, not the frameworks.** The components are numbered (C1 to C16).
2. **Write guardrails first.** Each is a thing the system does, with a component, evidence and a test. Reasoning: frameworks do not describe agents, so I wrote the agent controls myself and then asked which framework requirements each one supports.
3. **Map outward.** Each guardrail lists the ISO Annex A controls, EU AI Act articles and NIST subcategories it contributes to, and the OWASP agent risk it reduces.
4. **Do the exclusions honestly.** Every ISO and NIST item that is not covered by a guardrail is listed with a status and a reason.
5. **Generate and check.** All tables come from one data source. A validation script checks that every referenced ID exists, every risk has a guardrail, and no excluded control is also mapped.

## Design principles

- Enforce outside the model: authorisation, limits and network rules live in code, not in a prompt.
- Authorise against the user: the agent never has more rights than the customer.
- One identity per agent, short-lived scoped credentials.
- Tier every action: alone, with confirmation, or with a person's approval.
- Log so an incident can be reconstructed.

## Limitations and known gaps

- **Nothing is implemented or tested.** Status of every guardrail is *Designed*; residual risk scores are targets, not results.
- **The scores are my judgement** on a 1 to 5 scale for a hypothetical system.
- **"Mapped" means contributes toward, not satisfies.** ISO controls are met by the whole management system.
- **ISO management-system clauses (4 to 10)** are not assessed; only Annex A.
- **ISO 27001 text is not reproduced.** Titles are official; descriptions are my words.
- **Dates and Omnibus changes** to the EU AI Act come from secondary sources and should be checked against the Official Journal. The Art 50(2) question is open.
- **Not covered, by design:** GDPR, DORA and NIS2 (noted in 02); ISO/IEC 42001 (the AI management-system standard).
- **NIST AI RMF gaps:** engagement with affected people (MAP 5.2), sustaining value (MANAGE 2.2), environmental impact (MEASURE 2.12), human-subject evaluations (MEASURE 2.2), fairness and bias evaluation (MEASURE 2.11), and the business-case items (MAP 1.4, MAP 3.1).
- **The base architecture** follows a common multi-agent reference pattern; credit its source if you reuse a diagram.

## Reproduce

```bash
python tools/validate.py        # cross-reference checks and coverage numbers
python tools/build.py .         # regenerates the tables and documents from the data files
python tools/gen_figs.py images # regenerates the figures (needs Playwright with Chromium)
```

Edit `tools/data_core.py`, `data_iso.py` or `data_nist.py` to change content, then rebuild. The `.md` files here are generated; edits made directly to them will be overwritten.

## Sources

- ISO/IEC 27001:2022: https://www.iso.org/standard/27001
- EU AI Act, Regulation (EU) 2024/1689: https://eur-lex.europa.eu/eli/reg/2024/1689/oj
- NIST AI Risk Management Framework 1.0: https://www.nist.gov/itl/ai-risk-management-framework
- OWASP Top 10 for Agentic Applications 2026: https://genai.owasp.org/

## Related notes in this portfolio

- [05: Agentic AI and non-human identities](../../learnings/05-agentic-ai-and-nhis.md) is the argument behind the identity and monitoring guardrails.
- [03: The EU's three cybersecurity laws](../../learnings/03-eu-cra-nis2-dora.md) covers DORA and NIS2, which sit next to the AI Act for a financial firm.


