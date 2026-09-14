---
name: development-delegation-loop
description: Use this skill when a software project already has an upstream phase plan or acceptance contract and the main coding thread should act only as a supervisor. It defines a multi-agent development loop where repository/document inspection and test execution are delegated to cheap external agents, a bounded high-capability implementer writes code from a prepared brief, the supervisor performs mechanical acceptance and routing, and final cross-phase review is escalated to an upstream reviewer. Trigger on phase execution, acceptance-contract implementation, delegated repository work, external scouts/testers, bounded implementation workers, or final audit packaging.
---

# Development Delegation Loop

Use this skill once at the start of a delegated implementation session. Do not broadcast the full skill to subagents. Give each subagent only its role-specific brief and required evidence format.

## Purpose

Minimize expensive supervisor and implementer context while preserving control. Separate planning, evidence gathering, implementation, test execution, acceptance, and final review.

The loop assumes that an upstream planner/reviewer has already written the phase plan and acceptance contract to the repository. If no usable contract exists, stop and escalate instead of inventing one.

## Architecture

```text
Upstream planner / final reviewer
  requirement discussion -> phase contracts -> final audit
                         |
                         v
                  Main supervisor
        select -> delegate -> observe -> verify
        -> advance or block; do not redesign
                         |
          +--------------+--------------+
          |              |              |
          v              v              v
  Scout / evidence   Implementer     Test executor
  repo/docs/code     bounded code    build/test/device
  inspection only    changes only    evidence only
          |              |              |
          +--------------+--------------+
                         |
                         v
                  Main supervisor
             acceptance + state update
                         |
                         v
              Final review package
                         |
                         v
             Upstream final reviewer
```

## Roles

### Upstream planner / final reviewer

Outside this loop. Owns architecture, phase decomposition, acceptance contracts, and final cross-phase audit. A plan may leave implementation details open, but it must define scope, dependencies, acceptance criteria, required evidence, and allowed refinement.

### Main supervisor

Owns orchestration, not implementation. Read only the current plan/status plus returned evidence unless a targeted verification is required.

The supervisor must:

1. Select the next acceptance unit from the persisted plan/status.
2. Delegate repository/document/code inspection instead of performing broad discovery itself.
3. Consolidate returned evidence into a bounded implementation brief.
4. Send implementation work to the designated implementer.
5. Delegate all build, test, device, or runtime verification.
6. Compare evidence against the written acceptance criteria.
7. Advance, retry, block, or escalate without silently changing the contract.
8. Persist status and prepare the final review package when the phase closes.

The supervisor must not redesign the phase, perform opportunistic refactors, become the default repository scout, patch code after a worker failure, or declare cross-phase architectural acceptance on its own.

### Scout / evidence worker

Read-only. May inspect repository files, documentation, code, configuration, history, or other sources only within the delegated question. Return evidence, not implementation.

Every material conclusion must identify its source by file/path and, when practical, symbol or line range. Distinguish observed facts from inference. Do not modify files, run destructive commands, or broaden the task without reporting the need first.

### Implementer

Receives a prepared implementation brief and writes the bounded code change. It may inspect only the target files and explicitly permitted adjacent context needed to edit safely. It must not perform broad repository discovery, redesign the contract, run the full test suite, or expand scope on its own.

If the brief is insufficient or implementation requires an out-of-scope change, return `BLOCKED` with the missing information or required scope change instead of exploring freely.

### Test executor

Runs the exact validation requested by the acceptance contract or implementation brief. It does not edit code. Return commands, exit status, relevant output, environment constraints, and reproducibility notes. Device/manual verification belongs here as evidence when applicable.

## Core Loop

1. **Load once.** Read this skill, current project status, and the active phase/acceptance plan. Do not recursively load unrelated project material.
2. **Select.** Choose one acceptance unit whose dependencies are satisfied.
3. **Gather evidence.** Delegate targeted scouts for repository state, documentation, code location, interfaces, and known constraints. Parallelize independent questions.
4. **Verify evidence.** Resolve only material conflicts. Do not repeat broad searches merely to duplicate a scout's work.
5. **Build the implementation brief.** Use `references/implementation-brief.md`. Include goal, allowed files/scope, forbidden scope, evidence, contract constraints, acceptance tests, and known risks.
6. **Implement.** Send the brief to the designated implementer. The implementer returns changed files, implementation notes, unresolved concerns, and no fabricated test claims.
7. **Test externally.** Delegate validation to a test executor. Use `references/evidence-report.md` for the returned evidence.
8. **Route failures.** If validation fails, send the failure evidence back to the implementer. The supervisor does not patch the code itself. Repeat only while the contract remains valid.
9. **Accept or block.** Mark the unit complete only when every acceptance item has direct evidence. Otherwise block or escalate.
10. **Persist.** Update project status/evidence using the project's existing status mechanism. Do not invent a new project-state format unless the plan explicitly requires one.
11. **Close phase.** When all units pass, create `references/final-review-package.md` and escalate it to the upstream final reviewer. The supervisor reports `READY_FOR_FINAL_REVIEW`, not architectural final approval.

## Escalation Conditions

Stop mechanical execution and escalate when any of these is true:

- the acceptance contract is missing, ambiguous, contradictory, or stale;
- implementation requires changing architecture or crossing forbidden scope;
- scouts return materially conflicting evidence that targeted verification cannot resolve;
- repository HEAD/state changed enough to invalidate gathered evidence;
- required validation cannot be executed or its environment is unavailable;
- repeated implementation attempts fail for the same underlying reason;
- a security, permissions, data-loss, migration, or irreversible decision is not already covered by the contract;
- completing the current unit would invalidate a later phase contract.

## Evidence Rules

Evidence is stronger than narrative. Prefer exact paths, symbols, commands, test names, exit codes, diffs, and artifact identifiers over summaries.

A statement such as `tests pass` is insufficient. Record which tests ran and their result. A statement such as `the code already supports X` is insufficient without a source location. Do not convert inference into fact.

## Context and Cost Rules

- Load this full skill only into the main supervisor.
- Subagents receive only the minimum role brief, relevant contract fragment, and required output template.
- Prefer cheap/high-throughput agents for reading, searching, documentation inspection, build/test loops, and device/runtime evidence.
- Reserve high-capability implementers for bounded code reasoning and code changes.
- Do not spend supervisor context re-reading material already represented by trustworthy evidence unless acceptance requires direct verification.
- Keep implementation briefs stable across retries so cached context remains reusable when the underlying evidence has not changed.

## Output States

The supervisor should end each acceptance unit in exactly one of these states:

- `ACCEPTED` — all contract items have evidence.
- `RETRY_IMPLEMENTATION` — implementation failed but the contract and evidence remain valid.
- `REVERIFY` — evidence is stale, incomplete, or conflicting and must be reacquired.
- `BLOCKED` — execution cannot continue mechanically.
- `ESCALATE_PLAN` — upstream planning/architecture decision is required.

A completed phase ends as `READY_FOR_FINAL_REVIEW` until the upstream reviewer approves it.

## Reference Templates

- [Implementation brief](references/implementation-brief.md)
- [Evidence report](references/evidence-report.md)
- [Final review package](references/final-review-package.md)
