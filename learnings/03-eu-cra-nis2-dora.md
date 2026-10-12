# 03: The EU's Three Cybersecurity Laws (CRA, NIS2, DORA)

**From my article:** *The EU Has Three Cybersecurity Laws. And They're Actually Designed to Work Together.* (Substack, 29 Sep 2026). [PDF](../articles/03-eu-three-cybersecurity-laws.pdf)

**Core idea:** the EU treated cybersecurity regulation as **one system with three components**, not three unrelated rules that share a subject. The laws cross-reference each other by article number, route reports through shared institutions, and state which one wins when scopes overlap.

---

## 1. The three instruments

| | **Cyber Resilience Act (CRA)** | **NIS2** | **DORA** |
|---|---|---|---|
| **Legal text** | Regulation (EU) 2024/2847 | Directive (EU) 2022/2555 | Regulation (EU) 2022/2554 |
| **Regulates** | **Products** with digital elements sold into the EU (routers, connected toys, fitness apps, industrial sensors) | **Organizations** across 18 sectors (energy, transport, banking, health, digital infrastructure, public administration and more) | **Financial entities**: banks, insurers, investment firms, trading venues, crypto-asset providers under MiCA |
| **Main requirement** | Security by design, a documented vulnerability-handling process for the support lifetime, CE marking backed by a conformity assessment that scales with risk (self-assessment for ordinary products; third-party notified body for "important" and "critical" classes) | An all-hazards risk-management program: incident handling, business continuity, supply-chain security, MFA, encryption policy. Entities are "essential" or "important", mostly by headcount and revenue | The same kind of risk-management program as NIS2, **plus** mandatory threat-led penetration testing (TLPT) every three years for systemically important institutions, **plus** direct EU-level oversight of critical third-party ICT providers |
| **Key dates** | In force 10 Dec 2024. Reporting obligations mandatory 11 Sep 2026. Full requirements and CE marking 11 Dec 2027 | Transposition deadline 17 Oct 2024 | Applicable since 17 Jan 2025 |
| **Penalties** | Up to €15M or 2.5% of global turnover for the most serious violations | €10M or 2% (essential); €7M or 1.4% (important). Management can be held personally accountable, including temporary suspension of a CEO's authority for an essential entity | No single harmonized cap for financial entities (national law applies). For designated critical ICT providers: periodic penalties up to 1% of average daily worldwide turnover per day of non-compliance |

**CRA reporting cadence:** actively exploited vulnerabilities and severe incidents must be reported to ENISA and national CSIRTs on a **24-hour / 72-hour / 30-day** schedule. ENISA's reporting platform went live on the same day the obligation began.

## 2. How they interlock

This is the structural point of the article, and it's written into the texts:

- **DORA is *lex specialis* to NIS2.** Where DORA's finance-specific risk-management, incident-reporting and testing rules apply, they replace NIS2's general versions for the same entities. NIS2 remains the backstop for anything DORA doesn't cover.
- **The CRA imports NIS2's definitions** of "incident" and "near miss" by direct cross-reference (Articles 3(43) and 3(45)).
- **CRA reports route to the CSIRT that NIS2 designates as coordinator** (CRA Article 14), and technical reporting feeds NIS2's EU-CyCLONe crisis network and Cooperation Group.
- **The CRA reaches into other instruments:** a certified "substantial assurance" product under the EU Cybersecurity Act creates a presumption of CRA conformity, and the same notified bodies can act as competent bodies for high-risk AI systems under the EU AI Act.

**In short:** the EU didn't just write three laws on the same problem. It wrote three laws that *know about each other*.

## 3. The sharpest contrast: who watches the cloud vendor

A bank's technology risk can quietly become its cloud provider's risk.

- **Canada (OSFI B-13):** guidance stops at the bank. The bank must contractually flow security requirements to vendors, and that's as far as OSFI's authority reaches. OSFI has no statutory power to fine, inspect or sanction the vendor.
- **EU (DORA):** goes past the bank. On 18 Nov 2025 the EU's financial supervisors designated **19 providers**, including AWS, Microsoft and Google Cloud, as **Critical ICT Third-Party Providers**, putting them under direct "Lead Overseer" supervision: inspections, information requests, binding recommendations on contracts and subcontracting, enforceable with daily penalties.

## 4. What I took from it

- Real regulatory design means cross-references, shared institutions and explicit precedence rules. Without them you get overlapping reporting duties and unclear authority.
- Deadlines and penalties are what make a law real. The EU's dates have already produced real compliance scrambles.
- **The enforceability gap** is the useful lens: a regulator that can only reach the regulated firm, not its critical suppliers, has a blind spot.
- Designed doesn't mean costless. Industry groups already flag 2027 as tight for the CRA because much of the technical standard-setting is still in draft, and DORA's oversight regime is barely a year old.

## 5. To verify

- The "around 160,000 organizations in scope of NIS2" figure. It's commonly repeated and traces to a Commission impact assessment, but I couldn't pin it to a confirmed primary source, so I treat it as an estimate.
- The DORA critical-provider designation date and the count of 19, against the supervisors' published list
- Whether the CRA reporting platform and dates have changed
- National transposition status of NIS2 in individual Member States

## Related

- [02: Canada's AI governance patchwork](./02-canada-ai-governance-patchwork.md) is the "accumulated, not designed" comparison point.
- [04: India's layered cyber governance](./04-india-layered-cyber-governance.md) is a third pattern: a patchwork by choice.

