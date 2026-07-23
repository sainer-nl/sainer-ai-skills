# Best practices for skill creators

> Source: https://agentskills.io/skill-creation/best-practices (snapshot 2026-07-23 — see [sources.md](sources.md))

How to write skills that are well-scoped and calibrated to the task.

## Start from real expertise

The common pitfall: asking an LLM to generate a skill from its general training
knowledge alone. The result is vague, generic procedure ("handle errors
appropriately", "follow best practices") instead of the specific API patterns, edge
cases, and conventions that make a skill valuable. Ground the skill in real,
domain-specific context:

**Extract from a hands-on task.** Complete a real task in conversation with an agent,
then extract the reusable pattern. Pay attention to:

- Steps that worked — the sequence that led to success
- Corrections you made ("use library X instead of Y", "check edge case Z")
- Input/output formats — what the data looked like going in and out
- Context you provided — project-specific facts the agent didn't already know

**Synthesize from existing artifacts.** Feed real material into the creation process:
internal docs, runbooks, style guides, API specs, schemas, config files, code review
comments, issue trackers, version-control history (patches reveal patterns), and
real failure cases with their resolutions. A skill synthesized from *your* incident
reports beats one synthesized from a generic best-practices article.

## Refine with real execution

The first draft usually needs refinement. Run the skill against real tasks and feed
**all** results — not just failures — back in. Ask: what triggered false positives?
What was missed? What could be cut? Even one execute-then-revise pass noticeably
improves quality; complex domains often need several.

Read the agent's **execution traces**, not just final outputs. If the agent wastes
time on unproductive steps, common causes are: instructions too vague (it tries
several approaches), instructions that don't apply to the current task (it follows
them anyway), or too many options without a clear default.

## Spending context wisely

Once activated, the full `SKILL.md` body competes for the agent's attention with
conversation history, system context, and other active skills.

**Add what the agent lacks, omit what it knows.** Focus on what the agent wouldn't
know without the skill: project conventions, domain procedures, non-obvious edge
cases, which tools/APIs to use. Don't explain what a PDF is or how HTTP works. For
each piece of content ask: "Would the agent get this wrong without this
instruction?" If no, cut it. If unsure, test it. If the agent already handles the
whole task well without the skill, the skill may not add value.

**Design coherent units.** Scope a skill like a function: one coherent unit of work
that composes with other skills. Too narrow forces multiple skills to load per task
(overhead, conflicting instructions); too broad is hard to activate precisely.
Querying a database and formatting results = one coherent unit; also covering
database administration = probably too much.

**Aim for moderate detail.** Overly comprehensive skills hurt — the agent struggles
to extract what's relevant and pursues paths triggered by instructions that don't
apply. Concise stepwise guidance with a working example outperforms exhaustive
documentation. If you're covering every edge case, consider whether most are better
left to the agent's judgment.

**Structure large skills with progressive disclosure.** Keep `SKILL.md` under 500
lines / 5,000 tokens — just the core instructions needed on every run. Move detail
to `references/`. Crucially, tell the agent **when** to load each file: "Read
`references/api-errors.md` if the API returns a non-200 status" beats a generic
"see references/ for details".

## Calibrating control

Match the specificity of instructions to the fragility of the task — calibrate each
part of the skill independently.

**Give freedom** when multiple approaches are valid. For flexible instructions,
explaining *why* beats rigid directives — an agent that understands the purpose
makes better context-dependent decisions.

**Be prescriptive** when operations are fragile, consistency matters, or an exact
sequence must be followed ("Run exactly this command. Do not modify it or add
flags.").

**Provide defaults, not menus.** Pick one default tool/approach and mention
alternatives briefly as escape hatches — don't present four options as equals:

```markdown
Use pdfplumber for text extraction. For scanned PDFs requiring OCR, use
pdf2image with pytesseract instead.
```

**Favor procedures over declarations.** Teach *how to approach* a class of problems,
not *what to produce* for one instance. "Read the schema, join on the `_id`
convention, apply the user's filters, aggregate and format" generalizes; "join
`orders` to `customers` and filter EMEA" answers exactly one question. Specific
details (output templates, "never output PII", tool-specific rules) are still fine —
the *approach* is what must generalize.

## Patterns for effective instructions

Reusable techniques — use the ones that fit; not every skill needs all of them.

### Gotchas sections

Often the highest-value content: environment-specific facts that defy reasonable
assumptions. Concrete corrections, not general advice:

```markdown
## Gotchas

- The `users` table uses soft deletes. Queries must include
  `WHERE deleted_at IS NULL` or results include deactivated accounts.
- The user ID is `user_id` in the database, `uid` in the auth service, and
  `accountId` in the billing API. All three are the same value.
- `/health` returns 200 even if the database is down. Use `/ready` for full
  service health.
```

Keep gotchas in `SKILL.md` where the agent reads them *before* hitting the
situation — in a reference file it may not recognize the trigger. When an agent
makes a mistake you have to correct, add the correction to the gotchas section;
it's the most direct way to improve a skill iteratively.

### Templates for output format

When output must follow a specific format, provide a template — agents
pattern-match against concrete structures more reliably than prose descriptions.
Short templates live inline in `SKILL.md`; longer or conditional ones go in
`assets/` and are referenced so they load only when needed.

### Checklists for multi-step workflows

An explicit checklist helps the agent track progress and avoid skipping steps,
especially with dependencies or validation gates:

```markdown
Progress:
- [ ] Step 1: Analyze the form (run `scripts/analyze_form.py`)
- [ ] Step 2: Create field mapping (edit `fields.json`)
- [ ] Step 3: Validate mapping (run `scripts/validate_fields.py`)
```

### Validation loops

Instruct the agent to validate its own work before moving on: do the work, run a
validator (script, reference checklist, or self-check), fix issues, repeat until it
passes. A reference document can serve as the "validator" — check work against it
before finalizing.

### Plan-validate-execute

For batch or destructive operations: produce an intermediate plan in a structured
format, validate it against a source of truth with a script, and only then execute.
The key ingredient is the validation step — errors like "Field 'signature_date' not
found — available fields: customer_name, order_total, signature_date_signed" give
the agent enough to self-correct.

### Bundling reusable scripts

If execution traces show the agent reinventing the same logic every run (building
charts, parsing a format, validating output), write a tested script once and bundle
it in `scripts/`. See [using-scripts.md](using-scripts.md).
