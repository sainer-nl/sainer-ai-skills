# Email notifications

An email sent after a call has ended, to a fixed list of people, carrying what
the call analysis produced. It can go out for every call, or only for the calls
that match a rule.

This is where a custom analysis field stops being bookkeeping and starts being
useful: "email the porting team whenever a call was about porting" is a field
plus a rule, and neither half is worth much alone. When you propose a fixed-set
analysis field, the notification is usually the reason to bother, so offer both
together rather than leaving the user to work out the second half.

Not to be confused with **text messages**, which the operator sends to the
caller *during* the call. These go to colleagues, afterwards. See
[messaging.md](messaging.md) for the other one.

## What it needs

Only two things are required: a **name** (internal, nobody outside sees it) and
at least one **recipient**.

Everything else has a working default. A notification with no rule sends for
every call, with the standard body, under the standard subject. That is a
perfectly good notification, and often the right one. Do not decorate it.

**Never invent an email address.** The email carries the call summary and
whatever was captured from the caller, so a wrong address is a disclosure you
cannot take back. Use an address the user has given you in the conversation. If
you do not have one, ask; do not guess from a domain, a name, or another
notification.

## When it fires

A rule is a list of conditions, and **all of them must hold**. There is no OR.
If the user wants an email for porting *or* complaints, that is two
notifications, not one rule. Say so plainly rather than quietly building a rule
that only fires when a call is somehow both.

Each condition reads one of three things:

| Source | Reads |
|--------|-------|
| an **analysis field** | what the analysis concluded for this call |
| a **captured field** | something the operator collected from the caller during the call |
| the **handled scenarios** | which data-collection scenarios the call went through |

Then compares it. `equals` / `not_equals`, `contains` / `not_contains`,
`greater_than` / `less_than` all need a value to compare against. `is_set` /
`is_not_set` / `is_true` / `is_false` test presence only and take no value.

Comparison is forgiving: case is ignored, surrounding spaces are trimmed, and
`"5"` matches `5`. Against a list (handled scenarios, or a field that holds
several values) it matches if any entry does.

### The failure that is invisible

**A condition on a key that does not exist never matches, and nothing reports
it.** No error, no warning, no entry anywhere. The user configures a
notification, waits for the email, and it silently never comes.

So before writing a rule, check the operator's config for the field you are
about to key on, and use its exact key. If the field the user is describing does
not exist yet, propose it *first* and say the notification depends on it. A
rule pointing at a field nobody created is worse than no rule, because it looks
finished.

The same care applies to the value: a rule reading `equals "ICT"` against a
field whose allowed values are `ict` and `overig` is fine (case is ignored), but
`equals "IT"` matches nothing forever.

## The subject

Left empty, a sensible default subject is used. When the user wants their own,
these placeholders are filled in per call:

`{caller_name}` · `{caller_number}` · `{called_number}` · `{phone_number}` ·
`{direction}` · `{call_date}` · `{summary}` · `{caller_company}` ·
`{caller_intent}` · `{sentiment}`

There is one more: `{action}`, which is empty unless the call left action items,
so it works as a flag at the front of a subject. `{action:SPOED}` uses your own
label instead of the default.

Anything else in braces is not substituted. It arrives in the inbox looking
exactly like the mistake it is.

## What is in the email

By default: the call details, the summary, the sentiment, any action items, and
whatever was captured. That covers almost everyone, so leave it alone unless the
user asks for something specific.

The full set you can choose from, in the order you list them: call details,
summary, sentiment, action items, captured data, CRM fields, caller intent,
transcript. **The transcript and the caller intent are deliberately off by
default**. The transcript in particular makes for a long email and puts the
caller's own words in an inbox, which is a decision for the customer to make
rather than one to make for them.

An empty list is ignored and falls back to the default set, so you cannot use it
to send a body-less email.

## Test calls

Off by default: while a customer is testing their own operator, they do not want
each attempt emailing the whole team. Turn it on only if asked, and it is worth
mentioning as the reason a test call did not produce the email they expected.

## What you cannot set

**Time windows.** A notification can be limited to calls inside or outside
opening hours, or a linked timetable. That is set by hand on the Email
notifications page. Point the user there rather than working around it.

**Switching one off** is expressed as archiving it, never deleting. It stops
sending and stays recoverable.

---

## Before you finish an email notification

- [ ] Every recipient is an address the user actually gave you
- [ ] Each condition names a key that exists in this operator's config today
- [ ] Alternatives ("porting **or** complaints") are separate notifications, not one rule
- [ ] Operators that take no value (`is_set`, `is_true`) do not carry one
- [ ] A custom subject uses only the placeholders listed above
- [ ] The body is the default set unless the user asked for something else
- [ ] If it depends on an analysis field that does not exist yet, that field is proposed first
