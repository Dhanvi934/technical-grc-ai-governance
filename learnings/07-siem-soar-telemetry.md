# 07: SIEM, SOAR and Telemetry

Supporting notes (not from an article): continuous monitoring and incident response, using **Microsoft Sentinel** and **Microsoft Defender** as the working example, and how they connect to what frameworks ask for. This topic supports my agentic AI article, where telemetry is the thing that lets you see a hijacked agent.

> **Status:** These notes come from Microsoft's documentation and the SC-900 material, plus my own reading of the frameworks. I haven't yet built this hands-on, so the framework mappings are my interpretation. A hands-on version is a planned project.

---

## 1. SIEM vs SOAR

| | SIEM | SOAR |
|---|---|---|
| **Stands for** | Security information and event management | Security orchestration, automation and response |
| **Job** | Collect logs, detect threats, support investigation | Automate the response |
| **In Sentinel** | Data connectors, analytics rules, incidents, hunting | Automation rules and playbooks |
| **Question it answers** | "What is happening, and is it bad?" | "What do we do about it, automatically?" |

Sentinel is Microsoft's cloud-native SIEM and SOAR, built on a Log Analytics workspace and queried with KQL (Kusto Query Language).

## 2. How Sentinel works, in four steps

1. **Collect:** data connectors bring in logs from Microsoft services, other clouds, firewalls, and custom sources.
2. **Detect:** analytics rules, machine learning and user and entity behavior analytics (UEBA) raise **alerts**, which are grouped into **incidents**.
3. **Investigate:** work an incident, hunt with KQL queries, view the timeline of related events.
4. **Respond:** **automation rules** triage and route incidents; **playbooks** (built on Azure Logic Apps) carry out multi-step responses such as notifying a team, opening a ticket, or disabling an account.

**Defender XDR** sits alongside this. It correlates alerts across endpoints, identities, email and cloud apps into single incidents in the Microsoft Defender portal. Sentinel then brings those together with data from everywhere else.

## 3. Why this matters for GRC: monitoring as evidence

Frameworks ask for monitoring and incident response. A SIEM is how that gets done and, just as importantly, how it gets *evidenced*.

| Framework requirement (my reading) | What the technical setup provides |
|---|---|
| **ISO/IEC 27001:2022** A.8.15 Logging and A.8.16 Monitoring activities | Log collection, retention, and detection rules |
| **ISO/IEC 27001:2022** A.5.24 to A.5.28 (incident management: planning, assessment, response, learning, evidence collection) | Incident records, playbooks, timelines, post-incident review |
| **SOC 2** CC7.2 (monitoring for anomalies) and the incident-response criteria around it | Detection rules, incident tickets, response records |
| **GDPR** Article 33 (personal data breach notification, 72 hours to the authority for controllers) | A timeline that shows when the breach was detected |
| **NIS2** Article 23 (early warning within 24 hours, incident notification within 72 hours, final report within one month) | The same timeline, with the notification clocks running |

Please verify the control numbers and deadlines against the primary texts before relying on this table. They're from my notes, not from the standards themselves, and the interpretation is mine.

**The useful insight:** one incident produces evidence for several frameworks at once. The same incident record can show monitoring (ISO A.8.16, SOC 2 CC7.2), incident handling (A.5.26) and the notification timeline (GDPR, NIS2). That is why I'd like to write this up as a single-incident walkthrough instead of a framework-by-framework list.

## 4. Applying this to AI agents

Agents change what you collect:

- **Detect:** agent tool and API call records and identity sign-in events, compared against a baseline of normal use, with incidents raised on oddities (a new tool, unusual hours, a bulk read, an unfamiliar destination).
- **Respond:** a playbook can disable the agent's identity or revoke its sessions (if the identity platform supports it), isolate the workload, pause for human approval before high-risk actions, and open a ticket with the evidence.

Limits worth stating:

- **No logs, no detection.** The agent must emit tool calls and decisions in a form the SIEM can ingest.
- **A baseline takes work.** Agents vary by design, so rules need tuning or they create constant false alarms.
- **Automatic containment can cause outages.** Deciding which agents may be shut off without a human is a governance decision, not a technical one.

## 5. Tools beyond Microsoft

The same pattern applies to other SIEMs (Splunk, Elastic, IBM QRadar and others). A note on QRadar: IBM sold its QRadar SaaS assets to Palo Alto Networks in 2024, while on-premises QRadar remains supported.

## 6. What I want to do next

- Build a small Sentinel lab: connect a data source, write an analytics rule, and trigger a playbook
- Write the single-incident walkthrough mapping one incident to ISO 27001, SOC 2, GDPR and NIS2 at the same time
- Check how widely these tools are actually used, using adoption data and not assumptions

## Sources

- Microsoft Learn, Microsoft Sentinel documentation (data connectors, analytics rules, automation rules, playbooks)
- Microsoft Learn, SC-900 study guide
- ISO/IEC 27001:2022, Annex A (check against the standard itself)
- AICPA SOC 2 Trust Services Criteria
- GDPR Article 33 and NIS2 Directive Article 23 (check the primary texts)
- TechTarget, coverage of IBM selling QRadar SaaS assets to Palo Alto Networks (May 2024)

