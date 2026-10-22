# 09 · Prompting as Architecture — Prompt Reuse & Skills

*Caching mechanics, modular prompts, and Skills · 8 min*

**Reusable prompts raise a question the one-off prompt never does: how do you reuse them efficiently, and how do you package them so a team can share and govern them? This screen covers the mechanics of *caching*, the difference between a modular prompt library and a *Skill*, and the decision of which to use.**

> **Caching** — Reuses a previously processed portion of a prompt to reduce cost and latency on repeated or templated requests.
>
> **Skill** — Reusable instruction packages that extend what Claude can do without rebuilding the prompt each time.

> **A cache that never hits**
>
> A team put their reusable analysis prompt into production and saw none of the cost savings caching was supposed to bring. The cause was ordering: they had placed the per-request content (the document being analyzed) at the top of the prompt, ahead of the large fixed instruction block. Because the cache matches on a stable prefix, putting dynamic content first meant the prefix changed on every request and the cache never hit. The fix was to reorder with fixed content first and dynamic content last.

## Caching mechanics an architect designs around

### Cache breakpoints

Caching works on a stable prefix. Mark the boundary between the fixed part of the prompt (cacheable) and the variable part (not), and keep the fixed part genuinely fixed.

### Content ordering

Static before dynamic, always. The large, unchanging instruction block goes first; the per-request content goes after the breakpoint.

### TTL selection

Match the cache lifetime to how often the fixed content actually changes and how frequently the prompt is called. A prompt called constantly benefits from a longer-lived cache; one called rarely may never amortize the write.

### When the write overhead is not worth it

Writing to the cache has its own cost. If a prompt is called infrequently or its fixed portion is small, caching can cost more than it saves. Caching is a design decision, not a default to switch on everywhere.

## Modular prompt libraries vs Skills as versioned reusable units

There are two ways to make a prompt reusable across a team. A modular prompt library is a shared collection of prompt fragments and templates that engineers assemble in their own code. A Skill is a more formal, versioned, self-contained unit: a `SKILL.md` that packages the instructions, optional executable scripts, and version management, so the whole procedure travels as one governed artifact. A Skill is the reuse primitive named in the foundations screen (packaging a repeatable procedure) applied to prompting.

## The reuse decision: library or Skill

| Consideration | Lean toward a prompt library | Lean toward a Skill |
|---|---|---|
| **Repeatability** | An assembled, often-tweaked prompt per use. | A stable procedure run the same way every time. |
| **Distribution** | Shared within one codebase or team. | Distributed across teams or products that need the same procedure. |
| **Governance** | Lightweight; engineers own the fragments. | Needs versioning, approval, and rollback, Skills carry that. |

> **Cost · Complexity · Risk**
>
> **Cost:** Caching can cut cost substantially when it hits and add cost when it does not. The economics depend on call frequency and prefix size, so model them before committing.
>
> **Complexity:** A Skill concentrates a procedure into one versioned unit that can be reviewed and rolled back; a sprawl of copy-pasted prompts cannot.
>
> **Risk:** An ungoverned prompt that is copied across teams drifts into many slightly different versions, each with its own quietly different behavior. Versioned reuse is the control.
