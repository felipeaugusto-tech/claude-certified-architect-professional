# 02 · Alignment — Sort the responsibility

*Sort the responsibility · 3 min*

**Try it now.** You are reviewing the safety design for a partner's Claude assistant. Drag each obligation into the bucket that should enforce it: either Claude's trained behavior, or the application layer. The feedback explains why each one falls where it does.

> **Note:** the course page doesn't include the answers. The answers below are inferred from the teaching content, not the official key.

**Buckets:** Claude's trained behavior · Application layer (you own it)

### Obligations

- **A.** Refusing to help synthesize a dangerous weapon
- **B.** Never returning another tenant's data
- **C.** Declining to produce plainly hateful content
- **D.** Blocking advice outside the partner's approved script
- **E.** Requiring a sign off before a refund is issued

<details><summary>Answer</summary>

| Obligation | Bucket |
|---|---|
| A. Refusing to help synthesize a dangerous weapon | Claude's trained behavior |
| B. Never returning another tenant's data | Application layer |
| C. Declining to produce plainly hateful content | Claude's trained behavior |
| D. Blocking advice outside the partner's approved script | Application layer |
| E. Requiring a sign off before a refund is issued | Application layer |

Broad, general-purpose harm (A, C) is reduced by training; anything specific to the partner's data rules, script, or authorization model (B, D, E) was never given to Claude and must be enforced in a layer you build. *(See 02 · Alignment → Model training vs your layer.)*

</details>
