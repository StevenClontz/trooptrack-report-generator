<script lang="ts">
  export let startDate = new Date();
  export let endDate = new Date();
  let startDateString = startDate.toISOString().substring(0, 10);
  let endDateString = endDate.toISOString().substring(0, 10);
  $: startDate = new Date(startDateString);
  $: endDate = new Date(endDateString);
</script>

Start:
<input type="date" bind:value={startDateString} class="border px-2 py-1 rounded mr-2"/>

End:
<input type="date" bind:value={endDateString} class="border px-2 py-1 rounded mr-2"/>
