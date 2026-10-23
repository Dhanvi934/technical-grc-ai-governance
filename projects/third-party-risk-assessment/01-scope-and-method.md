# 01: Scope and Method

## The proposal

Halden Assist ([Project 1](../project-1-ai-governance-assessment)) lives in a Microsoft Azure environment: Azure OpenAI for the model, Azure AI Search for retrieval and Microsoft Entra ID for identity. The AI engineering team now wants retrieval over more documents and has proposed an **external managed vector database** for speed. It would sit outside the Azure tenant, so Halden's data would leave the perimeter.

The question for the GRC engineer: **should data leave, and if so, to whom and on what terms?**

## This is a change to the system, not just a purchase

Project 1 documents what Halden Assist is allowed to do and which suppliers it uses. A new data store changes all of that, so the review also covers:

- A **new component** (call it C17, the external vector store) and a new data flow out of Azure.
- A **new supplier**, so guardrails G-25 (supply chain) and G-26 (third-party providers) apply to it.
- A **reopened classification**: G-40 says a scope change reopens the EU AI Act classification. Adding retrieval over documents does not by itself add credit or insurance decisions, so the conclusion in [02](../project-1-ai-governance-assessment/02-eu-ai-act-classification.md) should hold. It still needs to be checked and recorded.
- A **new risk** for the register (proposed R-21: third-party vector store exposes or leaks customer data), next to R-06 (compromised supplier), R-13 (data leaks to a provider) and R-17 (provider outage).

## What would go to the vendor

The sensitivity of the data decides how strict the review is.

| Data class | Content | Why it matters |
|---|---|---|
| **0: Public** | Product pages, fee schedules, public policies | Low sensitivity. The risks are availability and tampering with answers. |
| **1: Internal** | Staff procedures, internal knowledge articles | Not reviewed separately here. Treated as class 2 if it names customers. |
| **2: Customer personal data** | Statements, case notes, messages | Personal data. Full review. |

Embeddings are not harmless. Research has shown that text can often be partly reconstructed from its embeddings ([Morris et al., 2023](https://arxiv.org/abs/2310.06816)), and OWASP lists vector and embedding weaknesses in its 2025 Top 10 for LLM applications (LLM08). So the approach here is to **treat embeddings of personal data as personal data**.

## Options

| Option | Description | Comment |
|---|---|---|
| **0: Stay in Azure** | Use Azure AI Search inside the tenant | No new supplier, no data leaves. The baseline everything else must beat. The "faster" claim should be measured before any risk is accepted. |
| **1: Vendor A** | Shared multi-tenant SaaS | Assessed below. |
| **2: Vendor B** | Dedicated single-tenant cluster | Assessed below. |

## Method

1. **Define the data class** that would be sent. The answer to everything else depends on it.
2. **Ask the same 27 questions** of both vendors ([02](./02-vendor-questionnaire.md)).
3. **Record each answer with its evidence level.** *Document seen* beats *claimed* beats *none*.
4. **Apply gates.** A gate is a must-have for a data class. Customer personal data has ten gates; public content has three.
5. **Score the residual risk** on the same matrix as Project 1 ([06](./06-risk-scoring.md)).
6. **Decide with conditions.** Approved, conditional (with named conditions, owners and a go-live block) or not approved.

I did not use one blended score such as "72 out of 100". It hides which weakness matters, double-counts related findings and invites arguing over weights. Scores are used where they belong, for the engagement's risk tier and for residual risk ([06](./06-risk-scoring.md)), and gates show plainly what blocks a decision. A small script, [`scripts/check_evidence.py`](./scripts/check_evidence.py), applies the gates to [`data/vendor-evidence.json`](./data/vendor-evidence.json) and prints the result. It shows what has and has not been proven. It does not decide anything.

## Framework anchors (my reading, not an official crosswalk)

| Source | Where it applies |
|---|---|
| GDPR | Art 28 (processor contract), Art 32 (security), Chapter V (transfers outside the EU) |
| DORA | Arts 28 and 30 (managing ICT third-party risk and key contract terms), including the register of information; whether this service supports a critical or important function is a call for the firm |
| ISO/IEC 42001 Annex A | A.10.3 Suppliers; A.10.2 Allocating responsibilities |
| NIST AI RMF | GOVERN 6.1 (third-party AI risk by policy), GOVERN 6.2 (contingency for third-party failure) |
| OWASP Top 10 for LLM Applications 2025 | LLM08 Vector and Embedding Weaknesses |
| EU AI Act | Art 10 (data governance) binds high-risk systems only. Halden Assist is not high-risk by the reasoning in Project 1, so it is a design target here, not a duty. |
| Project 1 | G-25, G-26, G-40, G-41, G-42; risks R-06, R-13, R-17 |
