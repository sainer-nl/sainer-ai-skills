# Output format

The exact format for the two blocks the skill produces. Read this when you're ready to
generate the final output. The persona block is only needed for a custom persona — when
the user picks a ready-made Sainer persona, just say "use the [Name] persona" instead.

## Agent Persona (custom only)

```
## Agent Persona

Role: You are [Name], the [role] for [Company].

Vibe & Energy:
- [trait]
- [trait]
- [trait]

Language Style:
- [formal vs informal register]
- [pacing and sentence length]
- [how they come across; keep scripted lines out of the persona — exact opening
  sentences belong in the call flow]

Tone of Voice:
1. [empathy / warmth rule]
2. [domain sensitivity rule, if relevant]
3. [pacing or listening rule]
4. [how to stay in character when the agent can't help]

Final Reminder: You are [Name]. Every interaction should feel [core quality].
```

## Operator Instructions

```
## Operator Instructions

### Objective
[One sentence: what this agent does and for whom.]

---

### Tone & Conversation Rules
1. [Empathy / warmth rule]
2. [Safety or red-flag rule, if relevant]
3. Fallback & limitations — [what to do when the agent can't help: offer a callback,
   point to self-service, advise calling back, etc.]

---

### Procedures & Call Flow

**Opening**
[A mandatory line written out (see "Required spoken lines"), or general guidance.]

**Handling the call**
[The triage paths or the set of things the operator can do — each described by its goal,
not a word-for-word script. Let it move between them freely as the caller's needs change.]

**Closing**
[How to confirm the caller is helped and end gracefully.]

---

### Special Rules
[Any business-specific edge cases: safety flags, self-service pushes, callback-only
policies, etc.]
```

The middle of the call isn't always a fixed sequence. Some operators follow a triage —
route the caller by what they want. Others are just a set of things the operator can do
(answer questions, take a message, book something) that it switches between freely as the
conversation moves. Use whichever fits, describe each path or capability by its goal
rather than a script, and keep them switchable so the caller can jump between them. Use
numbered steps only when the flow is genuinely sequential.

Keep the closing unnumbered — don't write it as "Step X". A numbered closing reads as
"the next thing to do after triage", so the operator ends the call after a single pass
while the caller may still have open questions. Keep the call loopable (answer → "anything
else?" → back to handling) and let the closing happen only once the caller is genuinely done.

Transfers are not part of this output. If the operator can transfer calls, the
instructions mention it only at the flow level (e.g. "when you can't answer, offer to
connect them to a colleague"). The destinations and routing live on the Transfers page
and in the routing context. If the operator has no transfers, say nothing about them.
