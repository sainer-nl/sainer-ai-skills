# Transfers — destination descriptions and routing guidance

Read this when the operator can connect callers to a person or a department.

**Who reads this text matters more than anything else here.** Destination
descriptions and routing guidance are read **only by the routing model** that
picks the destination. The voice model on the call never sees them — it sees
destination names only, and relays whatever the routing model asks. So write
both as *routing decision criteria*, not as call behaviour and never as
something to say out loud.

---

## Destination description

The one field that decides whether a caller reaches the right person. It answers
a single question: **when should a caller be routed here?**

Two shapes work:

- **By topic** — "Use for billing questions: invoices, payments, refunds, debtors."
- **By name only** — "Only when the caller asks for this person by name."

Rules:

- **Make each destination's scope distinct.** Overlapping descriptions are the
  most common cause of misrouting. If two destinations could both match the same
  call, say explicitly which wins and when.
- **Enumerate, don't gesture.** "Administrative matters" is weak; "an invoice,
  a payment, a reminder, a reimbursement or insurance question, or a change of
  contact details" is what actually routes correctly.
- **Say when NOT to route here.** A description that only says when to route
  here leaves the router guessing on near-misses. "Do not route here merely
  because the caller asked for 'someone' — route on the subject."
- **No scripts.** Never write what the operator should say. This text cannot
  reach the caller.

## Pre-transfer notice

A short heads-up spoken to the caller just before the transfer fires — "have
your reference number ready". Write it in English **as an instruction of
intent**, not as a literal sentence: the operator renders it in the caller's own
language. Leave it empty where it would be inappropriate, such as an urgent-care
line.

## Spoken languages

If a destination's staff speak only some of the operator's languages, set them.
A caller speaking outside that list gets a warning before the transfer fires,
instead of being handed to someone who cannot help them.

## Routing guidance

Cross-cutting rules that span destinations. Everything specific to one
destination belongs in *its* description; routing guidance carries only what no
single destination can express.

It **can** set:

- tiebreakers when a call matches more than one destination
- urgency or priority overrides between destinations
- what the router must never ask the caller just to route (a postcode, an
  account number, a customer ID)
- how to frame a stand-in when a destination is closed
- how the router should word a confirmation or a clarifying question

It **cannot**: invent destinations, override a destination's description, or
invent a closure the descriptions do not state.

**Never put voice-model behaviour here.** Whether to transfer at all, answering
from the knowledge base, and running a data-collection flow are the voice
model's job and belong in the Instructions. Guidance that says "always answer
from the knowledge base first" does nothing, because the model that reads it
cannot answer anything.

## A fallback destination is worth having

The destination a caller reaches when they need a person but nothing more
specific matches — and when they are frustrated and want out of the automated
system. Without one, those calls have nowhere to go.
