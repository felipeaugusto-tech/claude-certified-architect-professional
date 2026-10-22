# 05 · Review routing — Build the review-routing rule

*Build the review-routing rule · 3 min*

**Try it now.** A routing rule has three controls: a confidence threshold, the cost of a wrong answer, and a reversibility setting. The goal is to route high-stakes, low-confidence decisions to a person before they take effect, while keeping the review queue under the reviewer ceiling. Choose the rule that meets both targets at once.

> **Note:** the course page doesn't include the answer to Question 1. That answer is inferred from the teaching content, not the official key. The model answer to Question 2 is the course's own.

### Question 1

- **A.** Route everything below a high confidence threshold to review, regardless of stakes.
- **B.** Route decisions that are low-confidence AND either irreversible or high-cost to pre-action review; let confident, reversible, low-cost decisions through.
- **C.** Route by confidence alone, set low enough to keep the queue small.
- **D.** Send every decision to review to be safe.

<details><summary>Answer</summary>

**B** — it keys the rule to stakes (reversibility and cost) and uses confidence only to filter volume; A and C ignore stakes, and D floods the queue. *(See 05 · Review routing → Routing by stakes → What sets the stakes of a decision.)*

</details>

### Question 2

Now complete this step: In 1–2 sentences, name the single deciding control, what makes a low-confidence case still route to a person even when confidence is within tolerance?

<details><summary>Model answer</summary>

Reversibility and the cost of a wrong answer are the deciding controls, not confidence. Confidence filters the volume you route, but it does not change the stakes of the decision. A case can be confident and still route to a human if it is irreversible or high-cost enough to warrant it.

</details>
