# S01E03 · what it shows

Ticket #4471 on the support desk merged 412 reports about dates into one issue. It describes two problems:

| Problem | Reports | Ticket |
|---|---|---|
| Parser reads dates month first for day-first tenants | 379 | [#1](../../issues/1) |
| booking-v3 form hint says `MM/DD/YYYY` to day-first tenants | 33 | [#2](../../issues/2) |

379 + 33 = 412 unique reports (the export holds 427 rows; 15 are repeats by id). 5 of the 379 read as a one-day move; the desk counts them under the parser by tenant.

Engineering closed #4471 on 5 Oct: the parser is fixed (#1), the booking-v3 form was retired so the hint is won't fix (#2).

- `src/routes/kb/incidents/4471-what-it-shows/+page.svx`: the kb page.
- `src/lib/kb/ProblemSplit.svelte`: the component that computes the counts on that page.
- `rendered/4471-what-it-shows.html`: the page as the desk will see it.
