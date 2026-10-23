# 05: The Vendor Lifecycle and the Documents It Produces

How organisations typically bring in a third party, who does what, and which documents exist at each step. This is a general picture from common practice (it varies by firm and regulator), shown next to what this project contains.

## Who is involved

| Role | Part in the process |
|---|---|
| **Business owner / requester** (here: Head of AI Engineering) | Wants the vendor, owns the business case and later accepts residual risk |
| **GRC / third-party risk analyst** (you) | Runs the process: tiering, due diligence, findings, recommendation |
| **Information security** | Reviews technical evidence (architecture, encryption, access, tests) |
| **Privacy / DPO** | Data protection: processor terms, transfers, impact assessment |
| **Legal and procurement** | Contract, negotiation, commercial terms |
| **Risk committee** | Approves higher-risk vendors and time-limited risk acceptances |
| **Vendor manager** | Runs the relationship, monitoring and renewals after go-live |

## The steps

| # | Step | What happens | Documents produced | In this project |
|---|---|---|---|---|
| 1 | **Intake** | The business asks for a vendor. GRC records what, why, which data and which systems. | Vendor intake form; data flow diagram; data classification | [07](./07-templates.md) (filled intake form); [01](./01-scope-and-method.md) |
| 2 | **Inherent risk tiering** | Rate the engagement before looking at the vendor's controls: how sensitive is the data, how critical is the service. The tier sets how deep the review goes. | Tiering record | [06](./06-risk-scoring.md) |
| 3 | **Due diligence** | Send a questionnaire and ask for evidence. Review it with security, privacy and legal. | Questionnaire (or a standard one such as SIG or CAIQ); evidence request list; the vendor's SOC 2 report, penetration test summary, certificates, sub-processor list, business continuity summary, insurance certificate | [02](./02-vendor-questionnaire.md); [`data/vendor-evidence.json`](./data/vendor-evidence.json) |
| 4 | **Assessment** | Compare answers to requirements, rate findings, score the residual risk, compare options including doing nothing. | Assessment report; findings log; risk rating; options analysis | [03](./03-assessment-findings.md); [06](./06-risk-scoring.md) |
| 5 | **Decision** | Approve, approve with conditions, or reject. Conditions get owners and dates. Remaining risk is accepted in writing by the right person. | Decision memo; conditions tracker; risk acceptance form | [04](./04-decision-memo.md); [07](./07-templates.md) |
| 6 | **Contracting** | Legal and procurement turn conditions into contract terms. | Master services agreement; data processing agreement; service levels; security schedule; audit and exit clauses | Conditions C-4, C-7 in [04](./04-decision-memo.md); the contract itself is out of scope |
| 7 | **Onboarding** | Set up access and monitoring, record the vendor in the register, close pre-go-live conditions. | Vendor register entry; onboarding checklist; updated architecture and risk register | [07](./07-templates.md) (register entry) |
| 8 | **Ongoing monitoring** | Review on a schedule set by the tier. Track report renewals, incidents, sub-processor changes, service levels and news. | Annual review; monitoring log; incident record; renewed evidence | [07](./07-templates.md) (annual review checklist) |
| 9 | **Change and incident handling** | Reassess when scope, data or the vendor changes, and when the vendor has an incident. | Change assessment; incident notice and follow-up | Project 3 (the incident runbook) covers the Halden side |
| 10 | **Exit** | Retrieve or delete data, revoke access, close the register entry. | Exit plan; deletion certificate; access revocation record | [07](./07-templates.md) (exit plan outline) |

Tier 1 reviews often take weeks, mostly waiting for the vendor and for contract negotiation. Low tiers are handled in days with a short form.

## What the process is trying to avoid

- **Surprises.** A vendor holding data nobody recorded, or a fourth party nobody knew about.
- **Paper assurance.** Accepting a report that does not cover the service in use.
- **No way out.** A supplier that cannot be left or whose data cannot be recovered.
- **Orphaned risk.** An accepted risk with no named owner and no review date.

## What a regulator or auditor would ask to see

For a financial firm in the EU, expect requests for: the register of ICT third-party arrangements with the services provided and their criticality (the DORA register of information, Art 28(3)), the due diligence done before the contract (DORA Art 28(4)), the contract terms (Art 30), evidence of monitoring after go-live, the exit strategy for important services, and records of risk acceptance. Having each document in step 1 to 10 answers most of these.
