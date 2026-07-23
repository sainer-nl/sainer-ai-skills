# Using scripts in skills

> Source: https://agentskills.io/skill-creation/using-scripts (snapshot 2026-07-23 — see [sources.md](sources.md))

Skills can instruct agents to run shell commands and bundle reusable scripts in
`scripts/`.

## One-off commands

When an existing package already does what you need, reference it directly in
`SKILL.md` — no `scripts/` directory needed. Runners that auto-resolve dependencies:

| Runner | Ecosystem | Example | Notes |
|---|---|---|---|
| `uvx` | Python | `uvx ruff@0.8.0 check .` | Ships with uv (separate install); fast, caches aggressively |
| `pipx run` | Python | `pipx run 'black==24.10.0' .` | Mature alternative; broad OS package-manager availability |
| `npx` | Node | `npx eslint@9 --fix .` | Bundled with npm/Node; downloads on demand and caches |
| `bunx` | Bun | `bunx eslint@9 --fix .` | Only when the environment has Bun rather than Node |
| `deno run` | Deno | `deno run npm:eslint@9 -- --fix .` | Needs permission flags (`--allow-read` etc.); `--` separates Deno flags from tool flags |
| `go run` | Go | `go run golang.org/x/tools/cmd/goimports@v0.28.0 .` | Built into Go |

Tips:

- **Pin versions** (`npx eslint@9.0.0`) so behavior is stable over time.
- **State prerequisites** in `SKILL.md` ("Requires Node.js 18+"); for runtime-level requirements use the `compatibility` frontmatter field.
- **Move complex commands into scripts** — a one-off command suits a tool with a few flags; when it's hard to get right first try, a tested script is more reliable.

## Referencing scripts from SKILL.md

Use relative paths from the **skill directory root** (the agent runs commands from
there; the same convention applies inside `references/*.md`). List available
scripts so the agent knows they exist, then give the invocation:

````markdown
## Available scripts

- **`scripts/validate.sh`** — Validates configuration files
- **`scripts/process.py`** — Processes input data

## Workflow

1. Run the validation script:
   ```bash
   bash scripts/validate.sh "$INPUT_FILE"
   ```
````

## Self-contained scripts

Bundle scripts that declare dependencies **inline**, so one command runs them with
no manifest or install step:

- **Python — PEP 723** inline metadata, run with `uv run scripts/extract.py` (pipx also supports it):

  ```python
  # /// script
  # dependencies = [
  #   "beautifulsoup4",
  # ]
  # ///
  ```

  Pin with PEP 508 specifiers (`"beautifulsoup4>=4.12,<5"`); `requires-python` constrains the Python version; `uv lock --script` gives full reproducibility.

- **Deno** — `npm:`/`jsr:` import specifiers make every script self-contained: `import * as cheerio from "npm:cheerio@1.0.0"`. Packages with native addons may not work.
- **Bun** — auto-installs missing packages when no `node_modules` exists; pin in the import path (`import ... from "cheerio@1.0.0"`). A `node_modules` anywhere up the tree disables auto-install.
- **Ruby** — `require 'bundler/inline'` + a `gemfile do ... end` block. Pin explicitly (`gem 'nokogiri', '~> 1.16'`) — there's no lockfile.

## Designing scripts for agentic use

The agent reads stdout/stderr to decide its next step — design for that:

- **Never prompt interactively.** Hard requirement: agents run in non-interactive shells; a TTY prompt hangs forever. Take all input via flags, env vars, or stdin, and error clearly when something's missing: `Error: --env is required. Options: development, staging, production.`
- **Document usage with `--help`.** It's how the agent learns the interface: brief description, flags, usage examples. Keep it concise — it enters the context window.
- **Write helpful error messages.** Say what went wrong, what was expected, and what to try (`Error: --format must be one of: json, csv, table. Received: "xml"`). An opaque error wastes a turn.
- **Use structured output.** Prefer JSON/CSV/TSV over free-form or whitespace-aligned text — parseable by the agent and by `jq`/`cut`/`awk`. Send data to stdout, diagnostics/progress to stderr.

Further considerations:

- **Idempotency** — agents retry; "create if not exists" beats "fail on duplicate".
- **Input constraints** — reject ambiguous input with a clear error rather than guessing; use enums/closed sets.
- **Dry-run support** — `--dry-run` for destructive or stateful operations.
- **Meaningful exit codes** — distinct codes per failure type, documented in `--help`.
- **Safe defaults** — require `--confirm`/`--force` for destructive operations, matched to the risk.
- **Predictable output size** — harnesses truncate large tool output (often 10–30K chars). Default to a summary or limit, support `--offset` for more, or require `--output <file|->` when output is large and unpageable.
