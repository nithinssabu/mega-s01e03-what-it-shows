<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import notes from '#lib/data/desk-notes.json';
  import { causeOf, uniqueById, type Report } from '#lib/kb/causes';
  import { tenantOf } from '#lib/kb/locales';

  let { ticket }: { ticket: number } = $props();

  const REPO = 'https://github.com/nithinssabu/mega-s01e03-what-it-shows/issues';

  const merged = $derived(uniqueById(reports as Report[]).filter((r) => r.mergedInto === ticket));
  const isShift = (r: Report) => notes.oneDayMove.includes(r.id);

  // The desk footnotes the one-day moves under `parser`; they are a problem of their own.
  const problems = $derived(
    [
      { name: 'parser reads month first', issue: 1, n: merged.filter((r) => !isShift(r) && causeOf(r, tenantOf(r)) === 'parser').length },
      { name: 'booking-v3 form hint says MM/DD/YYYY', issue: 2, n: merged.filter((r) => !isShift(r) && causeOf(r, tenantOf(r)) === 'hint').length },
      { name: 'one day off, cause unconfirmed', issue: 3, n: merged.filter(isShift).length },
    ].sort((a, b) => b.n - a.n),
  );
</script>

<p class="sum">
  <strong>{merged.length}</strong> reports merged into #{ticket} ·
  <strong>{problems.length}</strong> problems
</p>

<table>
  <thead>
    <tr><th>problem</th><th>reports</th><th>ticket</th></tr>
  </thead>
  <tbody>
    {#each problems as p (p.issue)}
      <tr>
        <td>{p.name}</td>
        <td>{p.n}</td>
        <td><a href="{REPO}/{p.issue}">#{p.issue}</a></td>
      </tr>
    {/each}
  </tbody>
</table>

<style>
  .sum { font-size: 1.1rem; }
</style>
