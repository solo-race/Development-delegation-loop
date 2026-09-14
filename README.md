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
- `agents/scout.toml` — read-only, low-cost evidence worker.
- `agents/implementer.toml` — bounded high-capability code writer.
- `agents/tester.toml` — external validation worker; build/test side effects allowed, source edits forbidden by role contract.
- `agents/registry.example.toml` — role declarations to copy into the deployment `config.toml`.
- `references/implementation-brief.md` — bounded code handoff.
- `references/evidence-report.md` — scout/test evidence return.
- `references/final-review-package.md` — phase-close audit package.

The main supervisor is intentionally not defined as a subagent role, and there is no internal reviewer role: planning and final cross-phase review remain upstream.

## Design intent

Stable workflow rules live in the skill; per-task prompts contain only the implementation delta. The skill is model- and harness-agnostic at the contract layer; `agents/` provides one Codex deployment profile matching the current cost/capability split and can be replaced without changing `SKILL.md`.
