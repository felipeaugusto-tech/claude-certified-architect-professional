# 10 · Entry Points & Governance — Entry Point & Route Selection

*Entry point, route, and governance selection · 11 min*

**You've chosen the model and the shape, and now it is time to choose how a partner consumes Claude. Choosing how a partner consumes Claude involves three separate decisions, made in sequence. Getting them in the right order prevents the most common architecture mistakes.**

- Which *entry point* fits the work?
- Which build-time interface suits the team?
- Which *delivery route* fits the partner's cloud commitments?

The three layers (entry points, build-time interfaces, and delivery routes) were taught as vocabulary in the foundations section. This screen is the selection work: choosing among them under real constraints.

> **Entry point** — The interface through which end users or systems access Claude in production (e.g. claude.ai, a custom app, an API integration).
>
> **Delivery route** — The infrastructure path Claude is accessed through (e.g. direct Anthropic API, AWS Bedrock, Google Vertex AI), distinct from the entry point users see.

## Claude entry points: how you interact with Claude

An entry point is the wrapper that decides who can talk to Claude, what Claude can touch, and how much engineering the partner must do to get there. Think of entry points as the same intelligence packaged for different jobs. Picking the wrong entry point doesn't break the work, but it adds friction the partner will feel every day.

Beyond Claude.ai and Claude Code, Anthropic ships three entry points that extend Claude into specific applications or environments. Before building partner workflows that depend on any of them, verify the current capabilities, supported configurations, and availability against Anthropic's documentation.

### End-user entry points

| Entry point | Description | Audience and use | Core tradeoff |
|---|---|---|---|
| **Claude.ai** (web, mobile, and desktop apps) | The end-user chat product. A signed-in user opens a conversation, attaches files, uses Projects for shared context, and connects services like Slack, Outlook, or Google Drive through built-in connectors. It comes in consumer tiers (Free, Pro, Max) and in Claude for Work. Claude for Work has two tiers. The Team tier adds admin controls, *SSO* and SAML, domain capture, and a contractual commitment not to train on customer content. The Enterprise tier adds SCIM provisioning, configurable retention, audit logs, a Compliance API, and a HIPAA-ready option with a signed *BAA*. | For knowledge workers using Claude as a thinking partner for research, drafting, analysis, and review. No code is written. The web app, the mobile app, and Claude Desktop all reach the same Claude.ai product. This is the entry point for applied AI users, not builders. The consumer tiers fit individuals and small teams and Claude for Work fits organizations that need governance and identity controls on the same product. | Zero build cost vs. zero integration entry point: You get the product Anthropic ships and you cannot embed it in another product or customize what gets exposed. The right tier depends on governance needs: consumer tiers for low-sensitivity work, Claude for Work where admin controls, SSO, and no-training commitments are required. |
| **Claude Code** (terminal, IDE plugin, desktop, web) | An agentic coding tool that reads files, edits code, runs commands, and executes multi-step engineering tasks under configurable permission boundaries. | For engineers doing real development work: exploring codebases, refactoring across files, debugging, building features. The product runs in a terminal, in IDE plugins (VS Code, JetBrains, others), on the desktop, and on the web at claude.ai/code. Same agent, wherever you work. | Purpose-built for engineering: Outstanding option for code but could be the wrong shape for a customer-service product or any non-engineering workflow. |
| **Claude Cowork** | A desktop agent for non-developers that works with local files and applications, automating file and task management on the user's machine under configurable permissions. | For operations, admin, and other non-engineering roles who need Claude to take actions on their computer rather than just produce text in a chat window. Available on all paid plans (Pro, Max, Team, Enterprise) via the Claude Desktop app on macOS and Windows; Linux support is in beta. | Real system actions vs. supervision overhead: Powerful for file and task automation because Cowork operates on the user's machine directly, with the consequence that permission scoping and human review matter more than they do in a chat-only entry point. |
| **Claude in Chrome** | A browsing agent that operates inside the Chrome browser, navigating pages and taking actions on behalf of the user. | Knowledge workers whose tasks are anchored in web applications rather than in files or codebases. | N/A |
| **Claude for Excel** | A spreadsheet agent that operates inside Excel, working directly with cells, formulas, and structured data. | Analysts, finance teams, and any role whose primary working tool is a spreadsheet. | N/A |

> **SSO** — Lets users authenticate once to access multiple connected systems, instead of logging into each separately.
>
> **BAA** — A contract required under HIPAA that governs how a vendor handles protected health information on a covered entity's behalf.

## Build-time interfaces: how you program against Claude

Once an entry point is picked, the next decision is which programmatic layer the partner's code talks to. The table below describes the four build-time interfaces. The API, SDKs, *MCP*, and the Agent SDK are not alternatives to each other in every case, they layer on one another.

### Build-time interfaces

| Interface | Definition | Audience and purpose | Tradeoff |
|---|---|---|---|
| **Direct API** | The direct HTTP interface to Claude. A developer authenticates, sends a request with messages, a model name, and parameters, and gets a response back. | For teams building Claude directly into their own product. The partner's team owns everything: retries, streaming, tool use, observability, and the UI. It's the most foundational build-time interface, and the layer everything else sits on. | Maximum control vs maximum responsibility: Use this when an SDK has not exposed a feature you need, or when the team prefers raw HTTP. |
| **SDKs** (Python, TypeScript, Java, Go, Ruby, C#, PHP) | SDKs offer the same capability as the API, wrapped in language-native types and helpers that cut down on boilerplate. They handle authentication, request formatting, retries, streaming, and tool-use plumbing in idiomatic code. The agent loop, if any, is still the partner's code. | For teams using the same use case as the direct API, but the team wants language-native types, less boilerplate, and built-in ergonomics for streaming and tool use. Default choice for embedding Claude inside a product. | Ergonomics vs. control: SDK abstractions move at Anthropic's release cadence. If you need a raw API feature the SDK hasn't exposed yet, you'll drop back to HTTP anyway. |
| **MCP** | An open protocol for exposing tools, prompts, and resources from one server so any MCP-aware client including Claude.ai, Claude Code, the API, or a third-party client can discover and use them.<br><br>Not a calling convention for Claude in a single product, but a sharing convention across products. | Not a calling convention for Claude in a single product, but a sharing convention across products. For teams that need the same tools reachable from multiple Claude clients. The same tool entry point is needed in two or more clients: Claude.ai, Claude Code, an internal app, a partner's product. Build the server once, connect it everywhere. | Reusability across clients vs. added architectural complexity: If only one client will ever use it, MCP adds overhead without much payback. |
| **Agent SDK** (`@anthropic-ai/claude-agent-sdk`) | Running a managed agent loop, the same loop that powers Claude Code, from the partner's own application code. The package handles iteration, tool execution, and termination. Currently TypeScript (`@anthropic-ai/claude-agent-sdk` on npm) and Python (`claude-agent-sdk` on PyPI). | For teams that need Claude to act over multiple turns inside the partner's own product, with the partner's application controlling the surrounding workflow, and the Claude Code CLI is the wrong shape. Common case: an internal agent embedded in a web app, not a terminal tool. | Managed loop vs. custom orchestration: The Agent SDK handles iteration and termination, but the partner gives up fine-grained control over the loop itself. |

> **MCP** — An open protocol that lets Claude connect to external tools and data sources through a standard interface.

## A way to keep these terms separate

**API and SDK: the same entry point, different ergonomics.** The API and the SDK are the same entry point from Claude's perspective. The SDK is an opinionated wrapper stacked over the API. It adds language-native types, handles streaming and tool-use boilerplate, and lets the partner's team work in their preferred stack (Python, TypeScript, Java, Go, Ruby, C#, or PHP) rather than against raw HTTP. The SDK is the default for most teams. The only reason to drop to raw HTTP is when a freshly shipped API feature hasn't made it into the SDK yet.

**MCP and API tool use: different layers, not alternatives.** MCP is the protocol for sharing tools across entry point, how a tool entry point is exposed and discovered across multiple clients. API tool use is how Claude calls a tool inside a single request. MCP and API tool use are not alternatives. Under the hood, an MCP server exposes tools that any MCP-aware client can call using API tool use. The choice comes down to how frequently you're going to reuse the feature: pick MCP when one tool entry point needs to be reachable from multiple Claude clients; pick raw API tool use when the tools live inside one product only.

**Claude Agent SDK: when a partner needs an agent loop they can embed in their own product.** Partners and engineers often use "SDK" to mean either the Anthropic SDK or the Agent SDK depending on context. The Anthropic SDK is a convenience wrapper over the API. It handles boilerplate but does not run an agent loop. The Agent SDK is the managed runtime that runs the loop, the same one that powers Claude Code. The model picks a tool, runs it, sees the result, and keeps going until the task is done or a stop condition fires. The partner calls it from their own application code, and the SDK handles the rest. Which layer to use comes down to what Claude needs to do: one request, one response? Use the API or SDK. Reusable tools across multiple clients? Use MCP. Claude acting across multiple turns inside the partner's own product? Use the Agent SDK. The three layers work together, not against each other.

## Claude Code: customization and governance layers

Choosing Claude Code is the start of a second decision: which customization belongs at which layer. A layer, in this context, is a discrete configuration entry point that controls one aspect of how the agent thinks or acts: each one is independent, composable, and applied at a different point in the agent's execution. The layers fall into two groups: shape what the agent knows and does (`CLAUDE.md`, skills, subagents, MCP), and govern what the agent is allowed to touch (Hooks, permission boundaries, approval flows, sandboxing, and restricted execution). Shaping layers and governing layers are distinct. Getting this split right is what makes an agent both useful and safe to run in production.

### Claude Code customization and governance layers

| Layer | What it does | When it belongs here |
|---|---|---|
| **SHAPING: What the agent knows and does** | | |
| **`CLAUDE.md`** | A markdown file loaded into context at session start. Sets standing instructions, project conventions, and background knowledge the agent should always have. | Persistent context that applies to every task in the project (coding standards, repo layout, team conventions). |
| **Skills** | Markdown-defined procedures Claude Code can invoke on demand rather than loading upfront, keeping the main context lean. | Repeatable workflows the team should not have to spell out each time. Typical examples are a commit-push-PR flow, a release-notes generator, a schema-migration procedure. |
| **Subagents** | Agent calling and creating additional agents to parse out sections of a task or isolated context-window helpers for bounded tasks like code review or codebase exploration that would otherwise clutter the main thread. | Work that should run with read-only tools, a restricted tool entry point, or a different system prompt from the main session. |
| **MCP servers** | External tools and data entry points connected to Claude Code over the standardized protocol. | When the same tool entry point needs to be reusable across clients, for example when the team's Linear MCP server should also work from Claude.ai. |
| **GOVERNING: What the agent is allowed to touch** | | |
| **Hooks** | Scripts that fire on Claude code lifecycle events (e.g. before/after a tool runs, at session start, on stop), used as deterministic gates the agent cannot skip. | Deterministic gates the agent must not skip, where the guarantee has to come from code rather than from prompting. |
| **Permission boundaries and approval flows** | Six permission modes control what Claude Code can do without prompting. Default asks before each action. `acceptEdits` approves file edits and common filesystem commands (mkdir, touch, rm, mv, cp, sed), though other Bash commands still prompt. Plan mode locks the session to read-only until the user approves a plan. Auto mode uses a classifier to approve safe actions and block risky ones; it is a research preview that works on all plans (admin-enabled on Team and Enterprise) and defaults to the Anthropic API as provider. An environment variable enables CSP providers. `dontAsk` auto-denies anything that would prompt and runs only what your allow rules cover, which makes it the mode for locked-down CI. `bypassPermissions` skips all checks and is scoped to containers or CI only. | Any environment where the cost of an unintended action is non-trivial. Permissions govern what the agent is allowed to touch. Hooks govern what must happen before or after an action. |
| **Sandboxing and restricted execution** | Containment around the workspace in which Claude Code runs, including filesystem boundaries, network egress rules, and constrained command surfaces. | Any deployment where a wrong action would have real consequences and where approval prompts alone are not a sufficient backstop. The environment itself should enforce the boundary, not just the agent's judgment. |

## CSP delivery routes: where API traffic terminates

Once the entry point and build-time interface are picked, one more decision sits underneath: where does the API traffic terminate? The same Claude model is available through four delivery routes. What differs is which cloud account the spend lands in, which identity system handles authentication, which region the traffic terminates in, and which procurement contract the partner has already signed. The decision rule is not about technical capability, the model behaves the same way on each route. The rule is about what the partner has already committed to.

If the partner already has a long-term AWS contract, *Bedrock* is usually the easiest path, the AI spend falls under the same agreement they already have, and the identity system their team uses (IAM) works as-is. The same logic applies to *Vertex AI* on GCP and Foundry on Azure: if the partner lives in that cloud, use that route. The direct Anthropic API is the right call when the partner has no strong cloud preference, wants new features the moment they ship, or prefers to keep AI spend with Anthropic directly. One tradeoff: CSP-mediated routes (Bedrock, Vertex, Foundry) tend to lag the first-party API on new features by weeks, sometimes longer for major capabilities.

### CSP delivery routes

| Delivery route | What it is and how the partner reaches it | When to pick |
|---|---|---|
| **Anthropic first-party** | The direct Anthropic API at api.anthropic.com, billed by Anthropic, and authenticated with an Anthropic API key. SDKs in Python, TypeScript, C#, Java, Go, PHP, and Ruby wrap this entry point. | Partner has no binding cloud commitment, wants newest features the day they ship, or prefers to consolidate AI spend directly with Anthropic. Default choice when no procurement constraint is pulling the other way. |
| **AWS Bedrock** | Claude served as a managed model on AWS. Called via the Messages API at `/anthropic/v1/messages` on AWS-managed infrastructure, billed on the partner's AWS account and authenticated through IAM. The previous Bedrock Runtime integration (`InvokeModel`/`Converse` via boto3 or the AWS SDK) remains available as the documented legacy path. Regional availability matters and inference profiles solve the cross-region routing problem. | Partner has a committed AWS enterprise agreement, runs the rest of their stack on AWS, and wants AI spend to draw down against that commitment. Identity, networking, and audit all inherit from the existing AWS account. |
| **GCP Vertex AI** | Claude served as a managed model in Google Cloud's Vertex AI Model Garden. Called via the Anthropic Vertex client or Google's SDK, billed on the partner's GCP project, authenticated with Google Cloud credentials. Models are enabled per project in the Model Garden console. | Partner runs on GCP, the rest of their ML stack lives in Vertex AI, and they want a single billing and audit entry point across foundation models. |
| **Microsoft Foundry (Azure)** | Claude served through Microsoft's Foundry catalog on Azure. Billed on the partner's Azure subscription, authenticated through Entra ID, deployed into the partner's Azure region. | Partner has a Microsoft enterprise agreement, runs identity through Entra ID, and the rest of their cloud footprint is on Azure. Foundry consolidates AI procurement on the same paper. Note: Foundry offers Claude models in two hosting forms: Hosted on Azure (generally available, inference runs in the partner's Azure environment; as of this writing Opus 4.8, Sonnet 5, and Haiku 4.5, verify the current list at publish time) and Hosted on Anthropic infrastructure (other models, inference routes to Anthropic-managed infrastructure). Partners with strict *data residency* or GDPR requirements should verify the hosting form and compliance posture of the specific models they deploy before committing to this route. |

**What does not change across routes.** The Claude model itself is the same regardless of route. Prompting, evaluation strategy, tool use, and context-window behavior all transfer. What changes is the wrapper: model identifiers and version strings differ across routes, regional availability differs, and CSP-side features that wrap inference (such as inference profiles on Bedrock, model deployments in Foundry, and Model Garden access controls in Vertex) all add concepts the Architect needs to know exist even if the partner's engineering team owns the implementation.

> **Bedrock** — AWS's managed service for accessing Claude inside a customer's existing AWS environment.
>
> **Vertex AI** — Google Cloud's managed service for accessing Claude inside a customer's existing GCP environment.
>
> **Data residency** — Where data is physically stored or processed, relevant to regulatory requirements that restrict data to specific regions.

## Skills as an integration mechanism

Skills are an integration mechanism, not only a packaging one. A Skill can be attached to a request through the `container.skills` parameter, published and versioned through the `/v1/skills` endpoint, and managed under version control like any other deployed asset. Skills require the Code Execution Tool to run, which means the integration pattern carries a sandboxed execution environment dependency. When the integration decision is how a reusable procedure reaches Claude across entry points, a versioned Skill is one mechanism to weigh alongside MCP and direct tool use.

> **Cost · Complexity · Risk**
>
> **Cost:** Each entry point carries a non-trivial integration cost. Don't pick more than one unless the partner's use case spans them.
>
> **Complexity:** The two most common mistakes are reaching for Claude Code on non-engineering work and treating MCP as the default integration layer regardless of whether the reusability it offers is needed. The entry point should follow the work, not precede it.
>
> **Risk:** Outgrowing the wrong entry point is expensive, not just because of the code that has to be rewritten, but because of the conventions and user habits that built up around it. Starting over on a better entry point is cheaper than that and starting on the right one is cheaper.

## Security, governance, and regulated-industry constraints

Some entry point decisions are not at your own discretion. When a partner is subject to attorney-client privilege, HIPAA, GDPR, FedRAMP, or an internal data-residency policy, those constraints rule entry points in or out before cost, ergonomics, or build effort enter the conversation. Claude.ai is the entry point this hits most often. The consumer-grade product was not designed to satisfy every enterprise data-handling requirement out of the box. The API and SDK, routed through a partner-approved gateway with logging, retention, and identity controls in the partner's own infrastructure, are the entry points that survive most regulated reviews. Name the governing constraint when you recommend an entry point and let the constraint eliminate options before preferences do.

### Regulated-industry constraints

| Constraint | What it tends to rule out | What usually survives review |
|---|---|---|
| **Attorney-client privilege** | Consumer tiers of Claude.ai for privileged document review, and anything that touches privileged material through a surface the firm cannot audit end to end. Claude for Work adds admin controls and audit logging, but a firm still has to confirm the configuration meets its own privilege-handling bar before privileged material flows through it. | API or SDK behind the firm's own application, authenticated via SSO, routed through a firm-approved LLM gateway that logs every request. The firm owns the audit trail end to end, which is what privilege review turns on. |
| **HIPAA (PHI handling)** | Any entry point where a Business Associate Agreement is not in place for the specific configuration the partner is using. A BAA that exists for one configuration does not extend to another, so an uncovered route is ruled out even when the partner holds a BAA elsewhere. | API or SDK on a BAA-covered configuration via the delivery route the partner is already using. BAA existence is not sufficient because feature eligibility matters. Beta features are generally excluded from BAA coverage unless explicitly listed as eligible. |
| **GDPR & data residency** | Delivery routes where the region of model execution cannot be pinned, and routes where data leaves the approved geographic boundary at any step. | A CSP-mediated delivery route (Bedrock or Vertex) with the region pinned to a covered jurisdiction and DPA terms inherited from the existing cloud contract. Foundry is the route to check here, because its residency guarantees are not something this course can confirm. Verify with the Foundry route's current documentation before relying on it for residency. |
| **FedRAMP / government** | Any path that is not on an authorized cloud environment at the required impact level. | Claude for Government (C4G) for FedRAMP High civilian workloads. Bedrock GovCloud for FedRAMP High and DoD IL4/5. Vertex Assured Workloads for FedRAMP High and IL2. Note: authorized government environments run on a model lag, so the newest Claude models reach GovCloud and Assured Workloads after the commercial release. Confirm which model the route offers before committing to it. |
| **Internal data-residency policy** | Routes outside the partner's approved cloud vendor list, regardless of the underlying technical capability. | The delivery route on the partner's approved CSP. This is procurement, not engineering: the right route is whichever one their CIO has already cleared. |

Note: Always verify current authorization scope for each of the constraints with Anthropic before committing.

> **Forward pointer**
>
> Module 3 (Responsible AI, Safety and Risk for Architects) goes deep on guardrail design, data handling, and the full regulated-industry framework. The role of this section is to surface the constraint at the point in the design conversation where it eliminates options, which is right here at the entry point and delivery-route decision.
