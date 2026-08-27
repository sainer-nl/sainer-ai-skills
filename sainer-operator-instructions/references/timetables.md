# Schedules — opening hours and transfer availability

Read this before touching any weekly schedule.

---

## The one thing to get right

**A block is time that is CLOSED. Time no block covers is open.**

That is the opposite of what almost everyone assumes, and getting it backwards
closes a business during its own opening hours. Two consequences follow:

- A schedule with **no blocks at all** is open every minute of the week.
- Switching a schedule **off** does not close anything. It removes every closed
  window, so everything becomes open.

So "open Monday to Friday, nine to five" is not five blocks over the working
day. It is blocks over the nights and the weekend:

| Day | Closed block | Meaning |
|-----|--------------|---------|
| Sunday | 00:00–24:00 | closed all day |
| Monday | 00:00–09:00 and 17:00–24:00 | open 09:00–17:00 |
| Tuesday–Friday | same as Monday | open 09:00–17:00 |
| Saturday | 00:00–24:00 | closed all day |

Because this is so easy to invert, **always say in prose which hours end up
open** when you propose blocks, and let the person check you. "This leaves you
open Monday to Friday, nine to five, and closed at the weekend" catches the
mistake before it ships. A table of start and end minutes does not.

## Writing the numbers

- **Days** run 0 for Sunday through 6 for Saturday.
- **Times** are minutes from midnight. 09:00 is 540, 17:00 is 1020.
- **The end is exclusive.** A block that runs to the end of the day ends at
  **1440**, not 1439 — 1439 leaves the last minute of the day uncovered, which
  reads as open.
- **A window crossing midnight is two blocks**, one on each day. 22:00 Friday to
  06:00 Saturday is Friday 1320–1440 plus Saturday 0–360.
- There are **no date exceptions.** The schedule repeats weekly, so a public
  holiday or a one-off closure cannot be expressed. Say so rather than
  improvising something that looks close.

---

## The three kinds, and what each one does

The same weekly-block system is used three ways, and they are not
interchangeable.

### Opening hours

Describes when the business is open, and has **no effect on routing at all**.
Calls are answered exactly as before. Its one job is to tell the appointment and
data-collection planning which dates it may offer a caller, so an operator that
books appointments does not offer a Sunday.

This is the safest kind to work on, and the one to reach for when someone says
"it offered an appointment on a day we're closed".

### Transfer availability

Attached to a single transfer destination, and it decides whether the operator
may connect a caller there **right now**. While the destination is closed the
operator will not transfer to it, and does not offer it.

Blocks mean CLOSED here too. Worth setting when a department has real hours —
callers reaching a desk nobody is sitting at is worse than being told to call
back.

### Number routing

Decides **which operator answers a phone number** during a given window, and
here a block means the opposite: it is a routing window, and it carries a target.

This one is not yours to change. It controls where calls physically land, and a
mistake in it does not produce a worse answer, it produces a call nobody picks
up. Read one to explain why an operator is quiet outside certain hours, and
leave the editing to the person.

---

## Before you finish a schedule

- [ ] You stated in prose which hours end up OPEN, not just which blocks exist
- [ ] Full-day closures end at 1440
- [ ] Any window crossing midnight is two blocks
- [ ] You did not enter the opening hours as the blocks
- [ ] You said that holidays and one-off closures cannot be expressed
- [ ] You left number-routing schedules alone
