<script lang="ts">
    import AccountSelect from '$lib/AccountSelect.svelte';
    import CsvTable from '$lib/CsvTable.svelte';
    import DatePicker from '$lib/DatePicker.svelte';
    import InputFile from '$lib/InputFile.svelte';
    let csvData: Record<string, string>[] | null = null;
    let chosenAccountNames: string[] = [];
    let startDate: Date = new Date();
    let endDate: Date = new Date();
</script>

<h1 class="text-3xl font-bold text-center mt-8 mb-4 text-blue-700">TroopTrack Money Account Report Generator</h1>

{#if csvData}
    <AccountSelect {csvData} bind:chosenAccountNames />
    <DatePicker bind:startDate bind:endDate />
    <div class="mx-auto my-6 p-4 bg-gray-100 rounded shadow">
        <h2 class="text-xl font-semibold mb-2">CSV File Contents</h2>
        <CsvTable {csvData} />
    </div>
{:else}
    <p>
        Visit your TroopTrack Money Book page and export the report as a CSV file to upload here.
    </p>
    <InputFile bind:csvData />
{/if}

