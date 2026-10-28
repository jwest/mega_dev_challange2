<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import notes from '#lib/data/desk-notes.json';
  import { type Report } from '#lib/kb/causes';
  import { splitTicket, type Problem } from '#lib/kb/problems';
  import { tenantOf } from '#lib/kb/locales';

  // One number for a sentence: reports in one problem of a ticket, or (no problem) all of them.
  let { ticket, problem, what = 'reports' }: { ticket: number; problem?: Problem; what?: 'reports' | 'tenants' | 'mistagged' } = $props();

  const split = $derived(splitTicket(reports as Report[], ticket, tenantOf, notes.oneDayMove));
  const group = $derived(split.groups.find((g) => g.problem === problem));
  const value = $derived(
    !problem ? split.merged.length : !group ? 0 : what === 'reports' ? group.reports.length : what === 'mistagged' ? group.mistagged : group.tenants.length,
  );
</script>

<strong>{value}</strong>
