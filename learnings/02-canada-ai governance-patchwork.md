# 02: Canada's AI Governance Patchwork

**From my article:** *Canada Has No AI Law. Here's What's Actually Governing AI Right Now.* (Substack, 26 Sep 2026). [PDF](../articles/02-canada-has-no-ai-law.pdf)

**Core idea:** the absence of a dedicated AI law doesn't mean the absence of rules. It means the rules come from six different directions at once, each covering a slice, none designed to interlock.

---

## 1. What happened to AIDA

The Artificial Intelligence and Data Act (AIDA) was Part 3 of Bill C-27, introduced in 2022 alongside a privacy reform package. It proposed obligations for "high-impact" AI systems (risk assessments, mitigation measures, human oversight) enforced by a new AI and Data Commissioner. It died when Parliament was prorogued on 6 January 2025 and has not returned. The government has signalled a framework that is "light, tight, right" rather than a revival of the bill, with no fixed timeline.

## 2. The six instruments

| Instrument | Type | What it does for AI |
|---|---|---|
| **PIPEDA** | Federal privacy law | Doesn't mention AI, but consent, purpose-limitation and accuracy requirements apply to any AI system using personal information |
| **Quebec Law 25** | Provincial law, in force since Sept 2023 | For decisions made "exclusively automated": notify the person at the time, give a way to reach a human who can review, let them correct the data used, and on request explain the reasons and main factors |
| **Treasury Board Directive on Automated Decision-Making** | Binds federal departments only | Requires an Algorithmic Impact Assessment, a scored questionnaire that sorts a system into four risk tiers (little to no impact up to very high impact). Higher tiers trigger more human review and public disclosure |
| **OSFI Guideline B-13** | Binds federally regulated banks and insurers | Doesn't mention AI, but its technology and cyber risk requirements (segmentation, MFA, disaster recovery) apply to every system including AI fraud detection and credit models. In force since July 2022 |
| **Health Canada ML-enabled medical device guidance** | Explicitly non-binding ("current thinking"), with the binding Medical Devices Regulations beneath | Bias testing, representative training data, subgroup performance evaluation, and a **Predetermined Change Control Plan** that pre-authorizes specific future model updates |
| **ISED Voluntary Code of Conduct on Advanced Generative AI** | Voluntary, launched Sept 2023 as "a critical bridge" until AIDA | Signatories commit to accountability, safety, fairness, transparency, human oversight and robustness testing. Nothing is enforceable |

## 3. Why a patchwork isn't a policy

The six instruments differ on four axes:

- **By sector:** finance, healthcare
- **By jurisdiction:** Quebec but not the rest of the country
- **By actor:** the federal government binds itself, not private industry
- **By choice:** the voluntary code only applies to those who opt in

None was written to solve "how should Canada govern AI". Each solves a narrower problem within its own mandate, so **AI governance is a side effect of each body's real job, not the job itself.**

The practical consequence: two organizations can both be "doing everything required of them" and have very different AI governance. A bank has a mature model-risk program because OSFI made it build one. A retailer has almost nothing because no regulator with authority over it has mentioned AI.

## 4. What fills the gap internationally

- **NIST AI Risk Management Framework:** a four-function process (Govern, Map, Measure, Manage) for thinking about AI risk from first principles. Voluntary.
- **ISO/IEC 42001:** published December 2023. A certifiable AI management system with the same Plan-Do-Check-Act shape as ISO 27001.

Neither is Canadian and neither is mandatory anywhere, but they're what organizations reach for when they want AI governance more complete than "whatever my sector regulator requires". Whether Canadian organizations are actually closing the gap this way is, as far as I could find, an unanswered empirical question.

## 5. What I took from it

- Look for the *adjacent* rules. Privacy, financial-risk and device regulation already govern AI in practice even when they never use the word.
- Health Canada's Predetermined Change Control Plan is a quiet acknowledgment that **AI governance has to be continuous, not a one-time approval**. That idea comes back in [05](./05-agentic-ai-and-nhis.md).
- A voluntary framework is only as strong as the organization's decision to adopt it.

## 6. To verify

- Whether any new federal AI legislation or framework has been announced since 26 Sep 2026
- The current signatory list of the ISED voluntary code
- Quebec Law 25's exact wording on automated decisions

## Related

- [03: The EU's three cybersecurity laws](./03-eu-cra-nis2-dora.md) is the deliberate contrast: a designed system, not a patchwork.
- [01: Canada's critical infrastructure controls](./01-canada-critical-infrastructure-controls.md) covers B-13's technical requirements in detail.

