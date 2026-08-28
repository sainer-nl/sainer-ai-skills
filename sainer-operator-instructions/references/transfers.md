# Transfers — destination descriptions and routing guidance

Read this when the operator can connect callers to a person or a department.

**Who reads this text matters more than anything else here.** Destination
descriptions and routing guidance are read **only by the routing model** that
picks the destination. The voice model on the call never sees them — it sees
destination names only, and relays whatever the routing model asks. So write
both as *routing decision criteria*, not as call behaviour and never as
something to say out loud.

---

## What to ask before you write anything

You cannot write a destination description from the destination's name. "Sales"
tells you nothing about which calls belong there. Ask:

1. **Who actually sits behind each destination, and what do they handle?** Push
   for the list, not the label. "Sales" is a label; "quotes, availability, lease
   questions and anything from a customer who does not have an account yet" is
   what routes.
2. **Where do the destinations overlap?** Name the ambiguous call out loud — "a
   customer with an existing order who wants to change it, does that go to Sales
   or Support?" — and get a ruling. Overlap you did not resolve becomes
   misrouting you will hear about later.
3. **What should never be asked just to route?** Some businesses expect a
   customer number up front; most consider it hostile.
4. **What happens to a caller nothing matches?** This gets you the fallback.
5. **Does any destination have limited hours or limited languages?**

Ask two or three of these at a time, not all five.

---

## Destination description

The one field that decides whether a caller reaches the right person. It answers
a single question: **when should a caller be routed here?**

Two shapes work:

- **By topic** — "Use for billing questions: invoices, payments, refunds, debtors."
- **By name only** — "Only when the caller asks for this person by name."

Aim for two to five sentences. One sentence is almost always too vague to
separate two destinations; a paragraph usually means call behaviour has crept in.

### Worked example

A garage with a Workshop destination and a Parts Counter destination. First
attempt, and the reason it misroutes:

> Use for technical matters and repairs.

Every complaint about a car is "technical". The parts counter also handles
technical questions. A caller asking whether a specific brake disc is in stock
matches this description just as well as one whose car will not start, so the
router picks unpredictably. It also never says what to do with a near-miss.

Rewritten:

> Use for anything to do with a vehicle that is being worked on or needs to be:
> booking a service or MOT, a breakdown, a warning light, a noise or fault the
> caller is describing, the status of a car already in the workshop, and
> collection times.
>
> Do not route here for the price or availability of a part the caller wants to
> buy over the counter — that is the Parts Counter, even when the caller
> describes the fault first. Do not route here merely because the caller asked
> for "a mechanic"; route on what they actually need.

What changed: the topics are enumerated rather than gestured at, the overlapping
case is settled explicitly and in one direction, and the "asked for someone"
near-miss is closed. Note that nothing in it is a sentence anyone will say.

### Rules

- **Make each destination's scope distinct.** Overlapping descriptions are the
  most common cause of misrouting. If two destinations could both match the same
  call, say explicitly which wins and when — in both descriptions if it is not
  obvious from one side.
- **Enumerate, don't gesture.** "Administrative matters" is weak; "an invoice,
  a payment, a reminder, a reimbursement or insurance question, or a change of
  contact details" is what actually routes correctly.
- **Say when NOT to route here.** A description that only says when to route
  here leaves the router guessing on near-misses.
- **No scripts.** Never write what the operator should say. This text cannot
  reach the caller.

## Two kinds of destination

A destination points either at a **phone number** or at **another operator**, and
the difference changes what you should write about it.

A phone number hands the caller out of Sainer to a real team. Whatever happens
after that — hold music, voicemail, nobody answering — is outside the operator's
control, so describe the people and what they handle.

Another operator keeps the caller inside Sainer, and the receiving operator's own
instructions, tools and languages take over from that point. So its scope is
whatever THAT operator is configured to do, not whatever you would like it to do.
Never write a description that promises something the receiving side cannot
deliver; if you are unsure what it handles, say so rather than guessing.

Which kind a destination is, and where it points, are set in the app and are not
yours to change.

## Availability

A destination can carry a weekly schedule saying when it may be transferred to.
While it is closed the operator will not offer it and will not connect to it.
Blocks on that schedule are CLOSED windows — see
[timetables.md](timetables.md) before writing one, because entering the opening
hours as the blocks inverts it.

## Pre-transfer notice

A short heads-up spoken to the caller just before the transfer fires — "have
your reference number ready". Write it in English **as an instruction of
intent**, not as a literal sentence: the operator renders it in the caller's own
language.

> Tell the caller to have their policy number to hand.

Not:

> "Houd uw polisnummer bij de hand."

A literal sentence pins the wording *and the language*, so a German caller hears
Dutch. Leave the notice empty where a heads-up would be inappropriate, such as
an urgent-care line.

## Spoken languages

If a destination's staff speak only some of the operator's languages, set them.
A caller speaking outside that list gets a warning before the transfer fires,
instead of being handed to someone who cannot help them.

## Hold phrase

What the caller hears right after a transfer starts, while the routing model
decides who to connect. Unlike everything above, this IS voice-model behaviour:
it changes what is spoken, not where the call routes. It is a setting on the
Transfers page (`transfer_hold_phrase`), never a scripted wait line in the
Instructions. Three modes:

- **`default`** — a fixed per-language hold phrase ("Een ogenblik geduld
  alstublieft, ik kijk wie u kan helpen."). Reliable but noticeably scripted;
  the safe choice.
- **`composed`** — the operator words the hold sentence itself, in the caller's
  language and in persona. Recommend this when a customer finds the standard
  phrase robotic or cold. Optionally steer it with `holdPhrasePrompt`: English
  intent text, one short sentence's worth, positively phrased.

  > Warmly acknowledge what the caller needs and say you are checking who can
  > best help them.

  Not a literal line to repeat, and never an announcement that a transfer is
  already decided — the routing model may still come back with a question
  instead of a connection.
- **`audio_only`** — no spoken phrase, only a short typing sound. Only
  appropriate when the Instructions make the operator ask permission before
  transferring, so the silence follows a natural "yes". It depends on the
  operator's typing sound setting being on; with that off this mode is dead
  air, so check it before proposing this.

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

### Worked example

> When a call could match both Workshop and Parts Counter and the caller has not
> made it clear which they need, ask one short question rather than guessing:
> whether they want work done on the car or want to buy a part.
>
> A caller who says they are stranded or unsafe goes to Workshop immediately,
> ahead of any other match, and without a clarifying question.
>
> Never ask for a customer number, registration or postcode in order to route.
> Ask for those only if the destination itself needs them.

Each of these is genuinely cross-cutting: a tiebreaker between two destinations,
a priority override, and a global prohibition. None of it could sit inside a
single destination's description.

**Never put voice-model behaviour here.** Whether to transfer at all, answering
from the knowledge base, and running a data-collection flow are the voice
model's job and belong in the Instructions. Guidance that says "always answer
from the knowledge base first" does nothing, because the model that reads it
cannot answer anything.

## A fallback destination is worth having

The destination a caller reaches when they need a person but nothing more
specific matches — and when they are frustrated and want out of the automated
system. Without one, those calls have nowhere to go.

---

## Before you finish a transfer setup

- [ ] Every description enumerates its topics rather than gesturing at a category
- [ ] Every pair of destinations that could match the same call has an explicit winner
- [ ] At least one description says when *not* to route there
- [ ] No sentence anywhere that anyone would say out loud
- [ ] Pre-transfer notices are intent in English, never a literal quoted sentence
- [ ] Routing guidance contains nothing that belongs in a single description
- [ ] There is a fallback
- [ ] `audio_only` hold phrase only proposed with the typing sound on and a consent-question transfer flow
