<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import notes from '#lib/data/desk-notes.json';
  import { uniqueById, type Report } from '#lib/kb/causes';

  // causes.ts decides a cause from the tenant and the channel, never from what the
  // report says. These reports describe a different symptom, so no cause in
  // CAUSES fits them and CauseTable can only footnote them under the one they
  // were filed in. The desk already listed them in desk-notes.json; this table
  // is that list, read back out of the data rather than typed into a paragraph.
  let { ticket }: { ticket: number } = $props();

  const rows = $derived(
    uniqueById(reports as Report[])
      .filter((r) => r.mergedInto === ticket && notes.oneDayMove.includes(r.id))
      .sort((a, b) => a.receivedAt.localeCompare(b.receivedAt)),
  );

  const tenants = $derived(new Set(rows.map((r) => r.tenant)).size);
  const day = (iso: string) => iso.slice(0, 10);
</script>

<p class="sum">
  <strong>{rows.length}</strong>
  {rows.length === 1 ? 'report describes' : 'reports describe'} a one-day move, across
  <strong>{tenants}</strong>
  {tenants === 1 ? 'tenant' : 'tenants'} · no cause in <code>CAUSES</code> fits them
</p>

<table>
  <thead>
    <tr><th>report</th><th>tenant</th><th>in</th><th>received</th><th>what it says</th></tr>
  </thead>
  <tbody>
    {#each rows as r (r.id)}
      <tr>
        <td><code>{r.id}</code></td>
        <td>{r.tenant}</td>
        <td>{r.channel}</td>
        <td>{day(r.receivedAt)}</td>
        <td class="body">{r.body}</td>
      </tr>
    {/each}
  </tbody>
</table>

<style>
  .sum { font-size: 1.1rem; }
  .body { color: var(--muted); font-size: 0.85em; }
</style>
