# 02: Vendor Due Diligence Questionnaire

27 questions in six areas, written for a managed vector database that would hold documents the agent retrieves. A **gate** is a must-have for a given data class (defined in [01](./01-scope-and-method.md)). An answer to a gate only counts as proven if we have seen the document behind it.

Areas: data handling (Q-01 to Q-06), isolation and security (Q-07 to Q-14), assurance (Q-15 to Q-17), fourth parties (Q-18 to Q-20), incidents and resilience (Q-21 to Q-24), legal and change (Q-25 to Q-27).

**Evidence levels.** *Document seen* means we read the contract, report or certificate. *Claimed* means the vendor said so, and nothing backs it up yet. *None* means no answer.

| ID | Topic | Question | Why it matters here | Gate for |
|---|---|---|---|---|
| Q-01 | Data location | Where are vectors, text and metadata stored, and can we choose an EU region? | Data location decides which transfer rules apply and who can compel access. | - |
| Q-02 | What is stored | Which of vectors, source text and metadata does the service keep, and can we store vectors and IDs only? | Source text next to vectors is the biggest exposure. Vectors and IDs only is far safer. | - |
| Q-03 | Retention after deletion | How long does data survive in backups after we delete it? | A deleted record that lives on in backups still counts as held data. | - |
| Q-04 | Deletion on request | How fast is deletion, and is there confirmation we can show a regulator or a customer? | Customers can ask for erasure. Halden must be able to show it happened. | class 2 |
| Q-05 | No training on our data | Is our data excluded from model training and product improvement, in writing? | Customer documents must not become the vendor's training or product data. | class 2 |
| Q-06 | Vendor staff access | When can vendor staff see our data, and are we told and is it logged? | Staff access is a route to customer data that no technical control of ours covers. | - |
| Q-07 | Tenancy model | Is our data on a dedicated cluster or shared infrastructure, and how is it separated? | A dedicated cluster removes the risk of another tenant reaching our data. | - |
| Q-08 | Isolation evidence | What independent evidence shows tenants cannot reach each other's data? | Isolation claims need independent evidence, not marketing text. | - |
| Q-09 | Encryption at rest | Is data encrypted at rest, and can we hold the key (customer-managed key in our Azure Key Vault)? | Our own key means we can cut access and the vendor cannot read data without it. | class 2 |
| Q-10 | Encryption in transit | Which protocol versions protect data in transit? | Baseline protection on the wire. | class 0 |
| Q-11 | Private connectivity | Can we reach the service over Azure Private Link and switch off the public endpoint? | Keeps traffic off the public internet and lets us close the public door. | class 2 |
| Q-12 | Authentication | Can workloads sign in with Microsoft Entra ID federation, or only static API keys? | Static API keys leak. Federated sign-in ties access to a managed identity (see Project 1, G-03 to G-05). | - |
| Q-13 | Key and secret rotation | How are keys and secrets rotated? | Stale keys are a common route to a breach. | - |
| Q-14 | Penetration testing | Is there an annual independent test, and can we see a summary? | Shows whether someone independent has tried to break in. | - |
| Q-15 | SOC 2 scope and exceptions | Does the SOC 2 Type II report cover the exact service we would use, and what exceptions are noted? | A clean report is only useful if it covers the exact service we would use. | class 2 |
| Q-16 | Other certifications | Which other certifications are held, and can we see the certificates? | Supporting evidence. A certificate is not a guarantee of controls in our scope. | - |
| Q-17 | Audit rights | Can we audit the vendor, or only read their reports? | Regulated firms may need to audit critical suppliers. Reading a report is not auditing. | - |
| Q-18 | Sub-processors | Is there a current list of sub-processors with locations? | We cannot assess fourth parties we do not know about. | class 2 |
| Q-19 | Third-party processing of our text | Does any third party (for example an embedding service) receive our text? | A hidden fourth party that sees our text breaks the architecture we assessed. | class 2 |
| Q-20 | Sub-processor change notice | Are we told in advance of sub-processor changes, and can we object? | Changes to who holds our data should not arrive as a surprise. | - |
| Q-21 | Incident notification | How quickly is the vendor committed in contract to tell us about an incident? | GDPR Art 33(2) says a processor must tell the controller without undue delay. A long contractual delay leaves customers exposed while Halden cannot yet contain or report. | class 2 |
| Q-22 | Audit logs to us | Can we export access and admin logs to our own SIEM? | Without logs we cannot detect misuse or investigate (Project 1, G-29 and G-31). | - |
| Q-23 | Availability and recovery | What availability and recovery commitments (SLA, RPO) are in the contract? | If retrieval is down, the agent falls back to human handoff (G-28). Commitments set how often that happens. | class 0 |
| Q-24 | Exit and portability | Can we export everything in an open format, and is exit help committed? | A supplier we cannot leave is a risk in itself. Regulators expect an exit plan. | class 0 |
| Q-25 | Data processing agreement | Is there a data processing agreement with the GDPR Article 28 terms? | GDPR Article 28 requires a contract with specific terms for processors. | class 2 |
| Q-26 | International transfers | If data leaves the EU, what lawful transfer mechanism applies? | Personal data leaving the EU needs a lawful transfer mechanism. | class 2 |
| Q-27 | Service change notice | How much notice do we get of breaking or security-relevant changes? | A silent change can break a guardrail we tested. | - |

This questionnaire is my design for a learning project. A real firm would also use its own supplier policy, the regulator's templates and legal review.
