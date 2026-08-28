# Where things belong

When something the user describes belongs on another page rather than in the
Instructions, say so and point them to the right place — don't quietly absorb it into
the Instructions. The need → page lookup:

| The need | Where it goes |
|----------|---------------|
| How to pronounce a name, brand, or product | **Vocabulary** (with a rough phonetic spelling) |
| When and why to connect a caller to a specific place | That destination's **description** on the **Transfers** page |
| Whether a destination needs the caller to confirm, or is a catch-all, or whether its name may be spoken | That destination's settings on **Transfers** |
| Which languages a destination's staff speak (so callers get a heads-up) | That destination's **spoken languages** setting |
| Cross-cutting routing rules that span destinations (read only by the routing model, never by the voice model — write as routing decision criteria; see [transfers.md](transfers.md)) | The **routing context** (set alongside Transfers, not in the Instructions) |
| What the caller hears while a transfer is being routed (the hold phrase: fixed sentence, own wording, or sound only; see [transfers.md](transfers.md)) | The **hold phrase** setting on the **Transfers** page — never a scripted wait line in the Instructions |
| What a detail to capture is, and how to ask for it | That **field**'s description in **Data collection** |
| Overriding the default confirmation for a captured detail | That field's **custom verification** in **Data collection** (sensible defaults already exist) |
| Which details to capture, and when | A **scenario** in **Data collection** — a separate item that links fields to a situation |
| When the operator should run a lookup or external call, and how to read back the result | That **custom action**'s description, parameters, and interpretation instructions |
| When the operator should text the caller, and the message | That message's **"when to send"** and body under **Send SMS** |
| When the operator should press keypad tones | That action's **"when to use"** under **Keypad actions** |
| Opening / supported languages, voice, gender, which tools are on, a pre-recorded welcome | The operator **Settings** |

For how to phrase any of these tool fields, see [transfers.md](transfers.md).

## And some things belong to nobody

There is a third answer besides "the Instructions" and "another page": nothing
the customer can set. An operator's full system instruction is mostly written by
Sainer, not by the customer — fixed rules about how digits, acronyms, email
addresses and times are spoken, how the operator holds one language when a name
from another turns up, plus a governing rule per tool. None of it is editable,
and the Instructions are read alongside it rather than instead of it.

Two failures follow, and both look like good work:

- **Restating a fixed rule** in the Instructions. It applies cleanly, reads as an
  improvement, and changes nothing, because the rule was already there.
- **Contradicting one.** Now the operator has two instructions that disagree, and
  which wins on a live call is not something you can predict from the text.

So before writing Instructions about *how something is said* rather than *what is
said*, check whether it is already handled. In the Sainer console the config read
reports exactly this per operator: which sections are the customer's, which are
fixed, which are generated from other config, what the fixed ones already cover,
and which of those are deliberately **not** covered for this operator's language.
Trust that over anything you remember, and read the real text before telling
someone a behaviour cannot change.

When it genuinely cannot: say so plainly and raise it, rather than offering a
change that cannot work. A limitation stated honestly is more useful than a
confident fix that quietly does nothing.

When recommending text for another page (a destination description, a field's
description), the same "write it in English" guidance applies — the operator still
speaks to callers in their own language.
