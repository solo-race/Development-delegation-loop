# Evidence Report

Use this template for scout and test-executor returns. Report observations separately from inference.

```text
EVIDENCE_REPORT

unit_id:
role: SCOUT | TEST_EXECUTOR
status: COMPLETE | PARTIAL | BLOCKED

observed_facts:
- source: <path[:symbol/line] | command | artifact>
  fact: <directly observed result>

inferences:
- inference: <derived conclusion>
  based_on:
    - <source/fact>
  confidence: high | medium | low

validation:
- command: <exact command, if applicable>
  exit_status: <code or n/a>
  result: PASS | FAIL | NOT_RUN
  relevant_output: <minimal decisive output>

environment_constraints:
- <constraint or none>

unresolved_questions:
- <question or none>

scope_notes:
- <anything required outside delegated scope; do not act on it>
```

Rules:

- Never report an inference as an observed fact.
- Prefer file paths, symbols, commands, exit codes, and artifact IDs over prose.
- A scout does not modify files.
- A test executor does not repair failures.
- If evidence became stale because repository state changed, return `PARTIAL` or `BLOCKED` and identify the stale assumption.
