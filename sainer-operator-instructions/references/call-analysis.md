# Call analysis — custom fields

Read this when the business needs to know something about *every* call, not just
handle the call well.

Every call is analysed after it ends. The standard analysis already produces a
summary, sentiment, the outcome, the topics, the caller's intent, follow-up
actions and the questions that went unanswered. On top of that, an operator can
extract **its own fields** — one specific thing, pulled out of every
conversation, in a form that can be counted and acted on.

Most people do not know this exists, so it is usually you who has to raise it.

---

## Spotting the opportunity

You are not waiting to be asked. Listen for anything the business **sorts calls
by**, and you have a candidate:

- which department a callback belongs to
- which brand, model or product line was discussed
- which branch or location the caller wants
- whether the caller is an existing customer or a new one
- which of a handful of reasons they were calling about
- whether an appointment was actually wanted

You will usually hear these while working on something else — writing a transfer
description, going through the call flow. Say what you noticed, what the field
would capture, and what it would let them do. Then let them decide.

## Why an enum beats free text

A field can hold text, a number, true/false, or one of a fixed set of values.
The fixed set is worth far more than the others, because a fixed value can:

- **drive a notification.** An email rule can fire only for a particular value,
  so the right team is told and nobody else is.
- **group a report.** Fifty calls become five rows.
- **be counted over time.** A trend needs the same value to appear twice, and
  free text almost never does.

Free text can be read one call at a time and not much else. So whenever the
answer really does fall into a handful of buckets, make it a fixed set — and
propose the buckets rather than asking the user to invent them.

## Writing the field

The description is not a label. It is the instruction the analysis follows on
every call, and it is where a good field is won or lost. It has to answer four
things:

1. **When to fill it in, and when to leave it empty.** This is the one that gets
   skipped, and skipping it is why a field ends up filled in on calls it has
   nothing to do with.
2. **What each value means.** One line each, in the business's own words.
3. **How to choose when a call matches more than one.** Real calls are messy.
4. **What to fall back on when it is genuinely unclear.** Which is why the value
   set almost always needs a catch-all.

### Worked example

A company wants callback requests routed to the right team by email. First
attempt:

> The department for the callback.

This is filled in on every call, including ones with no callback at all, and the
analysis has to guess what the department names mean. Notifications then go to
teams who were never asked for.

Rewritten:

> Which department should call the caller back. Only fill this in for callback
> requests; leave it empty on any other call.
> commercie: sales, new business, purchasing, suppliers, invoices, partnerships,
> marketing and sponsoring.
> operatie: operational and logistics matters, planning, capacity, transport,
> and support for a branch or its staff.
> overig: anything that does not clearly belong to the two above, including
> complaints, HR and job applications, IT, and general enquiries.
> When a call touches more than one department, choose the one the caller's main
> request belongs to. When it is genuinely unclear, choose overig.

Values: `commercie`, `operatie`, `overig`.

Every one of the four requirements is met, and the whole thing is written for a
reader who has never seen this business before — which is exactly what the
analysis is.

## Naming

Lower_snake_case, and specific: `terugbel_afdeling`, not `department`. The name
is the field's identity — reusing an existing name edits that field rather than
adding a new one.

You cannot redefine any of the standard analysis fields, and you should not try
to reproduce one. If what they want is close to the summary, the outcome or the
topics, say so and save them a field.

## Where it pays off

Say the payoff concretely rather than leaving it abstract:

- **Notifications.** A fixed value is the cleanest condition an email rule can
  have. This is the strongest case for a custom field and worth naming first.
- **Reports.** The field is aggregated automatically — counts per value across
  the period, with no extra work.
- **The dashboard.** A numeric field can be pinned as a card. Three maximum.
- **The CRM.** A field can be copied onto the caller's contact as last-call
  context, when an integration is connected.

---

## Before you finish an analysis field

- [ ] The description says when to leave it empty, not only when to fill it in
- [ ] Every value is defined in the business's own words
- [ ] There is a rule for a call that matches more than one value
- [ ] There is a catch-all value for the unclear case
- [ ] The name is specific and lower_snake_case
- [ ] It does not duplicate something the standard analysis already produces
- [ ] You said what the field will actually be used for
