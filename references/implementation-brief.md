# Implementation Brief

Use this template for the handoff from the main supervisor to the implementation worker. Keep it bounded and evidence-backed.

```text
IMPLEMENTATION_BRIEF

unit_id:
phase:
status: READY_TO_IMPLEMENT

goal:
- <single bounded implementation goal>

acceptance_contract:
- <criterion 1>
- <criterion 2>

allowed_scope:
- <file/path/symbol allowed to change>

forbidden_scope:
- <files/modules/behaviors that must not change>

relevant_evidence:
- source: <path[:symbol/line]>
  fact: <observed fact>
- source: <path[:symbol/line]>
  fact: <observed fact>

constraints:
- <API/backward-compat/security/project constraint>

known_risks:
- <risk or none>

validation_to_be_run_externally:
- <exact test/build/runtime check>

required_return:
- changed files
- concise implementation rationale
- unresolved concerns
- BLOCKED plus reason if the brief is insufficient

worker_rules:
- Modify only allowed scope.
- Do not broaden repository discovery.
- Do not redesign the contract.
- Do not claim tests passed; tests are executed externally.
- If an out-of-scope change is required, stop and return BLOCKED.
```
