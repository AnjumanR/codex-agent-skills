---
name: codex-job-control
description: Manage Codex runtime setup and job lifecycle using setup, status, result, and cancel commands.
---

# Codex Job Control

Use this skill to check readiness and manage active/recent Codex jobs.

## Commands

```bash
node runtime/scripts/codex-companion.mjs setup [--enable-review-gate|--disable-review-gate] [--json]
node runtime/scripts/codex-companion.mjs status [job-id] [--all] [--json]
node runtime/scripts/codex-companion.mjs result [job-id] [--json]
node runtime/scripts/codex-companion.mjs cancel [job-id] [--json]
```

## Rules

- Use `setup` before first run or when auth/runtime issues appear.
- Use `status` for queue/progress checks.
- Use `result` to fetch final stored output for a finished job.
- Use `cancel` only for active jobs.
- Keep job IDs exact when multiple jobs are active.
