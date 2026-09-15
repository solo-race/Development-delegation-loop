# Development Delegation Loop

Repository-scale orchestration skill with two explicit workflows: planning and implementation. A session-level supervision gate decides whether implementation is directly supervised by a human or must pass independent review before each phase advances.

```text
load skill
   |
   +-- ask supervision gate once
   |
   +-- plan gate
       |
       +-- no detailed current-phase plan / explicit plan request
       |      -> planning loop -> optional unattended review -> sealed phase plan
       |
       +-- usable detailed phase plan
              -> implementation loop
                    |
                    +-- human supervisor -> mechanical acceptance -> advance
                    +-- unattended -> phase review -> revise/revalidate until PASS
```

## Structure

- `SKILL.md` — entry contract, session gate, routing rules, shared roles and states.
- `loops/planning.md` — project-plan bootstrap and current-phase planning loop.
- `loops/implementation.md` — bounded implementation, validation, retry, and phase-close flow.
- `modules/review.md` — reusable independent review protocol for unattended execution.
- `agents/` — example Codex role profiles; runtime/provider selection remains deployment policy.
- `references/` — compact handoff schemas used between supervisor and workers.

## Planning model

The supervisor and cheap scouts gather repository/document evidence first. The planner receives a source-backed planning packet rather than paying to rediscover the repository. When no project plan exists, planning first establishes the master objective and phase sequence, then materializes the current phase. When a master plan already exists, planning only expands or repairs the current phase unless the user explicitly requests broader replanning.

## Review model

Review is a supervision policy, not an implementation role. With a human supervisor present, automatic review stays off. Without one, each mechanically complete phase is independently audited from the contract, diff/change set, validation evidence, repository state, and prior findings. The reviewer does not inherit the implementer's reasoning history and does not edit code.

## Deployment

`agents/registry.example.toml` shows one role registry. `planner.toml` intentionally targets the high-reasoning planner profile; reviewer model/provider choice can be overridden by the runtime or OMP deployment without changing the skill contract.
