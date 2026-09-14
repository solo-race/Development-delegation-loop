# Final Review Package

Use this template when the supervisor has completed all acceptance units in a phase and needs an upstream final audit.

```text
FINAL_REVIEW_PACKAGE

phase:
status: READY_FOR_FINAL_REVIEW

contract_summary:
- <phase purpose>
- <acceptance boundaries>

completed_units:
- unit_id: <id>
  status: ACCEPTED
  changed_files:
    - <path>
  acceptance_evidence:
    - <criterion -> evidence reference>

validation_summary:
- check: <test/build/device/runtime check>
  result: PASS | FAIL | NOT_RUN
  evidence: <command/artifact/reference>

scope_integrity:
- unexpected_changes: none | <list>
- contract_refinements: none | <list with upstream approval reference>
- known_out_of_scope_items: none | <list>

repo_state:
- head: <commit/ref if available>
- working_tree: <clean/dirty/unknown>
- relevant_artifacts:
    - <artifact or report>

residual_risks:
- <risk or none>

review_questions:
- Does the implementation satisfy the written phase contract?
- Did any change cross architectural or forbidden scope?
- Are tests and runtime evidence sufficient for acceptance?
- Did the completed work invalidate assumptions in later phases?
- Is follow-up planning required before the next phase?
```

The supervisor does not convert `READY_FOR_FINAL_REVIEW` into architectural approval. That decision belongs to the upstream reviewer.
