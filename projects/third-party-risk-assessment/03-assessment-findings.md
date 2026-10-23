# 03: Assessment Findings

Findings for two **fictional** vendors. The facts are invented to illustrate the method; no real vendor was assessed. Raw answers are in [`data/vendor-evidence.json`](./data/vendor-evidence.json) and the gate check is `python scripts/check_evidence.py`.

## Gate results

| Data class | Vendor A: Skein Vector Cloud (shared SaaS) | Vendor B: Ferrule Vector Dedicated (dedicated cluster) |
|---|---|---|
| 0: Public content | Conditional: Exit and portability only partly met, encryption in transit needs evidence | Conditional: encryption in transit needs evidence |
| 2: Customer personal data | **Not approved**: 4 gates fail, 2 partly met | **Conditional**: 2 need evidence, 2 only partly met, 6 pass |

## What decided it

**Vendor A fails for customer data.** Four gates fail outright: no customer-managed keys (Q-09), no private connectivity (Q-11), a third-party service receives our text by default (Q-19), and incident notification takes 72 hours (Q-21). It also has a SOC 2 exception on the very thing we care about, separation between tenants (Q-08, Q-15), with the fix promised but not shown. Any one of the four failures would stop a go-live for personal data.

**Vendor B is close, but not there.** Its design fits the use case (dedicated cluster, no third-party processing, customer-managed keys, private link, EU only). Three things stop a clean approval:

1. *The SOC 2 report does not cover the service we would use* (Q-15). The report is clean, but dedicated clusters launched after the audit period. A clean report about a different product tells us little.
2. *Two gates rest on the vendor's word* (Q-09, Q-11). Customer-managed keys and Private Link sound right but have not been tested by us.
3. *Incident notification is 48 hours* (Q-21), slower than Halden would want, given its own reporting duties and the need to contain quickly. The vendor says this is negotiable.

## What the evidence levels show

- **Skein Vector Cloud (fictional):** 12 of 27 answers backed by a document we saw, 15 claimed only. Results: 11 meet, 9 partly, 7 fail.
- **Ferrule Vector Dedicated (fictional):** 18 of 27 answers backed by a document we saw, 9 claimed only. Results: 25 meet, 2 partly, 0 fail.

A document-backed 'partly' is more useful than a claimed 'meets'. This is why the check treats claims as *needs evidence*, not as passes.

## Full answers

### Skein Vector Cloud (fictional)

| ID | Topic | Result | Detail |
|---|---|---|---|
| Q-01 | Data location | Meets (claimed) | EU and US regions offered; EU selectable. |
| Q-02 | What is stored | Meets (claimed) | Stores vectors, full source text and metadata. No vectors-only option. |
| Q-03 | Retention after deletion | Partly | Backups kept 30 days after deletion (DPA schedule). |
| Q-04 | Deletion on request | Partly (claimed) | API delete; completion within 30 days including backups; no deletion certificate. |
| Q-05 | No training on our data | Meets | DPA states customer data is not used for training. |
| Q-06 | Vendor staff access | Partly (claimed) | Support staff can access data with a ticket; customer not notified. |
| Q-07 | Tenancy model | Meets | Shared multi-tenant service; logical separation by namespace. |
| Q-08 | Isolation evidence | Partly | SOC 2 Type II report notes an exception in testing of logical separation; management response promises a fix next quarter. |
| Q-09 | Encryption at rest | Fails | Vendor-managed keys only; no customer-managed key option. |
| Q-10 | Encryption in transit | Meets (claimed) | TLS 1.2 or higher. |
| Q-11 | Private connectivity | Fails | Public endpoint only. Private endpoint is described as on the roadmap. |
| Q-12 | Authentication | Fails | Static API keys only; no Entra federation. |
| Q-13 | Key and secret rotation | Partly (claimed) | Manual rotation by customer. |
| Q-14 | Penetration testing | Partly (claimed) | Annual third-party test, but the summary is not shared. |
| Q-15 | SOC 2 scope and exceptions | Partly | Report covers the service, with the exception above. |
| Q-16 | Other certifications | Meets (claimed) | ISO 27001 certified (certificate not yet seen). |
| Q-17 | Audit rights | Fails | No customer audits; reports only. |
| Q-18 | Sub-processors | Meets | Published list: cloud host (EU and US) and an embedding service provider (US). |
| Q-19 | Third-party processing of our text | Fails | Optional 'auto-embed' sends text to a third-party service in the US; on by default in the quick-start. |
| Q-20 | Sub-processor change notice | Partly | 14 days notice by email; no right to object. |
| Q-21 | Incident notification | Fails | 72 hours (standard terms). |
| Q-22 | Audit logs to us | Fails | Audit logs not exportable; available on support request. |
| Q-23 | Availability and recovery | Meets | 99.9% availability; RPO 24 hours. |
| Q-24 | Exit and portability | Partly (claimed) | Bulk export through the API in an open format; no exit assistance committed. |
| Q-25 | Data processing agreement | Meets | Standard DPA with Article 28 terms. |
| Q-26 | International transfers | Meets | Standard contractual clauses cover US sub-processors; EU region available. |
| Q-27 | Service change notice | Meets (claimed) | 30 days notice for breaking changes. |

### Ferrule Vector Dedicated (fictional)

| ID | Topic | Result | Detail |
|---|---|---|---|
| Q-01 | Data location | Meets | Cluster is created in the region we choose; EU available. |
| Q-02 | What is stored | Meets (claimed) | Stores vectors and metadata; source text is optional, so vectors and IDs only is possible. |
| Q-03 | Retention after deletion | Meets | Backups kept 7 days after deletion. |
| Q-04 | Deletion on request | Meets | Deletion within 7 days; certificate on request. |
| Q-05 | No training on our data | Meets | Contract excludes training on customer data. |
| Q-06 | Vendor staff access | Meets (claimed) | Break-glass access only, approved by the customer and logged. |
| Q-07 | Tenancy model | Meets | Dedicated single-tenant cluster per customer. |
| Q-08 | Isolation evidence | Meets | SOC 2 Type II report with no exceptions noted. |
| Q-09 | Encryption at rest | Meets (claimed) | AES-256 at rest; customer-managed keys supported through the customer's Azure Key Vault. |
| Q-10 | Encryption in transit | Meets (claimed) | TLS 1.3. |
| Q-11 | Private connectivity | Meets (claimed) | Azure Private Link supported and the public endpoint can be disabled. Needs a pilot to prove it. |
| Q-12 | Authentication | Meets (claimed) | Entra ID federation supported; no static keys required. |
| Q-13 | Key and secret rotation | Meets (claimed) | Automated rotation with customer-managed keys. |
| Q-14 | Penetration testing | Meets | Annual third-party test; summary shared under NDA. |
| Q-15 | SOC 2 scope and exceptions | Partly | Report covers the managed cloud product. Dedicated clusters launched after the audit period, so they are outside the current report; the next report is due in about 6 months. |
| Q-16 | Other certifications | Meets | ISO 27001 certificate seen. |
| Q-17 | Audit rights | Meets | Draft contract allows one customer audit a year with notice. |
| Q-18 | Sub-processors | Meets | Published list: cloud host only, in the EU. |
| Q-19 | Third-party processing of our text | Meets | None. The customer supplies the vectors. |
| Q-20 | Sub-processor change notice | Meets | 30 days notice with a right to object. |
| Q-21 | Incident notification | Partly | 48 hours in standard terms; vendor says it will negotiate. |
| Q-22 | Audit logs to us | Meets (claimed) | Audit logs exportable to a customer SIEM. |
| Q-23 | Availability and recovery | Meets | 99.95% availability; RPO 1 hour. |
| Q-24 | Exit and portability | Meets | Open-format export; 60 days of exit assistance in the draft contract. |
| Q-25 | Data processing agreement | Meets | Standard DPA with Article 28 terms. |
| Q-26 | International transfers | Meets | No transfers outside the EU. |
| Q-27 | Service change notice | Meets (claimed) | 60 days notice for breaking changes. |
