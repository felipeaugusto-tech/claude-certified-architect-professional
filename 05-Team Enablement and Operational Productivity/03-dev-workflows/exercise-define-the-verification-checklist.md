# 03 · Dev Workflows — Exercise: define the verification checklist

*Exercise: define the verification checklist*

Try it now. Write the verification checklist that AI-generated code must pass before production. For each of the four dimensions below, write one concrete check in your own words. Write your checklist, then reveal the model answer below.

## Your checklist

1. **Correctness** — *Write your check for correctness...*
2. **Security** — *Write your check for security...*
3. **Maintainability** — *Write your check for maintainability...*
4. **Human understanding** — *Write your check for human understanding...*

<details><summary>Model answer</summary>

**Correctness:** Tests exist and pass, and the behavior matches the stated requirement including edge cases.

**Security:** No secrets in code; inputs are validated; any tools or external calls use least-privilege access.

**Maintainability:** The code reads clearly, follows team conventions, and contains no unexplained complexity.

**Human understanding:** The developer submitting the change can explain what the code does and why, including how it handles the inputs it was not explicitly tested against.

</details>
