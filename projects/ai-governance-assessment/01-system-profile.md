# 01: System Profile

The system this assessment is about. **Halden Financial Services (fictional)** is an invented company; nothing here describes a real firm or product.

## The system in one paragraph

**Halden Assist** is a customer-facing chat agent for a regulated financial firm that serves customers in the EU. Customers sign in and ask it things like "what is my balance", "send me last month's statement", "freeze my card" or "change my address". A coordinator agent routes each request to one of three specialist agents (accounts, cards, service requests). Each specialist calls tools through its own tool server, which talks to the firm's back-end systems. The model behind the agents is a mix of a self-hosted model and a third-party LLM API.

## Scope

| In scope | Out of scope (the agent refuses and hands to a person) |
|---|---|
| Answering product and policy questions from approved sources | Credit, lending or overdraft decisions; creditworthiness assessment |
| Reading the customer's own balance, transactions, statements, card status | Insurance risk assessment or pricing |
| Reversible changes with confirmation: log a dispute, order a statement, freeze a card, update contact preferences | Investment, tax or legal advice |
| Sensitive changes with step-up and staff approval: change address or phone, replace a card, change limits | Moving money to a new payee; closing accounts |
| Handing the conversation to a person | Fraud adjudication and sanctions decisions (done by staff and other systems) |


The scope line matters for regulation as well as for safety. Excluding credit and insurance decisions is what keeps the system out of the EU AI Act's high-risk list (see [02](./02-eu-ai-act-classification.md)).

## Roles

- **The firm** builds the agent from a third-party model and its own components and puts it into service under its own name. Under the EU AI Act that likely makes it both **provider** (of the AI system) and **deployer**. That is my reading; the role analysis is in 02.
- **Model supplier** (third-party LLM API) is a supplier of a general-purpose model; the firm depends on their documentation and terms (G-26).
- **Cloud provider** hosts the runtime (physical controls are inherited, see 03a).
- **Customers** are the affected persons.
- **Staff:** service agents who take handoffs, and approvers for Tier 3 actions.

## Data

| Data | Examples | Where it appears |
|---|---|---|
| Identity and contact | Name, address, phone, email | Session, tool results, approval console |
| Financial | Balances, transactions, statements, card status and limits | Tool results, model context (tokenised where possible), logs (tokenised) |
| Conversation | What the customer typed and the agent replied | Session store, logs, model provider (redacted) |
| Operational | Agent actions, tool calls, decisions, approvals | Audit log, SIEM |


## Architecture

![Reference architecture with component IDs](./images/01-architecture-component-ids.png)

The base design is a generic multi-agent pattern (user interface, edge layer, identity provider, authorisation, PII redaction, coordinator, specialists with their own tool servers, session store, model layer, evaluation, cost tracking and observability feeding a SIEM). I added a human approval and handoff console (C16) and split the backend by action tier. [add the source of the base architecture diagram if you reuse one]

### Components

| ID | Component | What it does |
|---|---|---|
| C1 | Chat UI | Web/mobile chat window where the customer talks to the agent |
| C2 | Edge layer | WAF, DDoS protection, rate limits, API gateway |
| C3 | Identity provider | Authenticates the customer (MFA / step-up) and issues the user token |
| C4 | Application API | Session handling; passes the user token and message to the agent system |
| C5 | Authorisation service | Policy decision point: checks the action against the user's own rights, not only the agent's |
| C6 | PII redaction service | Masks / tokenises account numbers and personal data before the model and before logs |
| C7 | Coordinator agent | Understands the request, routes to a specialist, assembles the reply |
| C8 | Specialist agents (A, B, C) | A: accounts and statements. B: cards. C: service requests (address change, disputes) |
| C9 | MCP tool servers (A, B, C) | One tool server per specialist, each with its own agent identity and scope |
| C10 | Backend systems | Tier 1 read-only lookups; Tier 2 create/update records; Tier 3 sensitive changes |
| C11 | Session and memory store | Conversation history and inter-agent shared state |
| C12 | Model layer | Self-hosted model plus a third-party LLM API |
| C13 | Evaluation suite | Pre-release and regression tests: injection, scope, accuracy, safety |
| C14 | Cost and usage tracker | Token budgets, step limits, per-user and per-agent usage |
| C15 | Observability pipeline and SIEM | Prompts, agent and tool-call logs plus infrastructure metrics, feeding SIEM and SOAR |
| C16 | Human approval and handoff console | Staff queue for Tier 3 approvals and for customer handoff to a person |
| ORG | Organisation (not a runtime component) | Policies, roles, training, supplier management, internal audit, legal register |


### Two requests, end to end

**"What is my balance?" (Tier 1).** 1) The customer is signed in through C3 with MFA (G-01). 2) C2 applies rate limits; C4 passes the message and user token on. 3) C6 replaces any account numbers with tokens (G-20). 4) C7 decides this is an accounts question and hands it to Specialist A. 5) Specialist A asks Tool server A for the balance using its own short-lived token (G-03, G-04). 6) C5 checks that this user may read this account (G-02). 7) The tool server returns the balance, the reply is checked (G-18) and shown with the AI notice already on screen (G-15). 8) Every step is written to the audit log with tokenised identifiers (G-29).

**"Change my address" (Tier 3).** Steps 1 to 6 as above, then: the agent shows the exact change and asks the customer to confirm, and C3 asks for step-up authentication (G-10, G-01). The request goes to the approval queue in C16 (G-09). A staff approver sees the exact change, approves or rejects, and the decision is logged. Only then does Tool server C make the change. The customer gets an out-of-band notification. If anything fails or the model path is down, the customer is offered a person (G-19, G-28).

## Trust boundaries

- **Customer to edge (C1 to C2).** Everything the customer sends is untrusted.
- **Agent system to model layer (C7/C8 to C12).** Text leaving for the third-party model crosses a company boundary; only redacted text goes (G-20).
- **Agent to tool servers (C8 to C9).** The agent is not trusted to enforce limits; tool servers and C5 do.
- **Tool servers to backends (C9 to C10).** Narrow, tiered credentials.
- **Staff console (C16).** Privileged: only trained, authorised staff; every action logged.
- **Retrieved content and tool output.** Untrusted data, never instructions (G-13).

## Assumptions

- The firm is a regulated financial services firm in the EU; sector rules on ICT risk, incident reporting and outsourcing (DORA, NIS2) also apply but are outside this assessment.
- Customers are individuals; no business customers.
- The agent cannot send money to a new payee. This is a deliberate limit to keep the first version low-risk.
- No emotion recognition, biometric identification or profiling is used.
- Hosting is on a cloud provider with current security attestations, and the firm already runs an ISMS.
- Nothing is built. This is a design.

