# 02: EU AI Act Classification

My reasoning on how the EU AI Act (Regulation (EU) 2024/1689) classifies this agent. **Interpretation for a learning project, not legal advice.** A longer version is in [reference H](./reference/H-full-eu-ai-act-memo.md).

![Classification flow](./images/02-eu-ai-act-classification-flow.png)

**Not high-risk, provided the scope holds.** High-risk uses are listed in Annex III. The nearest are 5(b), evaluating creditworthiness or credit scores of individuals, and 5(c), risk assessment and pricing for life and health insurance. This agent does neither: it answers questions and performs bounded service tasks. Guardrail G-14 enforces that line, and more importantly no tool exists that could make such a decision.

**What still applies:** Art 50(1) requires that people are told they are dealing with an AI system (applies from 2 Aug 2026; guardrail G-15). Art 4 requires measures to support AI literacy of staff. Art 5 bans manipulative or vulnerability-exploiting practices, which a customer-facing agent must design against (G-16).

**Voluntary design targets:** Arts 9 to 15 and 17, and deployer practice under Art 26, are written for high-risk systems. I apply them anyway because they are a good published method for risk management, logging and human oversight, and because they would be needed at once if scope grew (Art 95 encourages this).

## What would change the answer

| Change | Effect |
|---|---|
| Agent gives credit-limit or loan-eligibility guidance that feeds a decision | Likely Annex III 5(b): high-risk |
| Agent prices or advises on life or health insurance | Annex III 5(c): high-risk |
| Agent infers customers' emotions | High-risk under Annex III 1(c), with Art 50(3) disclosure; Art 5(1)(f) bans it only at work and in education |


When scope changes, re-run this classification (G-40, risk R-15).

## Caveats

- The AI Omnibus moved the Annex III date to 2 Dec 2027. I read this in law-firm summaries, not the Official Journal. Check it before planning around it.
- Whether Art 50(2) marking of synthetic content applies to chat replies is an open question.
- GDPR, DORA and NIS2 apply alongside and are not assessed here.

Sources: [Regulation (EU) 2024/1689 on EUR-Lex](https://eur-lex.europa.eu/eli/reg/2024/1689/oj); AI Omnibus law-firm summaries.
