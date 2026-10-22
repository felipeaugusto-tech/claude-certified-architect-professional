# 06 · Entry Point — Pick the platform and required outcome fields

*Checkpoint: pick the platform and the required outcome fields*

Try it now. You are given a deployment scenario with three variables: the partner's primary cloud platform, the deployment's regulatory obligation level, and the primary performance constraint. Set the three variables. A decision model maps the combination to a recommended primary platform, a secondary platform where one applies, and the two outcome-document fields the combination needs to be reusable.

> **Note:** the course page doesn't include the answers. The answers below are inferred from the teaching content, not the official key.

### Scenario variables

- **Primary cloud platform:** AWS · GCP · Microsoft Foundry · Direct (no cloud)
- **Regulatory obligation level:** None · Moderate · Strict
- **Primary performance constraint:** Latency-sensitive · Cost-sensitive · Compliance-sensitive

### Your answer

- **Primary platform:** AWS Bedrock · GCP Vertex AI · Microsoft Foundry · Direct Anthropic API
- **Secondary platform:** Direct Anthropic API · None required
- **Required outcome fields:**
  - Control in place (auditable) + Measurement owner
  - Metric before + Metric after
  - Metric before + Metric after + Reuse potential

<details><summary>Answer</summary>

The course's decision model is hidden in the page script, so this is the rule the teaching implies, not the model's actual output:

- **Primary platform:** the partner's cloud route (Bedrock, Vertex AI or Foundry) when they are standardized on a cloud. Use the Direct Anthropic API when there's no cloud commitment.
- **Secondary platform:** the Direct Anthropic API, for non-regulated work that wants the newest features, when the primary is a cloud route. Otherwise none.
- **Required outcome fields:**
  - Strict obligation or compliance-sensitive: **Control in place (auditable) + Measurement owner**, so the result is evidenced rather than asserted.
  - Otherwise: **Metric before + Metric after**, plus Reuse potential where the pattern is meant to become partner IP.

**Worked example (the cumulative brief):** AWS, strict and compliance-sensitive gives **AWS Bedrock** (explicit in-region) as primary, the **Direct Anthropic API** as secondary, and **Control in place + Measurement owner** as the fields.

*(See 06 · Entry Point → Entry point selection and the outcome document → The deployment-entry-point decision matrix.)*

</details>
