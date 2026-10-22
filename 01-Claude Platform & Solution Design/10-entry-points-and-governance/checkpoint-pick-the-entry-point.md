# 10 · Entry Points & Governance — Checkpoint: Pick the Entry Point

*Pick the entry point and name the deciding tradeoff · 7 min*

For each partner scenario, choose the option that names both the right *entry point* AND the tradeoff that drives the choice. A right entry point paired with the wrong reason is not correct, the reasoning is what's being tested. A worked example is shown for scenario 1; you complete scenarios 2 through 6.

> **Note:** the course page doesn't include the answers. The answers below are inferred from the teaching content, not the official key.

> **Scenario 1, worked example**
>
> A non-engineering operations team needs a chat assistant over approved internal docs.
>
> **Correct choice: claude.ai with a Project**, because the deciding tradeoff is audience: a non-technical team needs a ready-made entry point, not a build-time interface.

### Scenario 2

A regional bank wants to deploy a loan officer assistant that retrieves customer account data from a core banking system and generates draft loan summaries. The bank runs on AWS and has an existing enterprise agreement.

- **A.** AWS *Bedrock*, because the deciding tradeoff is integration depth: the assistant needs programmatic access to core banking data and must embed into the bank's existing AWS infrastructure.
- **B.** claude.ai Enterprise, because the deciding tradeoff is integration depth: the bank needs a governed product with *SSO* and audit controls.
- **C.** Direct API, because the deciding tradeoff is integration depth: the direct API gives the most control over how requests are built.

<details><summary>Answer</summary>

**A** — the assistant must be built against core banking data, and the existing AWS agreement makes Bedrock the natural route; claude.ai can't embed, and "most control" ignores the AWS commitment. *(See 10 · Entry Points & Governance → Entry Point & Route Selection → CSP delivery routes.)*

</details>

### Scenario 3

A law firm wants to use Claude to assist attorneys reviewing privileged documents. The firm's general counsel has determined that all AI tooling touching privileged material must run behind the firm's own audit infrastructure.

- **A.** claude.ai Enterprise, because the deciding tradeoff is *governance/control*: Enterprise adds SSO, audit logging, and admin controls.
- **B.** Direct API or SDK behind the firm's own application and gateway, because the deciding tradeoff is governance/control: the firm must own the audit trail end to end, which requires routing through infrastructure the firm controls.
- **C.** AWS Bedrock, because the deciding tradeoff is regulatory residency: Bedrock provides regional data handling that satisfies privilege requirements.

<details><summary>Answer</summary>

**B** — privilege review turns on the firm owning the audit trail end to end, which means API or SDK behind the firm's own app and gateway. *(See 10 · Entry Points & Governance → Entry Point & Route Selection → Regulated-industry constraints.)*

</details>

### Scenario 4

A global logistics company wants to give warehouse operations staff a Claude assistant for shift handoff notes and equipment checklist completion. The staff are non-technical and work from shared tablets on the floor.

- **A.** Direct API with a custom-built interface, because the deciding tradeoff is integration depth: a custom build gives full control over the experience.
- **B.** claude.ai with a Project, because the deciding tradeoff is audience: non-technical staff need a ready-made interface they can use without training, and Projects provide the shared context the team needs.
- **C.** Claude Code, because the deciding tradeoff is audience: Claude Code runs on tablets and gives staff direct access to Claude's capabilities.

<details><summary>Answer</summary>

**B** — the audience is non-technical, so a ready-made entry point with shared Project context fits, matching the worked example; Claude Code is an engineering tool. *(See 10 · Entry Points & Governance → Entry Point & Route Selection → End-user entry points.)*

</details>

### Scenario 5

A healthcare network needs to deploy a clinical documentation assistant that processes patient records. The network holds a BAA with AWS and has confirmed Bedrock is covered under that agreement. A competing proposal suggests using the direct Anthropic API with a separately negotiated BAA.

- **A.** AWS Bedrock, because the deciding tradeoff is regulatory residency: Bedrock pins data to a US region which satisfies HIPAA.
- **B.** Direct Anthropic API with a separately negotiated *BAA*, because the deciding tradeoff is governance/control: the direct API gives more control over how PHI flows through the system.
- **C.** AWS Bedrock, because the deciding tradeoff is governance/control: the network's existing BAA covers this configuration, which eliminates the compliance risk before any other tradeoff applies.
- **D.** claude.ai Enterprise with a BAA, because the deciding tradeoff is governance/control: Enterprise adds HIPAA-ready configuration and audit controls.

<details><summary>Answer</summary>

**C** — HIPAA turns on a BAA covering the specific configuration, and the existing AWS BAA already covers Bedrock; A has the right route but the wrong reason (region pinning is not what HIPAA requires). *(See 10 · Entry Points & Governance → Entry Point & Route Selection → Regulated-industry constraints.)*

</details>

### Scenario 6

A financial services firm is building a high-frequency trade commentary system. The system must generate a short natural-language summary of each trade within 400 milliseconds of execution. The firm runs on GCP.

- **A.** Google *Vertex AI*, because the deciding tradeoff is latency: the 400ms requirement demands the lowest-latency path, and Vertex AI keeps the request path inside the firm's existing Google Cloud environment, minimizing network round-trip.
- **B.** Direct Anthropic API, because the deciding tradeoff is latency: the direct API has the fastest feature releases and lowest overhead.
- **C.** AWS Bedrock, because the deciding tradeoff is latency: Bedrock's managed infrastructure is optimized for low-latency inference.

<details><summary>Answer</summary>

**A** — the firm runs on GCP, so Vertex AI keeps the request path in its own cloud; B confuses feature-release speed with latency, and C is the wrong cloud. *(See 10 · Entry Points & Governance → Entry Point & Route Selection → CSP delivery routes.)*

</details>

> **Entry point** — The interface through which end users or systems access Claude in production (e.g. claude.ai, a custom app, an API integration).
>
> **Bedrock** — AWS's managed service for accessing Claude inside a customer's existing AWS environment.
>
> **SSO** — Lets users authenticate once to access multiple connected systems, instead of logging into each separately.
>
> **Governance/control** — Refers to who owns audit trails, access policy, and compliance posture for a deployment, not just whether controls exist.
>
> **BAA** — A contract required under HIPAA that governs how a vendor handles protected health information on a covered entity's behalf.
>
> **Vertex AI** — Google Cloud's managed service for accessing Claude inside a customer's existing GCP environment.
