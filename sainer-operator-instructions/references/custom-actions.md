# Custom actions

Read this when the operator looks something up or calls an external system
mid-call — an order status, a stock check, an address lookup.

A custom action needs an endpoint, authentication and a request body. Those are
set up in the app by the person who owns the system being called; this skill
covers the **text around it**, which is what decides whether the operator uses
the action at the right moment and says something useful with the result.

If the action does not exist yet, say so plainly and ask the user to create it
first with any working description. Once it exists, its wording can be improved.

---

## Description — when to use it

The most important field. Write it as a **caller-situation trigger**:

- Good: "Use this when the caller asks about the status of an existing order."
- Avoid: "Queries the order system." That is how it works, not when to use it.

Keep each action's trigger distinct from every other action, and from the
knowledge base. If two things could fire on the same request, the operator picks
unpredictably — say which situation belongs to which.

## Parameter descriptions

For each input, a short note about **what the caller provides**, not what the
API expects:

- `order_number` → "The caller's order number, usually six digits."
- `postcode` → "The postcode of the delivery address, not the billing address."

## How the response reaches the operator

An action's response can reach the operator three ways, and the choice decides
which of the fields below actually does anything.

| Mode | What happens | Cost |
|------|--------------|------|
| **Processed by AI** (default) | A second model reads the response and summarises it | An extra model round trip mid-call, growing with the size of the response |
| **Filtered by parser** | A generated parser reduces the response to the records that matter, and the operator reads those directly | No extra round trip |
| **Raw response** | The operator reads the response verbatim | No extra round trip, but only workable when the response is already small and speakable |

Recommend the parser when an action returns a big or repetitive payload — a
list of branches, a product catalogue, a search result — and the operator only
needs a few fields from it. That is where the extra round trip costs the most
and buys the least. Leave an action on the interpretation model when the
response is small, or when what to say about it genuinely depends on the
conversation.

**On a parser action, the interpretation instructions no longer run.** The
operator reads the parser's output directly. So if someone complains their
action says the wrong thing and it is on the parser, the fix is the
response-shape goal, not the interpretation instructions — rewriting those
changes nothing they will ever hear.

The one exception: an action set to the parser that never had a parser
generated falls back to interpretation. If the mode says parser but no parser
exists, that is the thing to fix first, and it is fixed in the app.

## Interpretation instructions

How to turn the result into something spoken. Only used when the action is on
the interpretation model. Cover three things:

- what to mention — "give the next delivery date and the carrier"
- what to leave out — internal codes, prices the caller should not hear
- what to do on an empty result — "say you could not find it and offer to take
  a message"

The empty case is the one that gets skipped and the one that goes wrong most
often on real calls.

## Pre-call preamble

For an action that runs *before* the call, this says how to **use** the fetched
data — not what the data is.

- Good: "This is the caller's account. Greet them by first name and mention
  their last order only if it is relevant to what they ask."
- Avoid: "This is a JSON object with the customer's details." The operator can
  see that.

## Response-shape goal

Plain words describing what the operator actually needs out of the API
response — "the three nearest branches with their opening hours". It is used to
generate the shaping automatically, so write the goal, not the transformation.

A goal that requires ranking needs something to rank *by* on both sides. Asking
for "the nearest branch" when the caller may not have given a location cannot be
answered, and an answer that looks confident will be wrong. Ask for what is
missing instead.
