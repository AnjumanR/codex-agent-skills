---
name: codex-review
description: Run a read-only Codex review of local git changes (working tree or base-branch diff) and return the review output verbatim.
---

# Codex Review

Use this skill when the user asks for a code review pass over current repository changes.

## Command

Run:

```bash
node runtime/scripts/codex-companion.mjs review [--wait|--background] [--base <ref>] [--scope auto|working-tree|branch]
```

## Rules

- This is review-only. Do not edit files or apply fixes in the same turn.
- Return Codex review output faithfully.
- Prefer `--background` for medium/large diffs.
- Use `--base <ref>` for branch-to-branch review.
- If no target is provided, let the runtime auto-select (`working-tree` when dirty, otherwise branch diff).

## Follow-up

If the user wants strict design pressure-testing, use `codex-adversarial-review`.
