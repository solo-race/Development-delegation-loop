# Repository Editing Guide

This file applies to the whole repository. It defines editing constraints and a compact file index for contributors and coding agents. Runtime behavior is defined by `SKILL.md` and the linked loop/module contracts; do not treat this file as a second workflow specification.

## Editing constraints

- Preserve separation of concerns:
  - `SKILL.md` owns entry routing, session supervision policy, shared roles, boundaries, and states.
  - `loops/` owns executable planning and implementation flow.
  - `modules/` owns reusable workflow modules such as independent review.
  - `agents/` owns deployment-oriented role profiles, not workflow policy.
  - `references/` owns compact handoff schemas.
- Do not duplicate stable workflow rules across files. Put a rule in its owning contract and link to it elsewhere.
- Keep the contract layer model/provider agnostic. Model, provider, reasoning-effort, and sandbox choices belong in `agents/` or deployment/runtime configuration unless the contract genuinely depends on them.
- Do not collapse planning and implementation into one loop. Planning creates or repairs an executable phase contract; implementation executes an already usable phase contract.
- Preserve the mandatory session supervision gate. Never infer human supervision from time of day, recent user activity, or earlier sessions.
- Preserve routing semantics: explicit planning requests or absence of a usable detailed current-phase plan route to planning; otherwise route to implementation.
- Planning must materialize the current phase. If no project plan exists, establish the master objective and phase sequence first, then detail the current phase.
- Under `human_supervisor=false`, a mechanically complete implementation phase must pass independent review before advancing. Under `human_supervisor=true`, do not add automatic review unless explicitly requested.
- Keep reviewer independence: reviewer is read-only, does not repair findings, and should receive the review package rather than the implementer's private reasoning history.
- Keep repository discovery with scouts where practical. Planner/reviewer/implementer should receive bounded, source-backed context rather than broad repository dumps.
- Treat current HEAD and direct repository evidence as authoritative over stale summaries or conversation memory.
- Preserve role boundaries: scouts inspect, planners plan, implementers edit bounded scope, testers validate, reviewers audit, and the main supervisor coordinates state and routing.
- Keep handoff templates compact and factual. Observation/evidence must remain distinguishable from inference and planning judgment.
- Avoid unrelated refactors while changing workflow behavior. Make the smallest coherent contract change and propagate only required schema/link updates.
- When renaming, moving, adding, or deleting contract files, update all repository-relative links plus the file indexes in `README.md` and this file in the same change.
- When changing state names, handoff fields, or required dispositions, update every producer/consumer contract and example that references them.

## Edit verification

Before considering a repository-structure or contract edit complete:

1. Read `SKILL.md` plus every loop/module/template directly affected by the change.
2. Check repository-relative Markdown links and role/profile references.
3. Check that routing, supervision, planning, implementation, and review semantics do not contradict each other.
4. Verify that provider-specific configuration has not leaked into the model-agnostic contract layer.
5. Report changed files and any intentionally unresolved deployment/runtime assumptions.

## File index

- `README.md` — human-facing overview, architecture sketch, repository structure, and deployment intent.
- `SKILL.md` — primary skill entry contract: session gate, routing, supervision policy, shared roles/boundaries, states, and links to executable contracts.
- `AGENTS.md` — repository-wide editing constraints and file index.

### Loops

- `loops/planning.md` — planning loop; bootstraps a master project plan when absent and produces/revises the detailed current-phase plan.
- `loops/implementation.md` — execution loop for bounded acceptance units, external validation, retries, phase close, and supervision-dependent review transition.

### Modules

- `modules/review.md` — reusable independent review protocol for unattended phase close and optional unattended plan review.

### Agent profiles

- `agents/scout.toml` — read-only evidence scout.
- `agents/planner.toml` — high-reasoning planner operating on prepared planning packets.
- `agents/implementer.toml` — bounded code implementer.
- `agents/tester.toml` — external validation executor.
- `agents/reviewer.toml` — independent read-only reviewer; provider/model may be selected by deployment or OMP.
- `agents/registry.example.toml` — example role registration for deployment configuration.

### Handoff references

- `references/implementation-brief.md` — bounded implementation handoff.
- `references/evidence-report.md` — source-backed scout/test evidence return.
- `references/planning-packet.md` — compressed evidence/context packet supplied to the planner.
- `references/phase-plan.md` — detailed executable phase-plan schema.
- `references/review-package.md` — independent review input package for implementation or planning review.
