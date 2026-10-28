# S01E03 · what it shows

Ticket #4471 on the support desk merged 412 reports about dates into one issue. It describes three problems:

| Problem | Reports | Ticket |
|---|---|---|
| Parser reads dates month first for day-first tenants | 374 | [#1](../../issues/1) |
| booking-v3 form hint says `MM/DD/YYYY` to day-first tenants | 33 | [#2](../../issues/2) |
| Dates arrive exactly one day off, cause unconfirmed | 5 | [#3](../../issues/3) |

374 + 33 + 5 = 412 unique reports (the export holds 427 rows; 15 are repeats by id).

- `src/routes/kb/incidents/4471-what-it-shows/+page.svx`: the kb page.
- `src/lib/kb/ProblemSplit.svelte`: the component that computes the counts on that page.
- `rendered/4471-what-it-shows.html`: the page as the desk will see it.
