---
name: codex-rescue
description: Delegate diagnosis, implementation, research, or follow-up tasks to Codex task runtime with optional model, effort, resume, and background controls.
---

# Codex Rescue

Use this skill when the user wants Codex to investigate or implement work directly.

## Command

Run:

```bash
node runtime/scripts/codex-companion.mjs task [--background] [--write] [--resume-last|--resume|--fresh] [--model <model|spark>] [--effort <none|minimal|low|medium|high|xhigh>] [prompt]
```

## Rules

- Preserve the user task intent and scope.
- Default to write-capable mode when the user expects fixes (`--write`).
- Use `--resume-last` for follow-up instructions on the latest task thread.
- Keep `--effort` unset unless the user explicitly asks for a reasoning effort.
- Map `spark` to model alias support via runtime.
- Return Codex task output faithfully; do not replace failed Codex execution with an invented answer.

## Common Patterns

- New task: `node runtime/scripts/codex-companion.mjs task --write fix flaky pagination test`
- Resume: `node runtime/scripts/codex-companion.mjs task --resume-last apply the safest fix`
- Background: `node runtime/scripts/codex-companion.mjs task --background investigate regression`
