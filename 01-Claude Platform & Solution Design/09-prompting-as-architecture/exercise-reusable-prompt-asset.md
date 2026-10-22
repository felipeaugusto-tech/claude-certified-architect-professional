# 09 · Prompting as Architecture — Exercise: Reusable Prompt Asset

*Author the reusable prompt asset · 6 min*

> **The brief**
>
> A partner's support organization runs the same operation hundreds of times a day: given a customer's support ticket and the relevant section of the product manual, produce a drafted reply that follows the partner's tone guidelines, cites the manual section it relied on, and never promises a refund or a timeline the agent has not approved.

Draft the reusable prompt asset. Make the three design decisions below and write a brief rationale for each:

1. Where does the stable prefix end and the per-request content begin? Name the breakpoint placement and explain why.
2. How do you enforce the never-promise-a-refund guardrail structurally, not just as a stated instruction? Describe the output contract constraint.
3. Should this ship as a template or a Skill? Name the packaging choice and state the reason.

*Write your answer, then reveal the model answer below. Feel free to ask Claude to compare what you've written with the provided answer.*

## Your prompt asset design

*Name the cache-breakpoint placement, the output-contract guardrail, and the packaging choice, with a brief rationale for each…*

<details><summary>Model answer</summary>

**Cache-breakpoint placement:** Put the role, tone rules, output contract, and never-promise guardrail first as the stable prefix, then the ticket and manual section as the only per-request content. Dynamic content before the breakpoint means the prefix changes on every call and the cache never hits. At hundreds of calls a day, the stable prefix is where the cost savings live.

**Enforcing the guardrail:** Build it into the output contract as a structural constraint the format requires, not as a sentence in the role text. A rule stated in the role can be drifted past. A constraint built into the output contract shapes the response format and cannot be quietly ignored.

**Packaging:** A versioned Skill. The procedure is stable, run identically hundreds of times a day across the support org, and needs versioning and rollback. A pasted prompt template each agent keeps locally has no central governance, cannot be rolled back, and drifts into slightly different versions over time.

</details>
