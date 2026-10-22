# 05 · Integration — Critique the integration diagram

*Critique the integration diagram · 5 min*

The diagram below shows a Claude deployment for a customer-service agent at a multi-tenant SaaS company. Review the labeled components and connections, and select every one that represents an integration problem.

*Select all problems. Leave sound components unselected.*

### Components

*(Diagram flow, top to bottom: 1 → {2, 3, 4} → 5 → 6 → 7.)*

1. Claude Code used as the backend for the customer-facing chat product
2. Shared API key used for all tenants
3. Capability check based on "I am a premium customer" in the user message
4. Account number and email passed in the user message and appearing in request logs
5. Server-side authentication layer in front of the API
6. Tenant data isolated per tenant at the storage layer
7. Claude response passed to the downstream CRM with no logging at the integration layer

<details><summary>Answer</summary>

**Problems: 1, 2, 3, 4, 7** · **Sound: 5, 6**

- **1** — Claude Code is built for developer workflows, not as a backend for multi-tenant, customer-facing products.
- **2** — a shared key can't attribute a rate-limit breach to a tenant; use separate keys per tenant.
- **3** — a role claimed in the user message is unverified; identity must be injected server-side.
- **4** — PII passed in the user message lands in plaintext request logs; pass only what the task needs.
- **7** — an unlogged data path can't be reconstructed after an incident.

*(See 05 · Integration → Enterprise integration patterns.)*

</details>
