---
name: writing-agent-skills
description: Guides creating, reviewing, and improving Agent Skills — SKILL.md files following the agentskills.io open standard (used by Claude Code, VS Code, Codex, Cursor, and others). Use when someone wants to write a new skill, review or refactor an existing SKILL.md, improve a skill description so it triggers reliably, bundle scripts into a skill, or set up evals for a skill — even if they just say "make this a skill" or "turn this workflow into a skill".
license: MIT
metadata:
  source: https://agentskills.io
  source-snapshot: "2026-07-23"
---

# Writing Agent Skills

You help someone create or improve an **Agent Skill**: a folder with a `SKILL.md`
(YAML frontmatter + Markdown instructions) that any compatible agent can discover
and load on demand. This skill condenses the official documentation at
**https://agentskills.io** — the canonical source. If something here may be
outdated or you need detail beyond it, fetch the source: every page is listed at
https://agentskills.io/llms.txt, and the full page map for this skill is in
[references/sources.md](references/sources.md).

Load references as needed — each maps to one topic:

- [references/specification.md](references/specification.md) — exact format rules: frontmatter fields, naming constraints, directory layout, validation. Load whenever you write or check frontmatter.
- [references/best-practices.md](references/best-practices.md) — writing the body: scoping, context economy, instruction patterns (gotchas, templates, checklists, validation loops). Load when drafting or reviewing body content.
- [references/optimizing-descriptions.md](references/optimizing-descriptions.md) — making the description trigger reliably, including the trigger-eval loop. Load when a skill isn't activating (or over-activating), or the user wants the description tuned.
- [references/evaluating-skills.md](references/evaluating-skills.md) — testing output quality with evals, assertions, grading, and iteration. Load when the user wants to test or systematically improve a skill.
- [references/using-scripts.md](references/using-scripts.md) — one-off commands, self-contained scripts, and designing script interfaces for agents. Load when the skill will bundle or invoke scripts.

## What a skill is (30 seconds)

```
skill-name/
├── SKILL.md          # Required: frontmatter (name, description) + instructions
├── scripts/          # Optional: executable code
├── references/       # Optional: docs loaded on demand
└── assets/           # Optional: templates, static resources
```

Agents use **progressive disclosure** in three stages: at startup they load only
each skill's `name` + `description` (~100 tokens); when a task matches the
description they load the full `SKILL.md` body; bundled files load only when the
instructions point to them. Every structural decision follows from this:
the description carries all triggering, the body must earn its tokens, and detail
belongs in references with explicit load conditions.

## Workflow

### 1. Decide whether it should be a skill

A good skill captures what the agent *wouldn't know on its own*: project
conventions, domain procedures, non-obvious gotchas, a specific tool sequence. If
a capable agent already handles the task well unaided, the skill adds cost, not
value. Scope it like a function — one coherent, composable unit of work (querying
a database and formatting results: one skill; also administering the database: too
much).

### 2. Ground it in real expertise

Never generate a skill from general knowledge alone — that produces vague filler
("handle errors appropriately"). Ask the user for source material: a task they
just completed with corrections along the way, runbooks, style guides, API
schemas, review comments, real failure cases. The corrections and gotchas are
usually the most valuable content. If the user has none of this, do the task
together first and extract the skill afterwards.

### 3. Scaffold and write the frontmatter

Create the folder and `SKILL.md`. Critical rules (full detail in
[references/specification.md](references/specification.md)):

- `name`: lowercase letters/numbers/hyphens, max 64 chars, no leading/trailing/consecutive hyphens, **must equal the folder name**.
- `description`: max 1024 chars — what the skill does **and** when to use it, phrased imperatively ("Use when..."), with the keywords real requests would contain. This field alone determines triggering, so write it with care ([references/optimizing-descriptions.md](references/optimizing-descriptions.md)).
- Optional: `license`, `compatibility` (only for real environment requirements), `metadata`, `allowed-tools` (experimental).

### 4. Write the body

Address the agent that will execute the skill ("you"), not the human reader. Keep
`SKILL.md` under 500 lines / ~5,000 tokens; move detail into `references/` and
state **when** to load each file ("Read references/api-errors.md if the API
returns a non-200 status" — not "see references/ for details"). Keep file
references one level deep.

For each part, apply the guidance in
[references/best-practices.md](references/best-practices.md):

- For every line ask: "Would the agent get this wrong without it?" If no, cut it.
- Match specificity to fragility — freedom plus a *why* where variation is fine; exact prescribed commands where operations are fragile.
- Provide one default per choice, alternatives only as brief escape hatches.
- Teach the generalizable procedure, not the answer to one instance.
- Use the patterns that fit: a gotchas section, output templates, progress checklists, validation loops, plan-validate-execute for destructive operations.

If the skill runs commands or bundles scripts, follow
[references/using-scripts.md](references/using-scripts.md): pin versions,
relative paths from the skill root, non-interactive, `--help`, structured output,
actionable errors.

### 5. Validate and iterate

- Check format: `skills-ref validate ./skill-name` (from https://github.com/agentskills/agentskills), or verify the frontmatter rules from the spec manually.
- Run the skill on a real task and read the **execution trace**, not just the output. Wandering usually means instructions that are vague, inapplicable, or option-heavy. Feed corrections back into the skill — new gotchas entries are the highest-leverage edit.
- When the user wants rigor, set up the eval loop from [references/evaluating-skills.md](references/evaluating-skills.md) (with-skill vs. without-skill baseline, assertions, grading) and the description trigger evals from [references/optimizing-descriptions.md](references/optimizing-descriptions.md).

## Gotchas

- The `name` not matching the folder name is the most common validation failure.
- A description that only says what the skill does ("Processes CSV files") without *when to use it* will under-trigger; agents match descriptions against user intent.
- Skills don't trigger on tasks the agent can trivially do alone — don't chase 100% trigger rates on simple prompts.
- Content in `references/` the agent is never told when to read is effectively invisible; every reference needs a load condition in `SKILL.md`.
- Gotchas belong in `SKILL.md` itself, not a reference file — the agent must read them *before* encountering the situation, and it may not recognize the trigger to go load them.
- More rules can make a skill worse: if quality plateaus while the skill grows, remove instructions and re-test.
- Agents may retry script invocations — make bundled scripts idempotent, and never let them wait for interactive input (they'll hang forever).

## Quality checklist before you output

- [ ] Frontmatter valid: `name` matches folder, description ≤ 1024 chars, imperative, covers what + when
- [ ] `SKILL.md` under 500 lines; details moved to `references/` with explicit load conditions
- [ ] Every instruction survives "would the agent get this wrong without it?"
- [ ] Defaults instead of option menus; procedures instead of one-off answers
- [ ] Fragile steps prescribed exactly; flexible steps explained with a why
- [ ] Gotchas from real experience captured in `SKILL.md`
- [ ] Scripts (if any): pinned versions, non-interactive, structured output, helpful errors
- [ ] Validated with `skills-ref validate` or against the spec rules
