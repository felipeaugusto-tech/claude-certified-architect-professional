# 05 · Pattern Selection — Watch Out: Fan-out Drop

*When fan-out hid a dropped unit · 4 min*

## The trace

A compliance team built a *multi-agent system* to check a 50-section vendor contract against an internal policy checklist. The orchestrator *fanned the work out* to one subagent per section, each returning a pass/flag verdict, and synthesized a clean summary: "48 sections reviewed, 3 flagged." The summary read as complete and was circulated to the legal lead.

Two sections had never been reviewed. One subagent had timed out and returned nothing; another had failed to parse a scanned page and returned an empty result. The orchestrator, given no coverage check, counted only the results it received and reported "48 reviewed", but there were 50 sections, and nobody had told the synthesis step to reconcile the count.

> **Multi-agent system** — An architecture where multiple Claude instances (agents) each handle a subtask, coordinated by an orchestrating layer.
>
> **Fanned the work out** — Fan-out: splitting one task across multiple parallel agent calls; a common source of silently dropped units of work if not tracked.

## What broke and why

- **No coverage check at synthesis.** The orchestrator synthesized over the results it happened to receive, with no rule that the number of results must equal the number of units dispatched.
- **A recoverable failure was never recovered.** A timed-out subagent is the recoverable case, but only if something retries it or flags the gap. Here the failure was silent because nothing was watching the boundary.
- **Confident synthesis over incomplete work.** The output's fluency masked the gap. A multi-agent system fails most dangerously when the summary looks complete and is not.

> **Why this broke**
>
> Completeness was assumed, not verified. The orchestrator dispatched 50 units and reported on the results it received. Two units never came back, and nothing in the design noticed the difference. Three gaps lined up to let that through.
>
> 1. **The count was never reconciled.** The synthesis step added up the verdicts it received and stopped there. No rule said the number of results had to match the number of units dispatched, so 48 returned results became "48 reviewed" instead of "two are missing."
> 2. **A recoverable failure had nothing watching it.** A timed-out subagent and an empty parse result are both the recoverable case, but only when something retries the unit or flags the gap. No component owned the subagent boundary, so both failures passed silently.
> 3. **The output read as complete.** The summary was fluent and well-formed, which is exactly what made the gap invisible. A multi-agent system fails most dangerously when a confident summary is built over work that was never finished. The fix is a coverage check at synthesis: results returned must equal units dispatched, or the run flags the difference before anyone reads the summary.
