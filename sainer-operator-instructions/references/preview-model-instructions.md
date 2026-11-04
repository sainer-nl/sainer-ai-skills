# Instructions for the preview model

Read this before you write, rewrite or improve the Instructions of an operator on the
preview model, and when you improve them after moving an operator to the preview model.
Everything in the rest of this guide still applies; this file only covers what differs.

---

## Why the Instructions change too

The goal is Instructions that work better for this operator, not Instructions that are
shorter. The preview model follows instructions far more literally and reliably than the
base model, so two things change. What only existed to make the base model comply (the
same rule repeated in three sections, NEVER and ALWAYS on every line, defensive
restatements) adds nothing and can go. And a rule that stops halfway gets followed
halfway, so some rules need to be more precise, which can make them longer. Usually the
result is shorter overall; that is a side effect, never the target.

These are guidelines, not a checklist to enforce. Operators differ: a strict intake for a
clinic needs different Instructions from a relaxed reception desk. Apply what improves
this operator, leave alone what already works, and when in doubt keep the customer's
rule.

## How to write them

- **Say each rule once.** Remove repetitions, restatements and emphasis that only existed
  to make the base model listen. Keep one rule in the one place it belongs.
- **Write the next step into the rule.** The preview model does what the rule says and
  then stops. "Blood draw is not offered at this location" produces a bare no. Write what
  comes after it: "When something is not offered at the location they asked about, name
  in that same answer the locations that do offer it."
- **Make a required check a precondition, not a script.** When something must be known
  before the operator acts, say what must be known and before which action: "Before you
  note an image request, know whether it is for the caller themselves, also when it
  sounds obvious." Prefer this over a fixed sequence of steps: a caller who answers early,
  combines two answers or skips one breaks a script, and the operator then re-asks or
  forces the order. A precondition holds however the conversation goes. Keep a fixed
  order only where the business really needs one.
- **Every promise needs a tool behind it.** Taking a message, arranging a callback,
  sending something, booking: only write it when the operator has the tool or setting
  that does it. Ask the user which tools are switched on if you do not know. A promise
  without one is a call that ends with the caller believing something will happen that
  never does. Remove it, and say what the operator does instead (usually connecting them
  to a colleague). Do not add lists of what the operator cannot do: it only has the tools
  it has, and a list of limits nobody asked about makes it defensive and goes stale the
  moment a tool is added.
- **Leave out judgements that need knowledge it lacks.** It has no map and no local
  knowledge. Where a caller could ask which location is closest, which is quickest, or
  similar, say it should not judge that unless the knowledge says so.

## What not to write

These are handled by Sainer on the preview model. Writing them in the Instructions adds a
second, competing rule.

- **No closing check-ins.** No "after every answer, ask whether there is anything else".
  The operator already keeps the conversation going and varies how it checks in.
- **No hold phrases or bridge lines.** The operator bridges a wait in its own words. When
  a customer wants the transfer wait bridged in a specific way, use the hold phrase
  setting on the Transfers page (see [transfers.md](transfers.md)), never the
  Instructions.
- **No register or tone.** Formal or informal address (u or je, Sie or du) and how the
  operator comes across live in the persona. Repeating them in the Instructions makes two
  places that drift apart.
- **No fallback after a declined transfer.** When the caller declines a transfer, the
  call simply carries on; do not make the operator push a message or callback instead.

## Leave the routing alone

Transfer destination descriptions and the routing guidance are read by Sainer's routing
model, not by the preview model. They do not get slimmed: a shortened version made the
routing less sure of itself, so it asked callers to confirm more often and made wrong
decisions. When moving an operator, leave them as they are. Only fix what is actually
wrong, such as a description or guidance that refers to a destination that is switched
off.

## Improving existing Instructions

When an operator has just moved to the preview model, improving its Instructions is an
optional follow-up, never part of the move itself.

- Ask once whether the user wants it, and say in one or two sentences what you would
  change (repetitions out, missing next steps in, promises without a tool behind them
  removed).
- **On an operator that takes real calls, suggest a copy first.** The copy takes no real
  calls, so they can test the result as often as they like and bring it back once they
  are happy. It is advice, not a gate.
- **One kind of change at a time.** For example first the check-ins and repetitions,
  then the missing next steps, then the promises without a tool behind them, each for the
  user to accept before the next. Never one wholesale rewrite.
- **Keep every business rule and fact.** Remove only what the base model needed to
  comply: repetitions, emphasis and rules Sainer already handles. A rule about the
  business is never repetition, even when it sounds generic: what may be promised to
  callers (no response times, no prices), privacy, what to do in an emergency, who
  handles what. If you are unsure whether a line is a business rule, keep it.

## Before you finish

- [ ] The changes make this operator better, not just shorter; every business rule and
      fact kept
- [ ] Each rule stated once, without emphasis words that were only there for the base
      model
- [ ] Every "no" or "not offered" rule says what to offer instead
- [ ] Required checks written as preconditions, a fixed order only where the business
      needs one
- [ ] Every promise (a message, a callback, a text, a booking) has a tool or setting
      behind it; no lists of limits
- [ ] No closing check-ins, hold phrases, bridge lines, register or tone
- [ ] A declined transfer carries on the conversation, with no pushed fallback
- [ ] Transfer descriptions and routing guidance untouched, apart from real errors
