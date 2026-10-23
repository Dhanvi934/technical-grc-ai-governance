# 07: Templates, Filled In

Five short documents a third-party risk programme normally keeps, completed for this fictional case so you can see what goes in each. Names, dates and approvals are placeholders. Nothing is signed.

---

## 1. Vendor intake form

| Field | Entry |
|---|---|
| Request ID | TPR-0001 (placeholder) |
| Requested by | Head of AI Engineering |
| Vendor | Ferrule Vector Dedicated (fictional) |
| Service | Managed vector database for retrieval in Halden Assist |
| Business reason | Faster retrieval over a larger document set |
| Alternatives considered | Azure AI Search inside the tenant (option 0); Vendor A |
| Data to be sent | Phase 1: public content (class 0). Phase 2: customer documents (class 2) |
| Systems connected | Azure tenant (Azure OpenAI, Azure AI Search, Microsoft Entra ID); agent retrieval path (Project 1, C7 to C9) |
| Where data is processed | EU (requested) |
| Planned go-live | After conditions in [04](./04-decision-memo.md) are closed |
| Contract value and term | To be filled by procurement |
| Fallback if the service is down | Static FAQ and human handoff (Project 1, G-28) |
| Initial tier | Tier 1 for class 2; Tier 3 for class 0 (see [06](./06-risk-scoring.md)) |
| Regulatory flags | GDPR (personal data); DORA (financial entity, ICT third party); EU AI Act classification to be rechecked (G-40) |

## 2. Vendor register entry

This is the record a regulator would ask for. In the EU financial sector it feeds the DORA register of information.

| Field | Entry |
|---|---|
| Vendor | Ferrule Vector Dedicated (fictional) |
| Service provided | Managed vector database, dedicated cluster |
| Supports which Halden service | Halden Assist retrieval |
| Data held | Vectors and tokenised IDs; no source text (condition C-6) |
| Data location | EU |
| Sub-processors | Cloud host only, EU |
| Tier | 1 (class 2) |
| Status | Conditionally approved: phase 1 only |
| Residual risk (today / target) | High 15 / Medium 10 |
| Business owner | Head of AI Engineering |
| Risk owner for the vendor | Vendor manager |
| Contract start and end | To be filled |
| Key evidence on file | SOC 2 Type II report (scope gap noted), ISO 27001 certificate, pen test summary (under NDA), draft DPA |
| Open conditions | C-1 to C-8 in [04](./04-decision-memo.md) |
| Exit strategy | Export to Azure AI Search; tested before class 2 (C-7) |
| Next review | 12 months, or on any change of scope, data or sub-processor |

## 3. Risk acceptance form (draft, unsigned)

Used when residual risk above Low is knowingly accepted.

| Field | Entry |
|---|---|
| Risk | VR-1: Customer data exposed through a breach or misconfiguration at the vendor |
| Residual rating when accepted | High (15) today; Medium (10) target |
| Why it cannot be removed | Customer data would leave the Azure perimeter by design |
| Controls in place or required | C-1 to C-8 in [04](./04-decision-memo.md) |
| What was considered instead | Staying in Azure AI Search; Vendor A |
| Duration | 12 months, then re-reviewed |
| Conditions of acceptance | Phase 2 starts only when C-1 to C-7 are evidenced |
| Accepted by | [Business owner, name and role, date] |
| Reviewed by | [GRC, DPO, security, date] |
| Approved by (for High) | [Risk committee, date] |
| Review trigger | New incident, scope change, loss of certification, expiry of this acceptance |

## 4. Annual review checklist

| Check | Result |
|---|---|
| Still needed and still used? | |
| Data class or volume changed since approval? | |
| New SOC 2 report received; scope still covers our service; exceptions reviewed | |
| Certificates in date | |
| Sub-processor changes since last review, and whether we agreed | |
| Incidents at the vendor, and what we learned | |
| Service levels met (availability, recovery) | |
| Penetration test summary refreshed | |
| Open conditions closed or re-dated | |
| Contract terms still adequate (incident notice, audit, exit) | |
| Register entry and risk scores updated | |
| Decision: continue, continue with conditions, or exit | |

## 5. Exit plan outline

| Item | Plan |
|---|---|
| Triggers | Failed review, serious incident, loss of certification, vendor insolvency, scope change |
| Alternative | Azure AI Search inside the tenant |
| Data return | Export of all vectors and metadata in an open format within an agreed time; format checked in advance |
| Rebuild | Source documents stay in Azure, so vectors can be recreated if an export fails |
| Transition | Run both in parallel for an agreed period; switch retrieval over once answers are checked |
| Deletion | Vendor deletes all copies, including backups, and provides a written confirmation |
| Access | Private endpoint removed, keys revoked, credentials rotated |
| Records | Register entry closed; confirmation and test results filed |
| Test | Rehearse the export and restore before the vendor is used for customer data (C-7) |
