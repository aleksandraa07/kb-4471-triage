# kb-4471-triage

Ticket #4471 on the support desk holds 412 reports, is marked known, and has one fix queued.
It is three problems, not one. Two of them have no ticket, no owner and no fix.

| problem | reports |
| --- | --- |
| The shared parser reads every date month first, so day-first tenants get the month and the day exchanged. | 374 |
| `booking-v3` prints the hint `MM/DD/YYYY`, people type month first, and the form reads it day first. | 33 |
| A one-day move, unrelated to date order. | 5 |

374 + 33 + 5 = 412. One ticket per problem is filed as an issue in this repo.

## What is here

```
src/routes/kb/incidents/4471/split/+page.svx   the page, for the kb
src/lib/kb/OneDayMove.svelte                   the component it needs
rendered/incidents-4471-split.html             the page as the desk will see it
```

The two files under `src/` sit at the paths they take inside the kb export
(`support-kb/src/…`), so they drop straight in.

## How the numbers were reached

From the desk's own rules, not from a fresh guess:

- `causeOf()` in `src/lib/kb/causes.ts` splits the ticket into `parser` and `hint`. Its own
  comment calls the second one "a different bug".
- `uniqueById()` is why the count is 412 and not 427: the ticket system exports in pages that
  overlap, so 15 rows arrive twice.
- The third problem is not something `causeOf` can see. It decides from the tenant and the
  channel and never reads the report, so these five come out as `parser`. The desk had already
  noticed them and listed them in `src/lib/data/desk-notes.json`; the kb footnotes them under
  the cause they were filed in.

The five were checked against the other 407 by parsing the date pairs out of every report
body. 403 describe a swap, where the day and the month are exchanged. These five describe a
one-day move, a meeting split across two cities, or an invite that arrived before the booking.
The month is right in all of them. No further reports of that kind are hiding in the ticket.

`rendered/incidents-4471-split.html` was generated from the export with those same rules, so
the page and the data cannot drift apart by a typo.

## Signature

The page's `owner:` is `aleksandraa07`. Its `signature:` is the ticket the course issued for
this Challenge, which is how the course recognises the page as ours. That signature gives desk
ref `DR-MSBM-SGZ7`, which is what `RenderStamp` prints at the foot of the page.
