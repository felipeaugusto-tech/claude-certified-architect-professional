# 10 · Entry Points & Governance — Watch Out: Claude Code Misused

*When Claude Code got picked outside engineering · 4 min*

> **Setup hook**
>
> Claude Code makes a strong first impression. It can execute complex, multi-step engineering tasks in a fraction of the time a developer would spend manually, and that capability is hard to unsee. The risk is that this leads teams to reach for Claude Code by default, even when the work doesn't require it and a simpler integration or Claude alone would be sufficient. The diagram below was a real handoff from a partner asking us to validate their design.

## The proposed architecture: regional bank operations assistant

A regional bank wanted what they called an "operations assistant" for their branch staff. The work required looking up customer balances, scheduling appointments, and answering policy questions. The proposed architecture had three components:

- Claude Code running on branch laptops, with a `CLAUDE.md` file maintained per branch to encode local conventions.
- MCP servers for the customer database, the appointment system, and the policy corpus, each exposed via the standardized protocol.
- Subagents handling compliance checks on every interaction, with their own restricted tool entry point.

## The annotation across the diagram

1. **Claude Code on branch laptops (per-branch `CLAUDE.md`)**
   - *Annotation:* Engineering *entry point* for an operational workflow. Branch staff do not run terminals; the entry point is mismatched with the user.
2. **MCP servers: customer database, appointment system, policy corpus**
   - *Annotation:* MCP earns its place only when reused across clients. No other Claude clients existed in this bank.
3. **Subagents run compliance checks on every interaction**
   - *Annotation:* Compliance assigned to the weakest deterministic guarantee. High-consequence path should not run on subagents.

> **Entry point** — The interface through which end users or systems access Claude in production (e.g. claude.ai, a custom app, an API integration).

The annotation in red across the whole diagram reads: this is an engineering entry point for an operational workflow. Branch staff do not run terminals, so the entry point itself is mismatched with the user. Compliance is a high-consequence path and should not run on subagents, which provide weaker deterministic guarantees than server-side code. The customer database does not need to be exposed over MCP just because MCP was on the menu for the entry point; MCP earns its place when the same tool entry point is reused across clients, and in this case there were no other clients in the bank.

The architecture that fits the work is a custom web application calling the API directly. Compliance lives in deterministic server-side code where the guarantees are explicit rather than emergent. The user interface is authenticated via the bank's SSO and fits a banking workflow rather than a developer workflow. Tool calls are audited at the server boundary. That architecture was the right answer from screen one.

## Three failure mechanisms, each visible in the original proposal

**The first: the entry point was chosen before the user was named.** Branch staff need an interface that fits a banking workflow, not a developer tool. That constraint should have determined the entry point before any other decision was made.

**The second: MCP was carried forward from a prior project as a default integration layer.** The reusability argument that justifies MCP didn't apply here. There were no other Claude clients in the bank that would consume the same tool entry point. The protocol layer was paying integration cost for a capability the partner didn't need.

**The third: compliance, the highest-consequence path in the system, was assigned to the entry point with the weakest deterministic guarantees.** The pattern was inverted. The work that most needed code-level certainty was running on the entry point furthest from it.

> **Why does this break?**
>
> Entry point choice should follow the user and the work. Reaching for Claude Code or MCP just because the last project used them is paying for capabilities the partner does not need.
