# 01: System and Scope

**Halden Assist** is a customer-service chat agent at **Halden Financial Services (fictional)**, an invented regulated firm with EU customers. Customers sign in and ask it to read their data (balance, statements, card status) or do bounded tasks (freeze a card, change an address). A coordinator agent routes each request to one of three specialist agents, each with its own tool server in front of the firm's back-end systems. The model is a mix of a self-hosted model and a third-party LLM API.

![Architecture](./images/01-architecture-component-ids.png)

Components are numbered C1 to C16 and used throughout. Full component list: [reference G](./reference/G-full-system-profile.md).

## What it may and may not do

| Level | Examples | Needs |
|---|---|---|
| Tier 0: information | Product and policy questions | Nothing |
| Tier 1: read own data | Balance, transactions, statements | Signed-in customer; authorisation check against that customer |
| Tier 2: reversible change | Freeze a card, log a dispute | Customer confirms the exact action |
| Tier 3: sensitive change | Change address, replace card, change limits | Step-up authentication and a staff member's approval |
| Never: refuse and hand to a person | Credit or insurance decisions, investment or legal advice, payments to new payees | Not possible: no tool exists for them |


The last row matters beyond safety: it is what keeps the system out of the EU AI Act's high-risk category ([02](./02-eu-ai-act-classification.md)).

## One request, end to end: "change my address"

The customer signs in with MFA (G-01). The agent shows the exact change and the customer confirms (G-10) and passes step-up authentication. The authorisation service checks that this customer may change this record (G-02). The request waits in the approval queue (C16) until a staff member approves the exact change (G-09). Only then does the tool server make it, using a short-lived credential that belongs to that one agent (G-03, G-04). Every step is logged (G-29), and the customer gets an out-of-band notification.

## Assumptions

- Individual EU retail customers; the firm already runs an ISMS and hosts on a cloud provider with current attestations.
- No emotion recognition, biometrics or profiling.
- The agent cannot send money to a new payee (a deliberate limit for version 1).
- DORA, NIS2 and GDPR also apply to the firm but are outside this assessment.
- Nothing is built. This is a design.

