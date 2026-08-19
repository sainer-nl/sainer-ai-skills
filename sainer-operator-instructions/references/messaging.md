# Text messages and keypad actions

Read this when the operator sends the caller a text mid-call, or presses keypad
tones on a line it has dialled.

---

## Text messages (SMS)

Each message is three fields.

**Name** — an internal label, never spoken: "Booking link", "Damage report form".

**When to send** — the trigger, as a caller situation:

- "When the caller asks for a link to the booking page."
- "When the caller agrees to receive the damage-report form by text."

Prefer *"when the caller agrees"* over *"when the caller asks"* where the
operator is expected to offer it. Those are different moments and the difference
decides whether the message is welcome or unexpected.

**Message body** — the actual text, kept short. Write it in the language the
caller will be speaking; this one *is* delivered verbatim, unlike almost
everything else in an operator's configuration. If it carries a verification
code, place the code placeholder exactly once.

### The pairing that gets missed

A text message is only useful if the call flow reaches the moment it is sent. If
the Instructions promise to text something, a template must exist to send; if a
template exists but nothing in the call ever offers it, it never fires. Check
both directions — a promise with no template is the more damaging half, because
the caller is told something will arrive and nothing does.

---

## Keypad actions (DTMF)

For lines the operator dials into that expect tones — a gate, an IVR menu, an
extension.

**Name** — the goal, plainly: "Open the gate", "Choose the service department".

**Digits** — only `0-9`, `*` and `#`.

**When to use** — the trigger, again as a situation: "When a delivery driver
asks to be let in at the gate."

Keypad actions do something physical in the world. Make the trigger narrow and
unambiguous, and never write one that could fire on a vague request — "when the
caller wants access" is too loose for something that opens a door.
