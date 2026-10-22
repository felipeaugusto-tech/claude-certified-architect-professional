# 05 · Wrap-up — Module quiz

*Module quiz*

Five scenario questions across the three topics. Choose the best answer; the feedback names the principle.

> **Note:** the course page doesn't include the answers. The answers below are inferred from the teaching content, not the official key.

### Question 1 · Team setup

A team is rolling Claude out to four departments at once and adoption is uneven. What is the best next move?

- **A.** Mandate daily usage targets for everyone.
- **B.** Enable a champion in each department first, prove the workflow, then seed adoption in batches.
- **C.** Wait until each department asks for help.
- **D.** Give the strongest model to everyone to encourage use.

<details><summary>Answer</summary>

**B** — the champion-then-batch rollout spreads adoption deliberately and gives each department a local expert and working example. *(See 02 · Team Setup → Configuring Claude tooling for teams → Roll out through champions, then batches.)*

</details>

### Question 2 · Skills distribution

A procedure must be run identically by every department and be revocable from one place. How should it be distributed?

- **A.** Pasted into each team's chat as a prompt.
- **B.** Bundled into an organization-managed plugin distributed to all departments, with group/org targeting, version-controlled updates, and rollback.
- **C.** A Claude Code project config in one team's repo.
- **D.** Emailed as a document for people to follow.

<details><summary>Answer</summary>

**B** — central revocation plus identical execution across groups needs plugin governance: targeting, versioned updates, and rollback. *(See 02 · Team Setup → Configuring Claude tooling for teams → Skill distribution mechanisms.)*

</details>

### Question 3 · Developer workflows

A team ships AI-generated code faster but a security issue slips through. What was most likely missing?

- **A.** A code-review SLA that exempted small AI-generated changes from security review.
- **B.** A linter configured to flag known vulnerability patterns before merge.
- **C.** A verification checklist that AI-generated code must pass before production, including a security dimension.
- **D.** More frequent model updates to incorporate recent security patterns.

<details><summary>Answer</summary>

**C** — the verification checklist is the diligence deliverable that gates AI output on correctness, security, maintainability, and human understanding before production. *(See 03 · Dev Workflows → Improving developer workflows with AI tooling → Diligence.)*

</details>

### Question 4 · Judgment

In review, a developer cannot explain why an AI-generated change handles an input the way it does, but the tests pass. What should happen?

- **A.** Merge it; the tests are green.
- **B.** Hold it until the author can explain the behavior and its rationale, the human-understanding check.
- **C.** Delete the tests and rewrite by hand.
- **D.** Escalate to the Architect for every merge.

<details><summary>Answer</summary>

**B** — passing tests aren't enough; the person merging must be able to explain what the code does and why, which is exactly the check the "merge nobody could explain" incident lacked. *(See 03 · Dev Workflows → Improving developer workflows with AI tooling → Watch out for.)*

</details>

### Question 5 · Operational support

Output quality on a live deployment has degraded over two months with no code changes. Where does the Architect look first?

- **A.** Increase the model tier; a more capable model will compensate for retrieval gaps.
- **B.** A model or prompt change, or retrieval drift as the corpus grew, connect the symptom to an architecture cause.
- **C.** Disable caching to ensure every call pulls fresh content.
- **D.** Roll back the last code deployment and re-run integration tests.

<details><summary>Answer</summary>

**B** — gradual degradation with no code change maps to a model/prompt change or retrieval drift in the symptom-to-cause table; D can't apply because nothing in the code changed. *(See 04 · Ops Support → Supporting debugging and operational issue resolution → Connect symptoms to architecture causes.)*

</details>
