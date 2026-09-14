# Development Delegation Loop

Lean orchestration skill for executing an existing repository phase plan with strict role separation.

```text
upstream plan/review
        ↓
main supervisor
  ├─ scouts: inspect only
  ├─ implementer: bounded code only
  └─ test executor: evidence only
        ↓
mechanical acceptance + persisted state
        ↓
upstream final review
```

The supervisor loads `SKILL.md` once. Workers receive only the relevant contract fragment and a compact template from `references/`.

## Files

- `SKILL.md` — stable execution contract, role boundaries, loop, gates, and states.
- `references/implementation-brief.md` — bounded code handoff.
- `references/evidence-report.md` — scout/test evidence return.
- `references/final-review-package.md` — phase-close audit package.

## Design intent

Stable workflow rules live in the skill; per-task prompts contain only the implementation delta. The skill is model- and harness-agnostic and is intended for indexing by a global skill router.
