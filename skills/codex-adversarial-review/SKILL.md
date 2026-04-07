---
name: codex-adversarial-review
description: Run a steerable adversarial Codex review that challenges implementation assumptions, risks, and design tradeoffs.
---

# Codex Adversarial Review

Use this skill when the user wants Codex to pressure-test a design or implementation, not just look for standard defects.

## Command

Run:

```bash
node runtime/scripts/codex-companion.mjs adversarial-review [--wait|--background] [--base <ref>] [--scope auto|working-tree|branch] [focus text]
```

## Rules

- This is review-only. Do not make code edits in the same turn.
- Keep any user-provided focus text intact.
- Return Codex output faithfully, including findings and uncertainty.
- Prefer `--background` unless the review scope is clearly tiny.

## Focus Examples

- `challenge whether this retry/caching design is safe`
- `look for race conditions and rollback hazards`
