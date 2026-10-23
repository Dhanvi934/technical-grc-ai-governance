# Project 2: Third-Party Vector Database Vendor Risk Assessment

Engineering wants to plug an external managed vector database into an Azure-based AI agent. Data would leave the Azure perimeter. Which of two vendors, if any, is safe to use, for which data, and on what conditions?

> **Status: Illustrative.** Both vendors are **fictional** and every finding is invented to show the method. No real vendor was assessed or is judged here.

## What this is

A vendor risk assessment pack for **Halden Assist**, the fictional customer-service agent from [Project 1](../project-1-ai-governance-assessment). It shows how I would handle a very common GRC request: *a team wants a new supplier, so decide whether to say yes.*

The main ideas:

- **Start from the data.** What kind of data would leave decides how strict the review is.
- **Evidence beats claims.** A vendor saying "we do this" is not the same as a contract or report that shows it.
- **Tiers and scores where they belong.** The data class sets the risk tier, must-have answers are checked as gates, and residual risk is scored on the same matrix as Project 1. There is no blended "72 out of 100".
- **Compare against doing nothing.** The baseline is staying inside Azure, which needs no new supplier.
- **Approve with conditions.** Each condition has evidence, an owner and a "needed by" point.

## Read it (about 10 minutes)

| # | File | What you get |
|---|---|---|
| 1 | [04-decision-memo.md](./04-decision-memo.md) | The recommendation and conditions. Start here. |
| 2 | [01-scope-and-method.md](./01-scope-and-method.md) | The proposal, data classes, options, method and framework anchors |
| 3 | [03-assessment-findings.md](./03-assessment-findings.md) | Gate results and what decided it, with the full answers |
| 4 | [02-vendor-questionnaire.md](./02-vendor-questionnaire.md) | The 27 questions and why each matters |
| 5 | [05-vendor-lifecycle-and-documents.md](./05-vendor-lifecycle-and-documents.md) | The steps organisations follow, who does what, and the documents each step produces |
| 6 | [06-risk-scoring.md](./06-risk-scoring.md) | Inherent tier, finding severity and residual risk scoring, with a worked example |
| 7 | [07-templates.md](./07-templates.md) | Intake form, register entry, risk acceptance form, annual review checklist and exit plan, filled in |

## Result in brief

| | Vendor A (shared SaaS) | Vendor B (dedicated cluster) |
|---|---|---|
| Public content | Conditional | Conditional |
| Customer personal data | **Not approved** (4 gates fail) | **Conditional** (8 conditions; 2 gates need evidence, 2 only partly met) |


## How it connects to Project 1

Uses the component and risk IDs and guardrails G-25, G-26, G-40, G-41 and G-42, and proposes one new component (C17) and one new risk (R-21). See [01](./01-scope-and-method.md).

## Limits

- Fictional vendors and invented evidence. A real review would verify answers and read contracts.
- The questionnaire and gates are my design, not an industry standard.
- Regulatory points (GDPR, DORA) are my reading and need legal confirmation.
- Scores are my judgements on a fictional case. There is no single blended score, so ranking many vendors at once is harder.

