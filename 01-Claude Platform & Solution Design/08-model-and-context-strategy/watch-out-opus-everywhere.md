# 08 · Model & Context Strategy — Watch Out: Opus Everywhere

*When defaulting to Opus everywhere produced a 7× cost overrun · 4 min*

> **Setup hook**
>
> When the demo has to land and the partner is in the room, the smart-sounding answer is "use the best available model." That answer is also the path of least resistance during build: no eval set required, no defending a choice. The bill arrives later, and by then the system has been live long enough that downgrading carries an associated change-management cost.

## 90 days after launch

**Symptom.** Monthly cost is running at seven times the original modeling figure. Latency on the user-facing path is sitting at 2.3 seconds median, which is well above the 800-millisecond target the partner agreed to at launch. Customer-satisfaction scores have not moved compared to the pre-launch baseline.

**Cause.** Every call in the stack is using Opus. There is no per-step model selection in the architecture, because there was no per-step eval that would have made per-step selection necessary. The team defaulted to "the best available model" during build and never came back to revisit the choice once traffic was live.

**Contributing factors.** Three things compounded: The original architecture document did not include a model-tier decision at any step, so the implicit default carried through to deployment. Cost was reviewed monthly rather than determined during development, so the gap between projected and actual spend did not surface until weeks after launch. Extended thinking had been enabled on a routing classifier that did not need reasoning at all, and that setting added measurable latency and cost to every request that passed through the classifier.

**What the team changed.** The team built an eval set retroactively, covering the work each pipeline step was actually doing. They routed the classifier step to Haiku and confirmed no regression on the eval. They routed mid-pipeline summarization to Sonnet and confirmed no regression there either. They kept Opus on the final response-composition step, which was the one place the eval said the higher tier earned its place. Monthly cost dropped 71%. Latency dropped to 940 milliseconds median. Customer-satisfaction scores remained unchanged.

## How a model-tier decision becomes a budget conversation

Three failure mechanisms compounded, and each one is recognizable in advance.

- **The first:** "no model-tier decision" was itself an implicit decision, and the system defaulted to the most expensive option. The absence of a deliberate choice is not neutral.
- **The second:** cost visibility lagged the build by weeks. The bill arrived after launch, after the partner had already gone live. That moved the cost conversation out of design and into change management.
- **The third:** the eval set didn't exist during the build, so when "use the best model" was proposed, the team had nothing to point to that would have grounded a different choice. The eval set is not just a release gate, it's the only thing that makes the tier decision defensible during the design conversation.

> **Why does this break?**
>
> Not choosing a model is equivalent to choosing the most expensive one. An eval set feels like extra work upfront but it's also the only thing that makes the model decision defensible.
