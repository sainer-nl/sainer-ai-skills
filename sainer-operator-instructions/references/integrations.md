# Integrations — what is connected, and what it changes

Read this when the operator's config has an `integrations` block, or the user
mentions a system the operator is connected to (a phone system, a CRM, a
calendar).

`integrations.integrations` lists every integration on this operator with
`connected: true` once it is switched on and holds credentials. You can **read**
this, but you cannot connect, disconnect or change an integration's settings.
Those live on the integration's own page (Tools, then the integration). When a
change belongs there, say so and point the user to it.

Never ask for, repeat or guess credentials. You do not see them, and nothing
you write should need them.

---

## Cisco BroadWorks (phone system)

`integrations.broadworks` is present when the operator is connected to a
BroadWorks phone system (RoutIT). It feeds the operator four things:

| What | What the operator gets |
|------|------------------------|
| **Directory** | The colleagues on the phone system, synced into Sainer |
| **Colleague availability** | A tool that checks whether a named colleague is free, on a call, or away, before offering to put a caller through |
| **Queue status** | A tool that reports how many callers are waiting in a phone-system queue |
| **Previous contact** | When a caller phoned before, which colleague they last spoke to |

### When the directory updates

On connect, after the user saves a directory setting on the integration page
(the sync is then **queued**; it runs as soon as it gets its turn), and every
night. Never promise "immediately" or "within a minute": syncs across all
customers share one connection to the phone system and wait their turn.

### The summary fields

- `colleagues` — how many people are in the synced directory. Colleague
  availability only knows these names, so "the operator could not find Jan"
  usually means Jan is not in the directory (yet).
- `withoutCallLog` — directory entries the phone system never hands call logs
  for, typically voicemail boxes. Expected, not a fault. Sainer skips them when
  importing calls.
- `saveAsInternalCallers` / `saveAsTransferDestinations` — whether the sync also
  creates internal callers and transfer destinations. Both are off unless the
  user switched them on.
- `departments` — `per_group` takes each colleague's department from the phone
  system; `fixed` puts everyone under one department. Colleagues without a
  department in the phone system land under a department called **Default**;
  moving one elsewhere by hand sticks.
- `placeholderDestinations` — transfer destinations the sync created that still
  carry their generated description (see below).

### Placeholder destination descriptions — your job

When "save as transfer destinations" is on, every colleague becomes a transfer
destination with only `Name, Department, extension 201` as its description.
That names the colleague but says nothing about **when** a caller should go
there, so the routing model almost never picks them. The config reports these
as the gap `integration_placeholder_destination_description`, and the user may
arrive from a "Vraag Matt" / "Ask Matt" button on the Transfers page asking for
exactly this.

Treat it like any destination without a description, following
[transfers.md](transfers.md):

1. Ask what each colleague (or group of colleagues) actually handles. Do not
   invent a role from a name or a department label.
2. Write a routing description per destination, as decision criteria for the
   routing model. For colleagues who only take calls for themselves, the
   "by name only" shape is right: route there only when the caller asks for
   that person.
3. Propose them for the user to apply. Once rewritten, a destination drops off
   the list and the sync never overwrites the description again.

Many colleagues at once: group the ones with the same role and confirm the
grouping before writing, rather than interviewing person by person.

### Previous contact

When this is on, the operator already receives one line of background before
the call: who the caller last spoke to and when. It is background only: the
operator does not open with it, and the routing model uses it only to break a
tie. Do **not** add instructions that greet callers with their last contact or
route by it. That would contradict how it is designed and can tell a caller
something they did not ask about.

### What not to do

- Do not list colleagues, extensions or departments in the Instructions to
  compensate for an empty directory. The fix is on the integration page, or a
  wait until the next sync.
- Do not tell the user a colleague is "available" or "busy" from the config:
  availability is checked live during a call, not stored.
