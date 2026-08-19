<div align="center">

# Sainer AI Skills

**Public [Agent Skills](https://agentskills.io) for building and tuning [Sainer](https://sainer.nl) AI phone operators.**

[![Skills](https://img.shields.io/badge/skills-2-2563eb)](#skills)
[![Standard](https://img.shields.io/badge/agentskills.io-compatible-22c55e)](https://agentskills.io)
[![Website](https://img.shields.io/badge/sainer.nl-website-0ea5e9)](https://sainer.nl)
[![Docs](https://img.shields.io/badge/docs.sainer.nl-docs-8b5cf6)](https://docs.sainer.nl)
[![License](https://img.shields.io/badge/license-MIT-blue)](./LICENSE)

</div>

---

## What is this?

[Sainer](https://sainer.nl) is an AI phone operator that answers your calls 24/7 — it
handles questions, connects callers to the right place, captures details, and books
appointments, in natural conversation.

This repository is a small, curated set of **public Agent Skills** that help you work with
Sainer. They follow the [agentskills.io](https://agentskills.io) standard, so they run in
any agent runtime that supports it (Claude Code, and others). Each skill is self-contained
and carries its own instructions and reference material.

## Skills

| Skill | What it does |
|-------|--------------|
| [`sainer-operator-instructions`](./sainer-operator-instructions) | Interviews you about your business and builds your Sainer phone operator — the **Instructions** and persona, plus the steering text its transfers, data collection, custom actions, messages and vocabulary need. |
| [`writing-agent-skills`](./writing-agent-skills) | Teaches an agent how to **write Agent Skills themselves** — scaffolding a `SKILL.md`, writing descriptions that trigger reliably, bundling scripts, and running evals. A condensed, source-referenced snapshot of the [agentskills.io](https://agentskills.io) documentation. |

## Install

These skills are installed with the [`skills`](https://agentskills.io) CLI. Run it with
`npx` — no global install required.

```bash
# Add every skill in this repo to your project
npx skills add sainer-nl/sainer-ai-skills

# Or pick a single skill
npx skills add sainer-nl/sainer-ai-skills -s sainer-operator-instructions
```

Other handy commands:

```bash
npx skills list                       # show installed skills
npx skills update                     # pull the latest versions
npx skills add sainer-nl/sainer-ai-skills -l   # list what's in this repo (no install)
```

### Use without installing

To generate a one-off prompt for a skill without adding it to your project:

```bash
npx skills use sainer-nl/sainer-ai-skills@sainer-operator-instructions
```

### Manual

You can also just copy a skill's folder into your agent's skills directory
(for Claude Code that's `.claude/skills/`). Each folder is everything the skill needs.

## About Sainer

Sainer answers your phone so you don't have to — 24/7, in natural conversation, in the
caller's own language. Learn more at **[sainer.nl](https://sainer.nl)**, and find the full
product documentation — every feature and tool, where each lives in the app, and how to set
it up — at **[docs.sainer.nl](https://docs.sainer.nl)**.

## Contributing

Issues and pull requests are welcome. Each skill lives in its own top-level folder with a
`SKILL.md` (and optional `references/`). Keep skills self-contained and free of any
non-public, customer-specific, or internal implementation detail.

## License

[MIT](./LICENSE) © 2026 Sainer. Use, copy, and adapt these skills freely; just keep the
copyright and permission notice. The license covers the skill content only — it grants no
rights to the "Sainer" name or brand.
