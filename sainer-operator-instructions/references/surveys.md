# Asking the caller

Read this when the operator asks the caller what they thought of the call, or
asks them to check details the call collected from them.

Both are the same thing underneath: the caller gets a text message with a link
to a short form. Both cost one text message each time, and both only reach Dutch
mobile numbers — a caller on a landline, a foreign number or a withheld number
gets nothing, and nothing should ever be promised to them.

---

## The satisfaction survey

One survey per operator. It is four fields plus its questions.

**Name** — an internal label, never spoken.

**When to offer it** — the trigger, as a caller situation:

- "At the end of the call, once the caller has been helped."
- "When the caller says they were happy with how it went."

The operator always *asks first* and only sends once the caller agrees. Write
the trigger as the moment to offer, not the moment to send — those are different
moments, and the difference is what keeps the message welcome.

**Message body** — the actual text, kept inside one message. This one *is*
delivered verbatim, like an SMS template. It must contain `{link}`; `{operator}`
is replaced with the operator's name. Anything longer than about 130 characters
is sent and billed as two.

**Intro and thank-you text** — what the caller reads above the questions, and
after they submit.

### The questions

The standard pair is a 1-5 rating plus an open remark, and there is a button in
the admin that fills them in. Beyond that: ratings (1-5, 1-10, NPS 0-10), short
or long text, single or multiple choice, yes/no, and the plain input types
(number, email, phone, date).

Two questions is usually the right number. Every extra one costs responses, and
the open remark is where the useful material actually comes from.

A question's key cannot be changed after it exists. Every answer already given is
filed under it, so renaming it would orphan them.

### Sending it automatically

Separate from whether the operator may offer a survey during a call, it can text
one after every call that qualifies. The two are independent switches: an
operator can offer one in conversation without ever sending automatically, or
the reverse.

The automatic send is inbound-only, and skips calls that were too short, calls
that reached a colleague (the caller would be rating them, not the operator),
and callers who were already surveyed recently. Each of those is adjustable.

It can also survey only a share of the calls that qualify. Set the percentage
when someone wants feedback without texting every caller; 100 is every call, 10
is roughly one in ten. Which calls get picked is decided per call and never
changes, so the same call always gets the same answer.

Do not reach for the monthly ceiling to do this. The ceiling surveys everyone
until the budget runs out and then nobody, so a month's feedback all comes from
whoever happened to call first. The percentage spreads it across the month.

### Reading the results

Report the funnel — sent, answered, response rate, average — never the average
on its own. Only Dutch mobile callers can be surveyed, so a lone score reads as
if it covered everyone who called, and it never does.

---

## Letting the caller check what was collected

A per-scenario setting on **Data collection**. When it is on, the caller gets a
text after the call with a form showing what the call captured from them —
their name, their email, the date they asked for — and can correct it. What they
send back becomes the authoritative version.

Turn it on for scenarios where a misheard value is expensive: an email address
that a confirmation goes to, an address someone drives to, an appointment date.
Leave it off for a scenario that only captures a topic or a preference.

Two things follow from switching it on, and both are worth saying out loud to
the customer:

- **It costs a text message per call** in that scenario.
- **The after-call exports wait for it.** The CRM write, the summary email and
  the webhooks hold until the caller answers or the wait expires (30 minutes by
  default), so that they carry the corrected values rather than the misheard
  ones. A caller who never answers simply gets the original behaviour, late.

There is nothing to write here: the form is generated from the scenario's own
fields, using each field's own label. Improving what the caller sees means
improving the **field labels** in Data collection, not writing a form.

---

## What does not belong here

- **A link to something else** — a booking page, a form of the customer's own.
  That is an ordinary text message under **Send SMS**, with the URL written into
  the body.
- **Asking for details during the call.** That is **Data collection**. This page
  is only about checking them afterwards.
- **Scripting the survey question in the Instructions.** The operator offers the
  survey in its own words from the trigger; the questions live on the form.
