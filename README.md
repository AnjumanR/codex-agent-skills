# Codex Agent Skills

Harness-agnostic skills for running Codex as a review and delegation companion.

## Install

Install from GitHub with the generic skills CLI:

```bash
npx -y skills add <owner>/codex-agent-skills
```

List available skills before install:

```bash
npx -y skills add <owner>/codex-agent-skills --list
```

Install only specific skills:

```bash
npx -y skills add <owner>/codex-agent-skills --skill codex-review codex-adversarial-review
```

## Included Skills

- `codex-review`: read-only review of local git changes
- `codex-adversarial-review`: steerable challenge review of implementation decisions
- `codex-rescue`: delegate diagnosis/fix/research tasks to Codex
- `codex-job-control`: setup, status, result, and cancel commands
- `gpt-5-4-prompting`: prompt construction patterns for stronger Codex runs
- `codex-result-handling`: rules for preserving Codex output fidelity

## Runtime Commands

Each operational skill bundles a self-contained runtime at:

```bash
runtime/scripts/codex-companion.mjs
```

When working from the repo directly, you can also use the root runtime:

```bash
node scripts/codex-companion.mjs <subcommand> [...args]
```

Supported subcommands:

- `setup [--enable-review-gate|--disable-review-gate] [--json]`
- `review [--wait|--background] [--base <ref>] [--scope auto|working-tree|branch]`
- `adversarial-review [--wait|--background] [--base <ref>] [--scope auto|working-tree|branch] [focus text]`
- `task [--background] [--write] [--resume-last|--resume|--fresh] [--model <model|spark>] [--effort <none|minimal|low|medium|high|xhigh>] [prompt]`
- `status [job-id] [--all] [--json]`
- `result [job-id] [--json]`
- `cancel [job-id] [--json]`

## Requirements

- Node.js 18.18+
- Codex CLI installed and authenticated:

```bash
npm install -g @openai/codex
codex login
```

## Development

```bash
npm test
```

## License

Apache-2.0. See `LICENSE` and `NOTICE`.
