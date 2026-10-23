# 06: Risk Scoring

How organisations score third-party risk, and how I applied it here. There are three different scores, and mixing them up is a common mistake.

| Score | Question it answers | When |
|---|---|---|
| **Inherent risk tier** | How risky is this *engagement* before we look at the vendor's controls? | Step 2: sets how deep the review goes |
| **Finding severity** | How bad is this one gap? | Step 4: ranks what to fix first |
| **Residual risk** | How risky is it *after* the vendor's actual controls and our conditions? | Steps 4 and 5: drives the decision and who signs it |

## 1. Inherent risk tier

Rate the engagement on a few factors, each Low (1), Medium (2) or High (3), before reading any vendor answers.

| Factor | What to ask | This engagement (customer data, class 2) | Public content only (class 0) |
|---|---|---|---|
| Data sensitivity | What data would the vendor hold? | 3: customer personal data | 1: public |
| Business criticality | What happens if the service stops? | 2: agent falls back to FAQ and human handoff (G-28) | 1 |
| Access and integration | How deeply does it connect to our systems? | 3: connects to our Azure tenant | 2 |
| Regulatory exposure | Which rules apply to this data and service? | 3: GDPR, DORA | 1 |
| Substitutability | How hard is it to replace? | 2: open-format export | 2 |

**Rule used here:** a 3 in data sensitivity or regulatory exposure makes it Tier 1 regardless of the total. Otherwise: total 11 or more is Tier 2, 7 to 10 is Tier 3, below 7 is Tier 4.

| Tier | Depth of review | Typical review cycle | Who signs |
|---|---|---|---|
| **1** (high) | Full questionnaire, evidence reviewed, contract terms, pilot | Annual, plus on any change | Risk committee |
| **2** | Shorter questionnaire, key evidence | Every 12 to 24 months | Business owner and GRC |
| **3** | Short form, certificates | Every 2 to 3 years | GRC |
| **4** | Standard terms only | On renewal | Procurement |

**Result:** sending customer data makes this **Tier 1**. Sending public content only would be **Tier 3** (total 7). The data class, not the vendor, sets the tier. This is why the same vendor can be acceptable for public content and unacceptable for customer data.

## 2. Finding severity

Each gap is rated on what it could cause and how easy it is to trigger.

| Severity | Meaning | Example from this project |
|---|---|---|
| **Critical** | Direct, likely exposure of sensitive data | Vendor A: our text goes to a third party by default (Q-19) |
| **High** | A must-have control is missing | Vendor A: no customer-managed keys (Q-09) |
| **Medium** | A control is weaker than required, or unproven | Vendor B: private connectivity claimed but untested (Q-11) |
| **Low** | Housekeeping | A certificate not yet seen (Q-16) |

A failed gate for customer data is rated High at least.

## 3. Residual risk

Residual risk is scored with the same matrix as the [Project 1 risk register](../project-1-ai-governance-assessment/04-risk-summary.md): likelihood and impact each 1 to 5, multiplied. **Low** is up to 5, **Medium** 6 to 11, **High** 12 to 19, **Critical** 20 or more.

The inherent column assumes a generic external vector database holding customer data with no vendor-specific controls. The vendor columns reflect the answers in [03](./03-assessment-findings.md).

| ID | Risk scenario | Inherent | Vendor A as assessed | Vendor B today | Vendor B target (conditions closed) |
|---|---|---|---|---|---|
| VR-1 | Customer data exposed through a breach or misconfiguration at the vendor | 4 x 5 = 20 Critical | 4 x 5 = 20 Critical | 3 x 5 = 15 High | 2 x 5 = 10 Medium |
| VR-2 | Another tenant reaches our data on shared infrastructure | 3 x 5 = 15 High | 4 x 5 = 20 Critical | 1 x 5 = 5 Low | 1 x 5 = 5 Low |
| VR-3 | Our text reaches a hidden fourth party (for example an embedding service) | 3 x 4 = 12 High | 5 x 4 = 20 Critical | 1 x 4 = 4 Low | 1 x 4 = 4 Low |
| VR-4 | Vendor outage makes retrieval unavailable | 3 x 3 = 9 Medium | 3 x 3 = 9 Medium | 2 x 3 = 6 Medium | 2 x 2 = 4 Low |
| VR-5 | We cannot leave the vendor or get our data back | 3 x 3 = 9 Medium | 3 x 3 = 9 Medium | 2 x 3 = 6 Medium | 1 x 3 = 3 Low |
| VR-6 | Slow incident notification delays containment and our own reporting | 3 x 4 = 12 High | 4 x 4 = 16 High | 3 x 3 = 9 Medium | 2 x 3 = 6 Medium |

**Reading it:**
- **Vendor A** has three Critical scenarios (VR-1, VR-2, VR-3). Its failed gates map directly to them.
- **Vendor B today** peaks at High (VR-1, 15). It is much lower than A mainly because it is single-tenant, has no hidden fourth party and has no third-party embedding.
- **Vendor B target** peaks at Medium (10) if the conditions in [04](./04-decision-memo.md) close and are proven. Customer data still leaves the perimeter, so the impact stays at 5 and the score cannot reach Low.

These are my judgements for a fictional case, not measurements. Target scores assume the conditions are met, and none of them has been tested.

## 4. From score to decision

| Highest residual rating | What it means |
|---|---|
| **Critical** | Do not proceed with this data. |
| **High** | Only with conditions, a named remediation plan, and a time-limited acceptance by the risk committee. |
| **Medium** | Business owner accepts in writing, with GRC agreement and a review date. |
| **Low** | GRC approves. |

Applied here: Vendor A is Critical, so it is **not approved**. Vendor B is High today, so it needs conditions and a risk committee acceptance before customer data is sent. Once its conditions are closed and proven, it is Medium and the business owner can accept it.

## 5. Why not one blended score out of 100

A common draft approach is to start at 100 and subtract points for each weakness. I avoided it because:

- **Weights are arbitrary.** Why is one gap worth 30 points and another 15? Nobody can defend it.
- **Related gaps get counted twice.** An unverified isolation claim and a SOC 2 exception on isolation are one problem, not two.
- **It hides what matters.** A vendor can reach 85 while failing the one gate that should stop the decision.
- **It invites arguing over the number** instead of fixing the gap.

The approach above uses scores where they belong (tiering and residual risk, both with stated scales) and uses **gates** for the must-haves, so a single failure is visible.

Some firms also use outside-in security ratings from commercial rating services. These can be one input to monitoring, but they judge only what is visible from the internet and never replace the evidence above.
