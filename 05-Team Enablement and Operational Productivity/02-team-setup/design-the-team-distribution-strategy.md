# 02 · Team Setup — Design the team distribution strategy

*Checkpoint: design the team distribution strategy*

Try it now. For each scenario below, choose how the team should receive the reusable asset, and identify the factor that makes that mechanism the right one. A correct mechanism paired with the wrong reason does not pass.

> **Note:** the course page doesn't include the answers. The answers below are inferred from the teaching content, not the official key.

**Mechanisms:** Org-provisioned Skill (Organization settings › Skills) · Claude Code project Skill · Plugin distributed org-wide / to a group · API Skill (Messages API container)

### Scenario A

A compliance-review procedure every department must run identically, that must be centrally updatable and roll-back-able.

<details><summary>Answer</summary>

**Plugin distributed org-wide (or to all relevant groups)** — the deciding factor is the need for central updates and rollback, which only governed plugin distribution provides (version-controlled updates from a connected repo). *(See 02 · Team Setup → Configuring Claude tooling for teams → Skill distribution mechanisms.)*

</details>

### Scenario B

A capability that genuinely should be available to every member, with no need for versioning or rollback.

<details><summary>Answer</summary>

**Org-provisioned Skill** — the deciding factor is org-wide reach with no governance requirement; it's the simplest path when everyone should have it. *(See 02 · Team Setup → Configuring Claude tooling for teams → Skill distribution mechanisms.)*

</details>

### Scenario C

A coding convention and tool set the engineering team should share on every project.

<details><summary>Answer</summary>

**Claude Code project Skill** — the deciding factor is that it's one team's convention for its own projects, so it should live in the repo (`.claude/skills/`) and version with the code. A plugin assigned to the engineering group is the alternative if the team needs install preferences or central rollback. *(See 02 · Team Setup → Configuring Claude tooling for teams → Skill distribution mechanisms.)*

</details>

### Scenario D

A reusable capability several of the partner's own products must call programmatically.

<details><summary>Answer</summary>

**API Skill (Messages API container)** — the deciding factor is machine-to-machine reuse by the partner's products, governed in the calling system with explicit version pinning. *(See 02 · Team Setup → Configuring Claude tooling for teams → Skill distribution mechanisms.)*

</details>
