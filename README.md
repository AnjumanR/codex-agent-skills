# Codex Agent Skills

Use Codex from inside your agent harness for code reviews or to delegate tasks to Codex.

These skills are for users who want an easy way to start using Codex from the workflow they already have.

This repository packages Codex workflows as installable skills via the generic `skills` CLI.

## What You Get

- `codex-review` for a normal read-only Codex review
- `codex-adversarial-review` for a steerable challenge review
- `codex-rescue` and `codex-job-control` to delegate work and manage background jobs
- helper skills for prompt quality and result handling (`gpt-5-4-prompting`, `codex-result-handling`)

## Requirements

- **ChatGPT subscription (including Free) or OpenAI API key**
  - Usage contributes to your Codex usage limits. [Learn more](https://developers.openai.com/codex/pricing)
- **Node.js 18.18 or later**
- **Codex CLI installed and authenticated**

```bash
npm install -g @openai/codex
codex login
```

## Install

Install from GitHub:

```bash
npx -y skills add AnjumanR/codex-agent-skills
```

List skills before install:

```bash
npx -y skills add AnjumanR/codex-agent-skills --list
```

Install specific skills only:

```bash
npx -y skills add AnjumanR/codex-agent-skills --skill codex-review codex-adversarial-review
```

## Runtime Entry Point

Operational skills bundle a self-contained runtime at:

```bash
runtime/scripts/codex-companion.mjs
```

When working from this repository directly, you can run the root runtime:

```bash
node scripts/codex-companion.mjs <subcommand> [...args]
```

## First Run

From this repository root:

```bash
node scripts/codex-companion.mjs setup
node scripts/codex-companion.mjs review
```

For background-capable delegated work:

```bash
node scripts/codex-companion.mjs task --background investigate the regression
node scripts/codex-companion.mjs status
node scripts/codex-companion.mjs result
```

In harnesses that expose background orchestration for review commands, a typical first pass is:

```bash
review --background
status
result
```

## Usage

### `review`

Runs a normal Codex review on your current work.

> Note:
> Review of multi-file changes may take a while. For larger diffs, background orchestration is generally recommended when your harness supports it.

Use it when you want:

- a review of current uncommitted changes
- a review of your branch compared to a base branch like `main`

Examples:

```bash
node scripts/codex-companion.mjs review
node scripts/codex-companion.mjs review --base main
```

`--base <ref>` is supported for branch review. `--wait` and `--background` are part of host integration flows; direct CLI execution is foreground.

This command is read-only and does not perform edits.

### `adversarial-review`

Runs a steerable review that questions implementation approach and design choices.

Use it when you want:

- pressure testing before shipping
- review focused on hidden assumptions and tradeoffs
- challenge review around auth, data loss, rollback, race conditions, or reliability

Examples:

```bash
node scripts/codex-companion.mjs adversarial-review
node scripts/codex-companion.mjs adversarial-review --base main challenge whether this was the right caching and retry design
node scripts/codex-companion.mjs adversarial-review look for race conditions and question the chosen approach
```

It uses the same review target selection as `review`, including `--base <ref>` for branch review.

This command is read-only. It does not fix code.

### `task` (rescue)

Hands a task to Codex for diagnosis, implementation, or follow-up.

Use it when you want Codex to:

- investigate a bug
- try a fix
- continue a previous Codex task
- take a faster or cheaper pass with a smaller model

Examples:

```bash
node scripts/codex-companion.mjs task --write investigate why the tests started failing
node scripts/codex-companion.mjs task --write fix the failing test with the smallest safe patch
node scripts/codex-companion.mjs task --resume-last apply the top fix from the last run
node scripts/codex-companion.mjs task --model gpt-5.4-mini --effort medium investigate the flaky integration test
node scripts/codex-companion.mjs task --model spark fix the issue quickly
node scripts/codex-companion.mjs task --background investigate the regression
```

You can also delegate in natural language from your harness, for example:

```text
Ask Codex to redesign the database connection to be more resilient.
```

Notes:

- if you do not pass `--model` or `--effort`, Codex uses its defaults
- if you pass `spark`, runtime maps it to `gpt-5.3-codex-spark`
- follow-up rescue requests can continue the latest task with `--resume-last`

### `status`

Shows running and recent Codex jobs for the current repository.

Examples:

```bash
node scripts/codex-companion.mjs status
node scripts/codex-companion.mjs status task-abc123
```

Use it to:

- check progress on background work
- see the latest completed job
- confirm whether a task is still running

### `result`

Shows final stored Codex output for a finished job.
When available, it includes the Codex session ID so you can reopen it with `codex resume <session-id>`.

Examples:

```bash
node scripts/codex-companion.mjs result
node scripts/codex-companion.mjs result task-abc123
```

### `cancel`

Cancels an active background Codex job.

Examples:

```bash
node scripts/codex-companion.mjs cancel
node scripts/codex-companion.mjs cancel task-abc123
```

### `setup`

Checks whether Codex is installed and authenticated.

Examples:

```bash
node scripts/codex-companion.mjs setup
node scripts/codex-companion.mjs setup --enable-review-gate
node scripts/codex-companion.mjs setup --disable-review-gate
```

## Typical Flows

### Review Before Shipping

```bash
node scripts/codex-companion.mjs review
```

### Hand A Problem To Codex

```bash
node scripts/codex-companion.mjs task --write investigate why the build is failing in CI
```

### Start Something Long Running

```bash
node scripts/codex-companion.mjs adversarial-review look for race conditions and rollback hazards
node scripts/codex-companion.mjs task --background investigate flaky integration tests
```

Then check in with:

```bash
node scripts/codex-companion.mjs status
node scripts/codex-companion.mjs result
```

## Codex Integration

This runtime wraps the [Codex app server](https://developers.openai.com/codex/app-server). It uses the global `codex` binary in your environment and applies the same Codex configuration.

### Common Configurations

To change default model or reasoning effort, define them in `config.toml`. For example:

```toml
model = "gpt-5.4-mini"
model_reasoning_effort = "xhigh"
```

Configuration is picked up from:

- user-level config: `~/.codex/config.toml`
- project-level overrides: `.codex/config.toml`

See Codex docs for more options:

- [Basic config](https://developers.openai.com/codex/config-basic)
- [Advanced config](https://developers.openai.com/codex/config-advanced)
- [Config reference](https://developers.openai.com/codex/config-reference)

### Moving Work Over To Codex

When a command returns a Codex session ID, you can continue directly in Codex:

```bash
codex resume <session-id>
```

## License

Apache-2.0. See `LICENSE` and `NOTICE`.
