# Evaluating skill output quality

> Source: https://agentskills.io/skill-creation/evaluating-skills (snapshot 2026-07-23 — see [sources.md](sources.md))

A skill that "seemed to work" on one prompt isn't proven. Structured evals answer:
does it work reliably, across varied prompts, in edge cases, *better than no skill
at all* — and give a systematic improvement loop.

## Designing test cases

A test case = **prompt** (realistic user message) + **expected output**
(human-readable description of success) + optional **input files**. Store them in
`evals/evals.json` inside the skill directory:

```json
{
  "skill_name": "csv-analyzer",
  "evals": [
    {
      "id": 1,
      "prompt": "I have a CSV of monthly sales data in data/sales_2025.csv. Can you find the top 3 months by revenue and make a bar chart?",
      "expected_output": "A bar chart image showing the top 3 months by revenue, with labeled axes and values.",
      "files": ["evals/files/sales_2025.csv"]
    }
  ]
}
```

Tips: start with 2–3 cases (expand after the first results); vary phrasing,
detail, and formality; cover at least one edge case (malformed input, ambiguous
request); use realistic context (file paths, column names) — "process this data" is
too vague to test anything. Don't write pass/fail checks yet; assertions come after
you see the first outputs.

## Running evals

Run each test case **with the skill and without it** (or against the previous
version) — the baseline is what tells you the skill's value.

Workspace layout, one directory per iteration:

```
csv-analyzer/            # the skill: SKILL.md + evals/evals.json
csv-analyzer-workspace/
└── iteration-1/
    ├── eval-<case-name>/
    │   ├── with_skill/     # outputs/, timing.json, grading.json
    │   └── without_skill/  # outputs/, timing.json, grading.json
    └── benchmark.json      # aggregated statistics
```

**Each run needs a clean context** — no leftover state from previous runs or the
skill-development conversation. Subagents (as in Claude Code) give this isolation
naturally; otherwise use a separate session per run. Give each run the skill path
(or none for baseline), the prompt, input files, and an output directory. When
improving an existing skill, snapshot it first and use the snapshot as the baseline
(save to `old_skill/` instead of `without_skill/`).

**Capture timing** per run (`timing.json`: `total_tokens`, `duration_ms`) — a skill
that improves quality but triples token usage is a different trade-off than one
that's better *and* cheaper. In Claude Code, the subagent completion notification
includes these values; save them immediately, they aren't persisted elsewhere.

## Writing assertions

Assertions are verifiable statements about the output, added to each case in
`evals.json` *after* the first round of outputs.

Good: "The output file is valid JSON" (programmatic), "The bar chart has labeled
axes" (observable), "The report includes at least 3 recommendations" (countable).
Weak: "The output is good" (vague), "uses exactly the phrase 'Total Revenue: $X'"
(brittle). Qualities like writing style or visual polish resist pass/fail checks —
leave those to human review.

## Grading

Evaluate each assertion against the outputs, recording PASS/FAIL **with concrete
evidence** that quotes or references the output (`grading.json` with
`assertion_results` and a pass-rate summary). The simplest grader is an LLM given
the outputs and assertions; for mechanical checks (valid JSON, row counts, file
exists) use a verification script — more reliable and reusable.

Principles:

- **Require concrete evidence for a PASS.** A section titled "Summary" containing one vague sentence fails an "includes a summary" assertion — label without substance.
- **Review the assertions themselves** while grading: fix ones that always pass regardless of quality, always fail even on good output, or can't be verified from the output alone.
- For comparing two skill versions, try **blind comparison**: an LLM judge scores both outputs without knowing which version produced which — complements assertions by catching holistic quality differences.

## Aggregating and analyzing

Compute per-configuration stats into `benchmark.json`: pass rate, time, tokens for
`with_skill` vs `without_skill`, plus the `delta`. The delta is the verdict: +50
points pass rate for 13 extra seconds is worth it; +2 points for double the tokens
probably isn't. (Stddev only becomes meaningful with multiple runs per eval; early
on, focus on raw pass counts and the delta.)

Then look past the aggregates:

- **Remove assertions that always pass in both configurations** — the model handles them without the skill; they inflate the pass rate.
- **Investigate assertions that always fail in both** — broken assertion, impossible test, or checking the wrong thing.
- **Study assertions that pass with the skill but fail without** — this is where the skill adds value; understand which instructions made the difference.
- **Tighten instructions when results are inconsistent across runs** — high variance means a flaky eval or ambiguous instructions; add examples or specificity.
- **Check time/token outliers** — read the execution transcript of the slow run to find the bottleneck.

## Human review

Assertions only check what you thought to check. For each case, review actual
outputs alongside the grades and record *specific* feedback (e.g. `feedback.json`
per case): "chart is missing axis labels, months sorted alphabetically instead of
chronologically" is actionable; "looks bad" is not. Empty feedback = passed review.

## Iterating on the skill

Three signals: **failed assertions** (specific gaps), **human feedback** (broader
quality issues), **execution transcripts** (why things went wrong — ignored
instruction = probably ambiguous; wasted steps = instructions to simplify or cut).

Most effective: give all three plus the current `SKILL.md` to an LLM and ask for
proposed changes, with these guidelines:

- **Generalize from feedback** — fix underlying issues broadly, don't patch specific test cases.
- **Keep the skill lean** — fewer, better instructions; if transcripts show wasted work, remove the instructions causing it; if pass rates plateau as rules grow, try *removing* instructions.
- **Explain the why** — "Do X because Y causes Z" outperforms "ALWAYS X, NEVER Y".
- **Bundle repeated work** — if every run rewrites the same helper, move it into `scripts/`.

The loop: propose improvements → apply → rerun all cases in `iteration-N+1/` →
grade and aggregate → human review → repeat. Stop when satisfied, feedback is
consistently empty, or improvement stalls.

The `skill-creator` skill (https://github.com/anthropics/skills/tree/main/skills/skill-creator)
automates much of this workflow.
