<script lang="ts">
  export let startDateString:string;
  export let endDateString:string;
</script>

Start:
<input type="date" bind:value={startDateString} class="border px-2 py-1 rounded mr-2"/>
{startDateString}

End:
<input type="date" bind:value={endDateString} class="border px-2 py-1 rounded mr-2"/>
