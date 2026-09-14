# Development Delegation Loop

Prototype orchestration skill for repository development with strict role separation.

The intended flow is:

```text
Upstream planner / final reviewer
        -> phase plan + acceptance contract
        -> main supervisor
        -> external scouts / bounded implementer / external test executor
        -> mechanical acceptance + persisted evidence
        -> final review package
        -> upstream final reviewer
```

The main supervisor loads `SKILL.md` once per implementation session. Subagents do not receive the full skill; they receive only the relevant contract fragment and one role-specific template from `references/`.

## Files

- `SKILL.md` — orchestration rules, roles, loop states, escalation conditions, and context/cost rules.
- `references/implementation-brief.md` — bounded handoff to the code implementer.
- `references/evidence-report.md` — scout/test evidence contract.
- `references/final-review-package.md` — phase-close package for upstream audit.

## Router intent

This skill should be indexed by a global skill router for tasks such as multi-agent phase execution, acceptance-contract implementation, delegated repository inspection, external test execution, bounded code workers, and final audit packaging.

The router should expose only the skill name and trigger description until the main supervisor selects it. The full skill should not be injected into every worker context.

## Current status

This is an initial contract-level prototype. It intentionally does not prescribe a specific model, harness, repository-state format, or test runner. Those remain deployment concerns so the loop can be reused across Codex, OMP, MCP-backed workers, and project-specific tooling.
