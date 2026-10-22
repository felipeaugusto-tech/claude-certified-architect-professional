# 03 · Guardrails — Place the controls

*Place the controls on the path · 4 min*

**Try it now.** You are given four controls. Each one answers two questions: where it sits on the request path, and what kind of check it is. Drop each control onto a grid. The horizontal axis is the placement point, from input screening to action authorization before a tool runs, to output filtering. The vertical axis is whether a deterministic check or a model-based check fit. Correct placement reveals the reasoning. **Not every zone will be filled, two cells will remain empty.**

> **Note:** the course page doesn't include the answers. The answers below are inferred from the teaching content, not the official key.

### Grid

| Placement | Model-based | Deterministic |
|---|---|---|
| **Input Screening** | | |
| **Output Screening** | | |
| **Action Authorization** | | |

### Controls

- **A.** Jailbreak and prompt-injection screen
- **B.** Banned term blocklist on the user message
- **C.** Toxicity judge on the generated response
- **D.** Refund authorization policy check before the tool runs

<details><summary>Answer</summary>

| Placement | Model-based | Deterministic |
|---|---|---|
| **Input Screening** | **A.** Jailbreak and prompt-injection screen | **B.** Banned term blocklist |
| **Output Screening** | **C.** Toxicity judge | *(empty)* |
| **Action Authorization** | *(empty)* | **D.** Refund authorization policy check |

Jailbreak intent and toxicity need language understanding, so they are model-based; a blocklist is a defined rule; and authorization must be provable and replayable, so it is deterministic. *(See 03 · Guardrails → Placing screening and authorization → Where each check fits.)*

</details>
