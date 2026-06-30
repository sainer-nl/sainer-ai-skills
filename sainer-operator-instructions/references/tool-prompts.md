# Writing the text that steers your tools

Several tools take a few short pieces of text that the user writes and the operator
reads — a "when to use this", a description, an instruction. These don't go in the
Instructions; they live on the tool itself. When the interview surfaces one of these,
help the user write it and output the finished text, ready to paste.

For the current list of tools, where each is configured in the app, and step-by-step
setup, see the Sainer documentation at https://docs.sainer.nl — it's the up-to-date
source if a tool or page has changed.

Three rules apply to all of them:

- **Write the trigger as a situation, not a mechanism.** "Use this when the caller asks
  about the status of an existing order" — not "calls the orders API". The operator
  decides whether to use a tool from *when to use it*, not from how it works.
- **Keep each tool's trigger distinct.** If two tools could fire on the same request,
  make their triggers clearly different so the operator picks the right one.
- **Write it in English.** The operator still speaks to callers in their own language;
  the steering text just works most reliably in English.

---

## Custom actions

A custom action lets the operator do something specific — look something up, call an
external system, or search a knowledge source. The user authors these text fields:

- **Name** — an operator-facing label ("Order status lookup"). Not spoken; just a label.
- **Description (for the voice agent)** — the most important field: *when* the operator
  should reach for this action. Write it as a caller-situation trigger.
  - Good: "Use this when the caller asks about the status of an existing order."
  - Avoid: "Queries the order system." (That's how it works, not when to use it.)
- **Parameter descriptions** — for each input the action needs, a short note so the
  operator knows what to ask the caller for. Keep it about what the caller provides.
  - e.g. `order_number` → "The caller's order number, usually six digits."
- **Interpretation instructions** (when the result is summarised) — how to turn the
  result into a spoken answer: what to mention, what to leave out, what to do on an
  empty result.
  - e.g. "Give the next delivery date and the carrier. If there's no matching order, say
    you couldn't find it and offer to take a message."
- **Preamble** (for actions that run *before* the call) — how to use the data that was
  fetched, not a restatement of it.
  - e.g. "This is the caller's account. Greet them by first name and mention their last
    order if it's relevant."

---

## Transfers

- **Destination description** — when to connect a caller to this destination. Two shapes:
  - By topic: "Use for billing questions — invoices, payments, refunds, debtors."
  - By name only: "Only if the caller asks for this person by name."
  Make each destination's scope distinct so they don't overlap. No scripts or "say X".
- **Routing guidance** — cross-cutting rules that span destinations. Crucial to understand who
  reads this: routing guidance and the destination descriptions are consumed **only by the routing
  model** that picks the destination — **never by the voice model on the call** (the voice model
  sees destination names only and just relays the routing model's questions). So write routing
  guidance as *routing decision criteria*, not call behaviour. It can set tiebreakers for
  multi-topic calls, urgency/priority overrides between destinations, what the router must never ask
  the caller just to route (a postcode, an account number), how to frame a stand-in when a
  destination is closed, and how the router should word a confirmation or clarifying question. It
  cannot invent destinations, override a destination's description, or invent a closure the
  descriptions don't state. **Never put voice-model behaviour here** — whether to transfer at all,
  answering from the knowledge base, or running a data-collection flow are the voice model's job and
  belong in the Instructions, not routing guidance. Keep each destination's "when to route here" in
  its description; routing guidance carries only what spans them.

---

## Data collection

Data collection has two separate, linked pieces: **fields** (the individual details to
capture) and **scenarios** (which fields to collect, and when). You set up the fields,
then a scenario links the relevant ones to a situation.

**Fields** — each detail the operator captures:

- **Display label** — name the field plainly ("Email address").
- **Description** — what this field is and how to ask for it: accepted formats, shortcuts
  ("or just confirm the number you're calling from"), disambiguation hints. This is the
  main text the operator reads to know what to capture, so write a clear one for every
  field — don't leave it blank.
- **Custom verification** (optional) — Sainer already confirms captured values with a
  sensible default for each field type (read-back, spelling out emails, and so on), so you
  rarely need to write one. Only add a custom verification when you want to override that
  default with specific phrasing — e.g. "Spell the email back letter by letter and ask if
  it's correct."

**Scenarios** — a named situation that tells the operator which fields to collect and when:

- **Name** — a label ("Callback request").
- **Description** — when this scenario applies. Enumerate every trigger so it isn't
  missed: the caller asks for a callback, the knowledge base has no answer, a transfer was
  declined, the end of the call, and so on. The scenario links the fields to capture in
  that situation.

---

## Send SMS

- **Name** — your label for the message ("Booking link").
- **When to send** — the trigger: "When the caller asks for a link to the booking page."
- **Message body** — the actual text, kept short. If it carries a code, place the code
  placeholder once.

---

## Keypad (DTMF) actions

- **Goal** — what the action does ("Open the gate").
- **When to use** — the trigger: "When a delivery driver asks to be let in at the gate."

---

## Knowledge sources

- **Name** — a clear, recognisable label ("Product FAQ", "Returns policy"). The content
  and URLs are about what's *in* the knowledge base, not how the operator talks, so no
  prompting craft is needed beyond a clear name.
