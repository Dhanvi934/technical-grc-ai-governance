# 05: Agentic AI and Non-Human Identities

**From my article:** *The Identity Crisis of Agentic AI: Why Machine Credentials & Continuous SIEM Telemetry Are the New Security Perimeter* (Substack, 11 Oct 2026). [PDF](../articles/05-identity-crisis-of-agentic-ai.pdf)

The article makes the argument. This file is the reference version: definitions, the risk list, the controls, the reference architecture, and where the frameworks do and don't reach.

**Core idea:** every action an AI agent takes runs under a credential. So governing an agent is, to a large extent, governing a non-human identity and watching what it does.

---

## 1. What an agent is

A chatbot answers. An agent runs a loop:

1. **Perceive:** read its goal and inputs (emails, documents, web pages, tool results).
2. **Plan:** decide the next step.
3. **Select a tool:** choose an API, database or function.
4. **Act:** execute it.
5. **Evaluate:** check the result and go again.

Nothing in the loop requires a human. That is the value of an agent and also the security problem.

## 2. The risk vocabulary: OWASP Top 10 for Agentic Applications (2026)

OWASP publishes a risk list for agentic systems (ASI01 to ASI10). It's a taxonomy of risks, not a certifiable standard. The ones I use most:

| ID | Risk | Where it shows up in the loop |
|---|---|---|
| ASI01 | Agent Goal Hijack | Untrusted input (step 1) redirects what the agent is trying to do |
| ASI02 | Tool Misuse and Exploitation | A legitimate tool is used unsafely (step 3) |
| ASI03 | Identity and Privilege Abuse | The credential the agent acts under is over-privileged or inherited |
| ASI05 | Unexpected Code Execution | Generated code runs where it shouldn't (step 4) |

The full list also includes supply chain, memory and context poisoning, insecure inter-agent communication, cascading failures, human-agent trust exploitation and rogue agents. See OWASP for the exact wording.

**How I think about it:** prompt injection (ASI01) is how an attacker steers the loop. ASI03 is how much damage the steering can do. The identity decides the blast radius.

## 3. Non-human identities (NHIs)

An NHI is any identity that isn't a person: API keys, OAuth tokens, service principals, workload identities, managed identities.

- They can't answer MFA prompts, they run around the clock, and they're often created quickly with broad permissions and never reviewed.
- Vendor surveys report machine identities heavily outnumbering human ones. One example is CyberArk's 2025 survey (reporting more than 80 to 1). It's a vendor figure, so I treat the exact ratio with caution. The direction is what matters.
- **Why agents make it worse:** a normal service account runs fixed code, so its behavior is predictable. An agent decides what to do at runtime from inputs that may be untrusted. An over-privileged credential now sits behind something whose next action can be steered.
- This is the **confused deputy** problem: the agent has legitimate authority and an attacker borrows it without stealing the key.

## 4. Controls that follow from the identity view

- **Least privilege, and least agency.** "Least agency" is OWASP's term: limit not only what an agent can access but how much it can do on its own.
- **Short-lived credentials** (for example workload identity federation or managed identities) over long-lived keys in configuration.
- **One identity per agent**, so you can tell agents apart in logs and disable one without breaking the others.
- **Authorize against the user, not just the agent**, so the agent can't be used to reach data the requesting user couldn't.
- **Human approval for high-impact actions** such as deleting data, sending data externally or changing permissions.
- **Telemetry**: log tool calls, decisions and identity events so something can detect and respond (see [07](./07-siem-soar-telemetry.md)).

## 5. A reference architecture: where the control points are

A typical multi-agent system: user interface, edge layer (WAF, DDoS protection, rate limits, API gateway), API, a **coordinator agent**, specialist agents each with their own **tool server (MCP)**, plus an identity provider, authorization service, PII redaction, session store, models (self-hosted or third-party), evaluation suite, cost tracker and observability. Four points decide the security outcome:

1. **Authenticate the human at the edge**, and carry that identity with the request.
2. **Authorize against the user, not just the agent** (the confused deputy defence).
3. **One identity per agent, scoped to its own tool server.** The agent that only reads data never holds a credential that can change or delete it. Sensitive tools get a human approval step.
4. **Observability feeds the SIEM**, so there is telemetry to detect on.

## 6. Microsoft Entra Agent ID (my SC-900 link)

Microsoft's Entra Agent ID treats agents as identities in Entra. From the Microsoft Learn documentation (page dated 1 May 2026):

- The capability is described as generally available, but creating an **agent identity blueprint** and an **agent identity** is labelled **Preview**. Check the current status before relying on it.
- **Conditional Access for agents** has templates such as blocking access for high-risk agent identities.
- **Identity governance for agents** includes access packages and sponsors, so each agent has an accountable human owner.

The point I take from it is not the product. It's that agents can get the same lifecycle controls as people: an owner, scoped access, a policy that can block them, and a review.

## 7. Where the frameworks fit, and where they don't

None of these frameworks was written for autonomous agents, so none has a control that maps cleanly. They apply once the agent's identity is treated as an in-scope identity.

| Framework | How it reaches an agent (my reading) | What it doesn't cover |
|---|---|---|
| **ISO/IEC 27001:2022** (A.5.17 authentication information, A.8.2 privileged access rights, A.8.16 monitoring activities) | Manage the agent's credentials, limit its privileges, monitor its activity | Never mentions agents. Nothing on goal hijack or tool misuse. The auditor must accept your interpretation. (A.8.24 is use of cryptography, not secrets management.) |
| **SOC 2** (CC7.2) | Agent telemetry becomes evidence of monitoring | Doesn't define anomalous agent behavior. Your detection rules fill that in. SOC 2 is an AICPA attestation (US), not an EU framework. |
| **NIST AI RMF** (Govern 1.2, Manage 2.2) | Runtime limits and monitoring help evidence the outcomes | Voluntary outcomes, not technical controls. "Least agency" is OWASP's term, not NIST's. |
| **EU AI Act** (Article 14) | Applies to **high-risk** systems only: oversight, approval gates, a stop mechanism | Says nothing about credentials or tool permissions. Most enterprise agents aren't high-risk by default. |

The practical consequence: write your own agent controls, then show how they satisfy the general requirements the auditor already knows. The framework is the audit hook, and the agent-specific control is yours.

## 8. Agent-specific guidance that is emerging (status as of October 2026)

| Source | What it is | Status |
|---|---|---|
| OWASP Top 10 for Agentic Applications 2026 | Risk taxonomy | Published; not a standard |
| NIST CAISI AI Agent Standards Initiative | Announced 17 Feb 2026, covering standards, protocols and agent security research | Early stage |
| NIST NCCoE concept paper on agent identity and authorization | Applies existing identity standards to agents | Draft concept paper |
| CoSAI agentic identity and access management paper | Treats agents as NHIs with a full IAM lifecycle (March 2026) | Industry guidance |
| UC Berkeley CLTC agentic risk-management profile | Extends the NIST AI RMF to agentic systems (February 2026) | Informational |

I read NIST's initiative page directly. The CoSAI and Berkeley items I know through a Cloud Security Alliance summary, so I should check the primary documents before citing them.

## 9. Open questions

- How should an auditor test "least agency"? What evidence shows an agent's autonomy was limited?
- How should agent identities be scoped in an ISO 27001 ISMS? Are they assets, identities, or both?
- How does continuous approval fit? Health Canada's Predetermined Change Control Plan (see [02](./02-canada-ai-governance-patchwork.md)) treats AI governance as ongoing, and agents need the same
- Which formal standards will come out of NIST's agent work, and how will they relate to ISO/IEC 42001 and the AI RMF?

## Sources

- OWASP, *Top 10 for Agentic Applications 2026*
- NIST, *AI Risk Management Framework (AI RMF 1.0)*
- NIST CAISI, [AI Agent Standards Initiative](https://www.nist.gov/caisi/ai-agent-standards-initiative)
- ISO/IEC 27001:2022, Annex A (check against the standard itself)
- EU AI Act, Article 14 and application timeline, via artificialintelligenceact.eu
- Microsoft Learn, *Microsoft Entra Agent ID* documentation
- Cloud Security Alliance, *AI Agent Standards in 2026: NIST, CoSAI, AARM, and China*
- CyberArk, *2025 Identity Security Landscape* (vendor survey)

