# 04: Decision Memo

**To:** Head of AI Engineering; Risk and Compliance Committee
**From:** Technical GRC Engineer
**Re:** External vector database for Halden Assist retrieval
**Status:** Draft for a fictional firm. Vendors and findings are invented to illustrate the method.

## Decision requested

Approve or reject the proposal to send Halden Assist documents to an external managed vector database, and set the conditions.

## Recommendation

1. **Do not approve Vendor A (Skein Vector Cloud) for Halden Assist.** It fails four must-have gates for customer data: no customer-managed keys, no private connectivity, a third-party service receives our text by default, and incident notification takes 72 hours. It also has an unresolved SOC 2 exception on separation between customers.
2. **Conditionally approve Vendor B (Ferrule Vector Dedicated), in two phases.**
   - *Phase 1:* public content only (data class 0), after a short pilot.
   - *Phase 2:* customer personal data (data class 2), only after the conditions below are closed and evidenced.
3. **Keep Azure AI Search as the baseline.** Before go-live, engineering shows with measurements that the latency gain is large enough to justify a new supplier. If it is not, the lowest-risk option is not to send data out.

## Why

- **Vendor B's design matches the risk.** Dedicated cluster, customer-managed keys, Azure Private Link, EU only, and no third party sees our text.
- **But the evidence is incomplete.** Its SOC 2 report does not yet cover dedicated clusters, two key controls are only claimed, and its incident notice (48 hours) is slower than Halden would want, given its own reporting duties.
- **Embeddings should be treated as personal data** when built from personal data, so a leak of vectors could be a leak of customer information.
- **A second supplier adds oversight work.** Using Vendor A for public content only is possible, but I would not take on two new suppliers for that.

## Conditions for Vendor B (customer data)

| # | Condition | Evidence required | Owner | Needed by |
|---|---|---|---|---|
| C-1 | Private connectivity through Azure Private Link, public endpoint disabled | Pilot test showing public access is refused | Cloud platform lead | Pilot (public data only) |
| C-2 | Customer-managed key in Halden's Azure Key Vault; revoking the key stops access | Key revocation test | Cloud platform lead | Before class 2 |
| C-3 | SOC 2 coverage of dedicated clusters | Next report, or a bridge letter, plus a penetration test summary in the meantime | Vendor manager | Before class 2 |
| C-4 | Incident notification within 24 hours, with help for investigations | Signed contract clause | Legal | Before class 2 |
| C-5 | Audit logs exported to Halden's SIEM (C15) | Working feed and detection rules (G-31) | Security operations | Before class 2 |
| C-6 | Minimal data: vectors and tokenised IDs only; source text stays in Azure; no customer identifiers in metadata (G-20) | Data flow review | AI engineering | Before class 2 |
| C-7 | Exit tested: export from the vendor and restore into Azure AI Search | Exit test record | AI engineering | Before class 2 |
| C-8 | Records updated: DORA register of information, AI system inventory (G-40), risk register (R-21), architecture (C17), data protection impact assessment (G-42) | Updated documents | GRC | Before any data |

## Residual risk

Scored in [06](./06-risk-scoring.md) on the Project 1 matrix: Vendor A has three Critical scenarios, Vendor B peaks at High today (15) and Medium (10) once its conditions are closed.

With all conditions closed, I expect the third-party data exposure risk to sit at medium, as for the model provider in Project 1. That is a target, not a result. A named business owner must accept the residual risk in writing (G-42) before phase 2.

## What would change this recommendation

- **Vendor A closes its gaps.** Customer-managed keys, private connectivity, a way to turn off third-party processing, 24-hour notification, and a clean SOC 2 re-test would let us reassess.
- **Vendor B fails the pilot.** If Private Link or key revocation does not work as claimed, phase 2 stops.
- **Scope grows.** If retrieval is ever used to support credit or insurance decisions, the EU AI Act classification changes and this whole assessment is redone.
- **Measurements show no real speed gain.** Then option 0 wins.

## Limits of this assessment

- The vendors and their facts are fictional. The method is the point.
- Answers marked *claimed* have not been checked. Real due diligence would verify them.
- I did not review any contract text, and nothing here is legal advice. Legal must confirm the GDPR and DORA points.
