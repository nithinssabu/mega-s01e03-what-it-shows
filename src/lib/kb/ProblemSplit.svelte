<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import notes from '#lib/data/desk-notes.json';
  import { causeOf, uniqueById, type Report } from '#lib/kb/causes';
  import { tenantOf } from '#lib/kb/locales';

  let { ticket }: { ticket: number } = $props();

  const REPO = 'https://github.com/nithinssabu/mega-s01e03-what-it-shows/issues';

  const merged = $derived(uniqueById(reports as Report[]).filter((r) => r.mergedInto === ticket));
  const count = (cause: 'parser' | 'hint') => merged.filter((r) => causeOf(r, tenantOf(r)) === cause).length;
  const moved = $derived(merged.filter((r) => notes.oneDayMove.includes(r.id)).length);

  const problems = $derived(
    [
      { name: 'parser reads month first', issue: 1, n: count('parser') },
      { name: 'booking-v3 form hint says MM/DD/YYYY', issue: 2, n: count('hint') },
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

{#if moved}
  <p class="note">{moved} of the parser reports read as a one-day move; counted under the parser by tenant.</p>
{/if}

<style>
  .sum { font-size: 1.1rem; }
  .note { color: var(--muted); font-size: 0.85em; }
</style>
