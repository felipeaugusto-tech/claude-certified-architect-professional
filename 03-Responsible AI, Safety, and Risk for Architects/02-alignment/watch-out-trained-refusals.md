# 02 · Alignment — Watch Out: trained refusals

*When trained refusals are mistaken for a domain policy · 3 min*

> **Setup hook**
>
> You are a strong systems Architect. Claude already refuses broadly harmful requests in testing, so you assume its training also covers your partner's data handling policy. You move on without building a separate enforcement layer.

## A postmortem: a policy that was never encoded in any layer

A team deployed an internal assistant for a partner whose data-handling policy prohibited users from accessing records belonging to other business units. In review, Claude had refused every harmful prompt the team threw at it, so they assumed cross-unit disclosure was covered by the same safety behavior and never built an authorization check for it.

In production, a normal-looking, in-domain request asked for a forbidden record. Nothing in the request looked harmful in general terms, so Claude answered it. The rule the team believed was enforced actually did not exist. It was never part of Claude's training, and the team never encoded it in a classifier, system prompt, or application control, because they assumed the model already covered it.

> **Why this broke**
>
> A domain policy was conflated with trained alignment. Trained refusals cover broad harm, not deployment-specific rules. Any rule that is specific to your partner must be enforced in a layer you build. Remember, Claude cannot enforce what it was never given.
