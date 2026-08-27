# Data collection — fields and scenarios

Read this when the operator needs to capture details from a caller: a callback
number, an address, an appointment preference, a description of a problem.

Data collection is **two linked things**, and confusing them is the usual cause
of an operator that captures nothing:

- A **field** is one detail — an email address, a registration number.
- A **scenario** is a situation that says *which* fields to collect and *when*.

A field attached to no scenario is never collected. A scenario with no fields
collects nothing. Both are silent failures: everything looks configured.

---

## Fields

**Display label** — name it plainly: "Email address", not "email_1".

**Description** — the main text the operator reads to know what to capture, and
the one that most often decides whether collection succeeds. Write:

- what the detail actually is
- how to ask for it, in situation terms
- accepted formats and shortcuts — "or just confirm the number they are calling
  from"
- disambiguation — which of two similar things you want ("the registration of
  the car, not the policy number")

Never leave it blank. An empty description is the single most common reason a
field is captured wrongly or not at all.

**Required** — only when the call genuinely cannot be useful without it. Every
required field is another thing the operator must chase before it can close,
and a caller who cannot supply one gets stuck.

**Verification** — Sainer already confirms captured values sensibly per field
type: reading numbers back, spelling emails out. Only write a custom
verification to *override* that default with specific phrasing. Adding one that
restates the default just makes calls longer.

**Field key and type are permanent.** The key identifies every value already
collected under it, so renaming it orphans the history; the type drives
validation and read-back. Both are set once, at creation. Get them right rather
than planning to fix them.

---

## Scenarios

**Name** — the situation, not the mechanism: "Callback request", not
"collect_phone".

**Description** — every trigger that should start this scenario. Enumerate them;
a scenario that lists one trigger fires on one trigger. Typical triggers:

- the caller asks to be called back
- the knowledge base has no answer
- a transfer was declined or nobody was available
- the caller wants to report something the operator cannot handle
- the end of the call

**Attach the fields.** A scenario collects exactly the fields linked to it.

---

## Writing a scenario is usually two changes, not one

A scenario describes what to capture and when — but the operator also has to
*reach* that point in the conversation. If the call flow never leads anywhere
that triggers it, the scenario is dead configuration.

So when a scenario matters to how the call goes, check the Instructions
mention the situation at flow level ("when you cannot answer, offer to take
their details"), while leaving the field list and the triggers here. Do not
duplicate the field list into the Instructions — that is exactly the
maintenance trap this split exists to avoid.
