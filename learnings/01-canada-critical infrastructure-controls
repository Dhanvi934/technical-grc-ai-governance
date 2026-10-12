# 01: Canada's Critical Infrastructure Controls

**From my article:** *Securing Canada's Critical Infrastructure: Why Network Segmentation, Backup Resilience, and Secure Remote Access Are Governance Imperatives* (Substack, 10 Sep 2026). [PDF](../articles/01-canada-critical-infrastructure-segmentation-backup-remote-access.pdf)

**Core idea:** four well-understood technical controls (segmentation, perimeter defence, tested backups, VPN with MFA) are not IT tickets. They are governance obligations that need an owner, evidence and continuous review.

---

## 1. The case that motivates it

In December 2022 the Hospital for Sick Children (SickKids) in Toronto was hit by ransomware. Internal systems, phone lines and lab and imaging results were disrupted for weeks. The attack was carried out by an affiliate of LockBit, which later apologised publicly and provided a free decryptor.

Public reporting doesn't say which of the hospital's own controls held and which failed, so I don't claim to know the root cause. What the case shows is the *kind of outcome* these controls exist to prevent: one intrusion cascading into weeks of operational disruption.

## 2. The standard: CAN/DGSI 104

CAN/DGSI 104 (formerly CAN/CIOSC 104) was developed by the Digital Governance Council (formerly the CIO Strategy Council) and is administered through the CyberSecure Canada certification program. It has three tiers:

1. **Organizational controls:** leadership accountability, cybersecurity risk assessment, staff training
2. **Baseline controls:** patch management, endpoint protection, encrypted backup, perimeter defence, access control
3. **Baseline controls by operating environment:** cloud and outsourced IT, mobile devices, websites, point-of-sale and financial systems, log management

The four controls in my article:

| Control | What it requires |
|---|---|
| **5.7.3.6** Network segmentation for public and corporate networks | Separate publicly available networks (guest Wi-Fi, customer-facing) from the internal network using VLANs, firewall rules and ACLs |
| **5.7.3.1** Firewalls | Firewalls between network perimeters to control what passes |
| **5.6.2.7** Security and integrity of critical backups | Backups tested regularly to confirm they can be restored, remain accessible in an emergency, and haven't been tampered with |
| **5.7.3.4** VPN connectivity with MFA | Remote access encrypted in transit, through a configured VPN, gated by MFA, never a password alone |

## 3. How it maps to ISO/IEC 27002:2022 and OSFI B-13

| Theme | ISO/IEC 27002:2022 | OSFI Guideline B-13 (in force for federally regulated financial institutions since 31 Jul 2022) |
|---|---|---|
| **Segmentation and perimeter** | 8.20 Network security; 8.22 Segregation of networks | Authorized logical network zones with controls limiting or blocking traffic between them; minimized attack surface through layered controls; intrusion detection and behaviour-based monitoring to catch lateral movement |
| **Backup** | 8.13 Information backup | An Enterprise Disaster Recovery Program with recovery time objectives, validated against "severe but plausible" scenarios, ransomware explicitly included |
| **Remote access** | (access control themes) | Risk-based identity and access controls including MFA for external-facing channels and privileged accounts; least privilege; continuous logging of account activity |

## 4. The governance argument

None of these controls is new or exotic. What CAN/DGSI 104, ISO 27002 and B-13 share is the insistence that they can't stay one-time IT projects. They have to be **attestable, continuously monitored obligations with a named owner**.

Two examples I find useful:

- A firewall rule set deployed in 2023 and never reviewed is not "segmentation" in a governance sense, even if the network diagram still shows separate VLANs.
- A backup job that runs nightly but has never been restored in a test is not "resilience", even if the storage bill proves it exists.

A backup counts in governance terms only if it **exists on a schedule suited to the data, is encrypted, and is tested**.

Where it usually fails is in the gaps: a legacy remote-access path from before the VPN rollout, a contractor account "temporarily" exempted, MFA enforced on email but not on the VPN concentrator.

## 5. Why it matters beyond compliance

- CAN/DGSI 104 certification is increasingly a **procurement requirement** in Canadian public-sector and enterprise contracts.
- Cyber insurers are tightening underwriting questions around these same control families.
- For hospitals, a ransomware event touching health records is both an operational outage and a **privacy breach with notification obligations** under PHIPA and PIPEDA.

## 6. What I took from it

- A control is only governance if someone owns it, someone checks it, and there's evidence it still works.
- "Is it deployed?" and "does it still work?" are different questions, and audits often only ask the first.
- The same four control families recur across a standard, a regulator's guideline and ISO, which is a good sign they're the real baseline.

## 7. To verify

- The CAN/DGSI 104 control numbers against the standard itself
- The exact B-13 wording on each requirement (I paraphrased)
- Whether CAN/DGSI 104 has been revised since I wrote this

## Related

- [02: Canada's AI governance patchwork](./02-canada-ai-governance-patchwork.md) covers B-13 again as the floor for AI systems in banks.
- [07: SIEM, SOAR and telemetry](./07-siem-soar-telemetry.md) covers the monitoring side that makes these controls continuous.

