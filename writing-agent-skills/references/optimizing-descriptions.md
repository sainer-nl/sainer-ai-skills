# Optimizing skill descriptions

> Source: https://agentskills.io/skill-creation/optimizing-descriptions (snapshot 2026-07-23 — see [sources.md](sources.md))

A skill only helps if it gets activated. The `description` field is the primary
mechanism agents use to decide whether to load a skill — it carries the entire
burden of triggering. Under-specified means it won't trigger when it should;
over-broad means it triggers when it shouldn't.

One important nuance: agents typically only consult skills for tasks beyond what
they can handle alone. A simple "read this PDF" may not trigger a PDF skill even
with a perfect description. Descriptions make the difference on tasks involving
specialized knowledge — unfamiliar APIs, domain workflows, uncommon formats.

## Writing effective descriptions

- **Use imperative phrasing.** "Use this skill when..." rather than "This skill does...". The agent is deciding whether to act — tell it when to act.
- **Focus on user intent, not implementation.** Describe what the user is trying to achieve; the agent matches against what the user asked for.
- **Err on the side of being pushy.** Explicitly list contexts where the skill applies, including when the user doesn't name the domain: "even if they don't explicitly mention 'CSV' or 'analysis'."
- **Keep it concise.** A few sentences to a short paragraph; hard limit 1024 characters.

Before/after example:

```yaml
# Before
description: Process CSV files.

# After
description: >
  Analyze CSV and tabular data files — compute summary statistics,
  add derived columns, generate charts, and clean messy data. Use this
  skill when the user has a CSV, TSV, or Excel file and wants to
  explore, transform, or visualize the data, even if they don't
  explicitly mention "CSV" or "analysis."
```

More specific about what it does; broader about when it applies.

## Designing trigger eval queries

Build ~20 realistic user prompts labeled with whether they should trigger the skill
(8–10 should-trigger, 8–10 should-not):

```json
[
  { "query": "I've got a spreadsheet in ~/data/q4_results.xlsx with revenue in col C — can you add a profit margin column?", "should_trigger": true },
  { "query": "whats the quickest way to convert this json file to yaml", "should_trigger": false }
]
```

**Should-trigger queries** — vary along several axes: phrasing (formal, casual,
typos), explicitness (names the domain vs. describes the need without naming it),
detail (terse vs. context-heavy), and complexity (single-step vs. buried inside a
larger multi-step chain). The most useful ones are where the skill would help but
the connection isn't obvious from the query — that's where wording matters.

**Should-not-trigger queries** — the valuable ones are **near-misses** that share
keywords but need something different. "Write a fibonacci function" tests nothing;
"write a python script that reads a csv and uploads rows to postgres" is a strong
negative for a CSV *analysis* skill (CSV keyword, but the task is ETL).

**Realism** — include file paths, personal context ("my manager asked..."),
specific details (column names), casual language and occasional typos.

## Testing whether a description triggers

Run each query through the agent with the skill installed and observe whether the
skill's `SKILL.md` was loaded (use the client's logs / tool-call history / verbose
output). Model behavior is nondeterministic — run each query multiple times (3 is a
reasonable start) and compute a **trigger rate**. A should-trigger query passes if
its rate is above a threshold (0.5 default); a should-not-trigger passes if below.

In Claude Code you can detect invocation from JSON output, e.g.:

```bash
claude -p "$query" --output-format json \
  | jq -e --arg skill "$SKILL_NAME" \
    'any(.messages[].content[]; .type == "tool_use" and .name == "Skill" and .input.skill == $skill)'
```

Loop that over the query file, count triggers per query, and emit trigger rates.

## Avoiding overfitting: train/validation split

Optimizing against all queries risks a description that fits those phrasings but
fails on new ones. Split: **train ~60%** (guides changes), **validation ~40%**
(checks generalization). Both sets need a proportional mix of positive/negative
queries; shuffle once and keep the split fixed across iterations.

## The optimization loop

1. **Evaluate** the current description on both sets.
2. **Identify failures in the train set only** — keep validation results out of the revision process.
3. **Revise the description:**
   - Should-trigger failing → too narrow: broaden scope, add context about when it's useful.
   - Should-not-trigger false-firing → too broad: state what the skill does *not* do, clarify the boundary with adjacent capabilities.
   - Don't paste keywords from failed queries (overfitting) — address the general category they represent.
   - Stuck after several iterations → try a structurally different framing, not incremental tweaks.
   - Watch the 1024-character limit; descriptions grow during optimization.
4. **Repeat** until train passes or improvement stalls. ~5 iterations is usually enough; if nothing improves, suspect the queries (too easy/hard/mislabeled), not the description.
5. **Select the best iteration by validation pass rate** — it may not be the last one; later iterations can overfit.

## Applying the result

1. Update `description` in the frontmatter; verify it's under 1024 characters.
2. Sanity-check manually with a few prompts.
3. For rigor: write 5–10 *fresh* queries never used in optimization and run the eval — an honest check on generalization.

The `skill-creator` skill (https://github.com/anthropics/skills/tree/main/skills/skill-creator)
automates this loop end-to-end: splits the eval set, evaluates trigger rates in
parallel, proposes improvements, and generates a live HTML report.
