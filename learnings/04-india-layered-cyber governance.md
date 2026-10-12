# 04: India's Layered Cyber Governance

**From my article:** *Beyond the Checklist: What CSCRF, CERT-In, and DPDP Actually Require You to Build* (Substack, 5 Oct 2026). [PDF](../articles/04-beyond-the-checklist-cscrf-cert-in-dpdp.pdf)

**Core idea:** India's rules feel confusing because they were never designed as one system. Read them as **three stacked layers**, plus a **fourth axis** for whatever your foreign client's jurisdiction imposes. Compliance has also shifted from a point-in-time annual ritual to something your systems must be able to prove is true *right now*.

---

## 1. Why it feels different from the EU

The EU's NIS2, DORA and CRA were drafted as one architecture through one institutional process. India has four regulators with four legal bases, written about 26 years apart, each solving its own problem:

- **CERT-In:** authority traces to a section added to the IT Act of 2000
- **DPDP Act:** a 2023 law enforced by a board that didn't exist until November 2025
- **SEBI** and **RBI:** each wrote sector rules on their own schedules, with no statute requiring them to reference each other or the other two

The confusion isn't poor drafting. Nobody drew the connective tissue between them.

## 2. The three-layer model

| Layer | What | Who it applies to |
|---|---|---|
| **Layer 1: CERT-In** | The 2022 Directions: 6-hour incident reporting, 180-day log retention, clock synchronization | Everyone operating in Indian cyberspace |
| **Layer 2: DPDP Act** | Data protection, with "reasonable security safeguards" backed by a penalty schedule. Extraterritorial | Almost every business, because almost every business processes personal data. A US company's offshore GCC in Bengaluru is in scope the moment it touches an Indian data principal's data, wherever the servers are |
| **Layer 3: Sector overlay** | **CSCRF** for securities (SEBI), **2026 RBI Directions** for commercial banking | Only if you're in securities or commercial banking |

So a SaaS company with no sector regulator stands on Layers 1 and 2. A stockbroker stands on all three, with CSCRF on top. A commercial bank stands on all three and reports the same incident twice, once to RBI's DAKSH platform and once to CERT-In, because the obligations run in parallel rather than one satisfying the other.

## 3. The fourth axis: your client is foreign

The three Indian layers never go away. A fourth axis sits on top, orthogonal to the model: **whatever law your client's own jurisdiction imposes on the data you handle for them.** It's the default condition for most of India's IT services and software industry.

It arrives differently by sector, but the shape is always a foreign rulebook arriving through market access or a contract:

| Sector | Where the foreign axis attaches | Example |
|---|---|---|
| **Finance** | The **institution** | An Indian bank with a London or New York branch answers to the host regulator (PRA, or the Federal Reserve and OCC) directly. RBI convenes supervisory colleges with host regulators for banks with a real foreign footprint |
| **IT / ITES** | The **contract** | GDPR arrives through the Data Processing Agreement an EU client makes the vendor sign |
| **Pharma** | The **product** | A US-bound drug needs the plant to pass FDA cGMP inspection and keep electronic records compliant with 21 CFR Part 11. An EU-bound drug needs an EU GMP certificate per manufacturing site |

**The GDPR Article 33 detail:** the controller (the EU client) has up to 72 hours after becoming aware of a breach to notify its supervisory authority, "where feasible". The processor (the Indian vendor) has no fixed number, only "without undue delay". In practice the DPA tightens this, because the client needs runway left for its own 72 hours. A **24-to-48-hour vendor notification window** in the contract is common, stricter than GDPR requires of a processor, agreed voluntarily.

**A detail worth getting right:** CERT-In's clock-sync requirement names **NIC and NPL**, not ISRO. If a vendor's EU-facing infrastructure syncs to a different time source than its Indian delivery systems, cross-border log correlation during an incident can fail silently.

## 4. My four principles

1. **Treat Layer 1 as baseline code, not policy.** Write 180-day retention, NTP sync and the 6-hour window as automated checks that fail a build, not as a paragraph in a binder.
2. **Build your Layer 2 posture now, for the 2027 version of the rules.** As written in the article: DPDP's security safeguards and breach-notice duties go live 13 Nov 2026, while the DPO and data-principal-rights obligations don't bite until May 2027. Waiting for each phase means playing catch-up under a live penalty schedule.
3. **Design one incident response plan that satisfies both clocks.** A bank reporting to CERT-In and RBI's DAKSH on the same 6-hour window should produce one evidence package.
4. **Track your fourth axis with the same discipline as the first three.** A DPA breach-notification SLA, a host regulator's reporting rules, a pharma data-integrity standard: they belong in the same system that tracks Layer 1.

## 5. What I took from it

- The regulators' destination is **continuous, evidenced, machine-checkable compliance**, not an annual PDF.
- Evidence that survives an audit is different from a number that looks reassuring. Tamper-evident, not just asserted.
- Stacked obligations create operational problems of their own, such as two regulators on one clock.
- The same mechanism (foreign rulebook through contract, institution or product) shows up in every sector, even though the details differ.

## 6. To verify

- DPDP Rules phase-in dates (13 Nov 2026 and May 2027) against the notified Rules and any amendments
- The status and exact scope of the "2026 RBI Directions" and whether DAKSH reporting timelines have changed
- CERT-In's 6-hour and 180-day requirements against the current Directions text

## Related

- [03: The EU's three cybersecurity laws](./03-eu-cra-nis2-dora.md) is the designed-as-one-system contrast.
- [07: SIEM, SOAR and telemetry](./07-siem-soar-telemetry.md) covers the monitoring and evidence side behind "prove it on demand".

