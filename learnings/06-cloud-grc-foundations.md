# 06: Cloud GRC Foundations (SC-900 notes)

Supporting notes (not from an article): the security, compliance and identity concepts that everything else in cloud GRC builds on. They come from studying for Microsoft's **SC-900 (Security, Compliance, and Identity Fundamentals)**, using Microsoft's official study guide on Microsoft Learn (updated September 2026; the skills measured change on 21 October 2026, so check the current version).

The notes use Microsoft's stack because that's what I'm studying, but the concepts are not Microsoft-specific. For the full set of notes, see the study guide linked at the end.

---

## 1. The core concepts

### Shared responsibility
The cloud provider and the customer secure different layers, and the split depends on the service type (IaaS, PaaS, SaaS).

- **Always the customer:** your data, your endpoints and devices, your accounts and identities.
- **Always the provider:** the physical datacenter, network and hosts.
- **Varies:** the operating system, network controls, applications.

Even in SaaS, the customer still owns data and identities. This matters for GRC because "the cloud provider is certified" never means "we are compliant". The provider's certification covers their layer only.

### Defense in depth
Multiple layers of protection (physical, identity, perimeter, network, compute, application, data), so that one failure doesn't expose everything.

### Zero Trust
"Never trust, always verify." Three principles:

1. **Verify explicitly:** use all available signals (user, device, location, risk).
2. **Use least-privilege access:** just-in-time and just-enough access.
3. **Assume breach:** limit blast radius, segment, encrypt, and monitor.

### Identity is the new perimeter
With cloud, remote work and personal devices there is no single network edge to defend, so the control that matters most is knowing who or what is asking for access. This idea is the foundation of my [agentic AI article](./05-agentic-ai-and-nhis.md), where "who" includes software.

### Authentication vs authorization
Authentication is proving who you are. Authorization is what you're allowed to do once identified. Authentication always comes first.

### Encryption vs hashing
Encryption is reversible with a key (symmetric for bulk data, asymmetric for key exchange and signatures). Hashing is one-way and is used for integrity checks and password storage.

### GRC in one line each
- **Governance:** the rules, roles and accountability.
- **Risk:** identifying, assessing and treating threats to the business.
- **Compliance:** meeting the requirements of laws, regulations and standards.

---

## 2. Identity: Microsoft Entra

Identity is the largest domain on the exam after security solutions (25-30%), and it's the one most relevant to my AI governance interest.

- **Identity types:** users, **workload identities** (service principals and managed identities), devices and external identities. Workload identities are the software equivalent of an employee account, and they are exactly where AI agents land.
- **Managed identities** let an Azure resource authenticate without anyone storing a credential in code. This is the pattern I'd point to when talking about avoiding hardcoded keys.
- **Conditional Access** is an if-then policy engine: if these signals (user, device, location, risk), then these decisions (allow, block, require MFA).
- **Privileged Identity Management (PIM)** gives just-in-time, time-bound access to privileged roles, which is least privilege in practice.
- **Identity governance** covers the joiner/mover/leaver lifecycle, access packages and access reviews.
- **Entra ID Protection** detects risky sign-ins and risky users and feeds risk-based policies.

**Trap to remember:** Entra roles (administering the directory) and Azure RBAC (access to Azure resources) are separate systems.

---

## 3. Security solutions

| Area | The one-line idea |
|---|---|
| **Azure DDoS Protection, Firewall, WAF, NSGs, Bastion, Key Vault** | Network and infrastructure controls: absorb floods, filter traffic, protect web apps, restrict access, secure admin access, keep secrets out of code |
| **Defender for Cloud** | Cloud security posture (am I configured securely?) plus workload protection (am I being attacked right now?) |
| **Microsoft Sentinel** | SIEM and SOAR: collect, detect, investigate, respond (see [07](./07-siem-soar-telemetry.md)) |
| **Defender XDR** | Correlates alerts across email, endpoints, identities and cloud apps into single incidents |

---

## 4. Compliance solutions

- **Service Trust Portal:** where Microsoft publishes its audit reports and certifications. It shows how *Microsoft* meets standards, which is useful evidence when an auditor asks.
- **Compliance Manager:** assessments, improvement actions and a compliance score. The score is a **progress measure, not proof of legal compliance**.
- **Data protection tools:** classification, **sensitivity labels** (how data is protected), **DLP** (stopping sensitive data leaving), and **retention** (how long data is kept).
- **Insider risk, eDiscovery and Audit:** detecting risky internal behavior, finding content for investigations, and recording who did what.

---

## 5. How this connects to the frameworks

| If a framework asks for... | The kind of technical control that supports it |
|---|---|
| Access control and privileged access (ISO 27001 A.8.2) | Entra roles, PIM, Conditional Access |
| Authentication information management (ISO 27001 A.5.17) | MFA, passwordless methods, managed identities, Key Vault |
| Monitoring (ISO 27001 A.8.16, SOC 2 CC7.2) | Sentinel, Defender XDR |
| Evidence of controls | Audit logs, Compliance Manager, the Service Trust Portal |

These mappings are my interpretation. The frameworks don't name Microsoft products, and an auditor decides whether a control satisfies a requirement.

---

## 6. Things I want to double-check or go deeper on

- The updated skills outline taking effect on 21 October 2026
- Licensing differences (what needs P1 vs P2) before I rely on a feature in a project
- Hands-on practice in a Microsoft 365 / Azure trial tenant

## Source

- [Study guide for Exam SC-900, Microsoft Learn](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/sc-900)

