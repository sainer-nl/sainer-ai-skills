# Agent Skills specification

> Source: https://agentskills.io/specification (snapshot 2026-07-23 — see [sources.md](sources.md))

## Directory structure

A skill is a directory containing, at minimum, a `SKILL.md` file:

```
skill-name/
├── SKILL.md          # Required: metadata + instructions
├── scripts/          # Optional: executable code
├── references/       # Optional: documentation
├── assets/           # Optional: templates, resources
└── ...               # Any additional files or directories
```

## SKILL.md format

`SKILL.md` must contain YAML frontmatter followed by Markdown content.

### Frontmatter fields

| Field           | Required | Constraints |
|-----------------|----------|-------------|
| `name`          | Yes      | Max 64 chars. Lowercase letters, numbers, hyphens only. No leading/trailing hyphen. |
| `description`   | Yes      | Max 1024 chars, non-empty. What the skill does **and** when to use it. |
| `license`       | No       | License name or reference to a bundled license file. Keep it short. |
| `compatibility` | No       | Max 500 chars. Environment requirements (intended product, system packages, network access). Most skills don't need it. |
| `metadata`      | No       | Arbitrary string-to-string map for extra properties (e.g. `author`, `version`). Use reasonably unique key names to avoid conflicts. |
| `allowed-tools` | No       | Space-separated string of pre-approved tools, e.g. `Bash(git:*) Bash(jq:*) Read`. Experimental — support varies between agents. |

Minimal example:

```markdown
---
name: skill-name
description: A description of what this skill does and when to use it.
---
```

With optional fields:

```markdown
---
name: pdf-processing
description: Extract PDF text, fill forms, merge files. Use when handling PDFs.
license: Apache-2.0
metadata:
  author: example-org
  version: "1.0"
---
```

### `name` rules

- 1–64 characters
- Only lowercase alphanumeric (`a-z`, `0-9`) and hyphens
- Must not start or end with a hyphen
- Must not contain consecutive hyphens (`--`)
- **Must match the parent directory name**

Valid: `pdf-processing`, `data-analysis`, `code-review`
Invalid: `PDF-Processing` (uppercase), `-pdf` (leading hyphen), `pdf--processing` (consecutive hyphens)

### `description` rules

- 1–1024 characters
- Describe both what the skill does and when to use it
- Include specific keywords that help agents identify relevant tasks

Good:

```yaml
description: Extracts text and tables from PDF files, fills PDF forms, and merges multiple PDFs. Use when working with PDF documents or when the user mentions PDFs, forms, or document extraction.
```

Poor:

```yaml
description: Helps with PDFs.
```

### Body content

The Markdown body after the frontmatter contains the skill instructions. There are
no format restrictions — write whatever helps agents perform the task effectively.
Recommended sections: step-by-step instructions, examples of inputs and outputs,
common edge cases.

The agent loads the **entire** body once it activates the skill — split longer
content into referenced files.

## Optional directories

- **`scripts/`** — executable code agents can run. Scripts should be self-contained or clearly document dependencies, include helpful error messages, and handle edge cases gracefully. Supported languages depend on the agent (Python, Bash, and JavaScript are common).
- **`references/`** — extra documentation loaded on demand (e.g. `REFERENCE.md`, domain-specific files). Keep individual files focused; smaller files mean less context use.
- **`assets/`** — static resources: templates, images, data files, schemas.

## Progressive disclosure

Agents load skills progressively — structure skills to exploit this:

1. **Metadata** (~100 tokens): `name` + `description` are loaded at startup for all skills.
2. **Instructions** (< 5000 tokens recommended): the full `SKILL.md` body loads when the skill activates.
3. **Resources** (as needed): files in `scripts/`, `references/`, `assets/` load only when required.

**Keep `SKILL.md` under 500 lines.** Move detailed reference material to separate files.

## File references

Reference other files with relative paths from the skill root:

```markdown
See [the reference guide](references/REFERENCE.md) for details.

Run the extraction script:
scripts/extract.py
```

Keep file references **one level deep** from `SKILL.md` — avoid nested reference chains.

## Validation

Validate with the `skills-ref` reference library
(https://github.com/agentskills/agentskills/tree/main/skills-ref):

```bash
skills-ref validate ./my-skill
```

This checks that the frontmatter is valid and all naming conventions are followed.
