# Sources

This skill is built from the official Agent Skills documentation at
**https://agentskills.io** — the canonical, always-current source. Everything in this
skill's `SKILL.md` and `references/` is a condensed snapshot of those pages. When the
site and this skill disagree, the site wins.

**Snapshot date: 2026-07-23.**

## Page map

The site publishes a machine-readable index of every page at
**https://agentskills.io/llms.txt** — fetch that first to discover pages that may have
been added since this snapshot. Each page is also available as raw markdown by
appending `.md` to its URL.

| Source page | Local file |
|---|---|
| https://agentskills.io/specification | [specification.md](specification.md) |
| https://agentskills.io/skill-creation/quickstart | folded into `SKILL.md` |
| https://agentskills.io/skill-creation/best-practices | [best-practices.md](best-practices.md) |
| https://agentskills.io/skill-creation/optimizing-descriptions | [optimizing-descriptions.md](optimizing-descriptions.md) |
| https://agentskills.io/skill-creation/evaluating-skills | [evaluating-skills.md](evaluating-skills.md) |
| https://agentskills.io/skill-creation/using-scripts | [using-scripts.md](using-scripts.md) |
| https://agentskills.io/home | background only (what Agent Skills are) |
| https://agentskills.io/clients | not included — a showcase of compatible clients |
| https://agentskills.io/client-implementation/adding-skills-support | not included — for people building agent runtimes, out of scope for skill authors |

Other official material:

- **Example skills:** https://github.com/anthropics/skills — real-world skills to learn from, including `skill-creator`, which automates the description-optimization and eval loops.
- **Standard development:** https://github.com/agentskills/agentskills — the spec repo, including the `skills-ref` validation library.

## How to refresh this skill

1. Fetch https://agentskills.io/llms.txt and note any new or removed pages.
2. For each page in the map above, fetch `<url>.md` and compare against the local file.
3. Update the local reference files to match — keep them condensed, but never contradict the source.
4. Reflect any changes to core rules (frontmatter fields, limits, naming) in `SKILL.md` itself.
5. Update the snapshot date at the top of this file.
