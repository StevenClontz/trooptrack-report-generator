<script lang="ts">
    import AccountSelect from '$lib/AccountSelect.svelte';
    import CsvTable from '$lib/CsvTable.svelte';
    import DatePicker from '$lib/DatePicker.svelte';
    import InputFile from '$lib/InputFile.svelte';
    let csvData: Record<string, string>[] | null = null;
    let chosenAccountNames: string[] = [];
    let startDate: Date = new Date();
    let endDate: Date = new Date();
    let showReport = false;
    const runReport = () => {
        showReport = true;
    }
</script>

{#if chosenAccountNames.length > 0 && showReport}
    <button class="bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700" on:click={_=>showReport = false}>Reset Report</button>
{:else if csvData}
    <AccountSelect {csvData} bind:chosenAccountNames />
    <DatePicker bind:startDate bind:endDate />
    <button class="bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700" on:click={runReport}>Run Report</button>
    <div class="mx-auto my-6 p-4 bg-gray-100 rounded shadow">
        <h2 class="text-xl font-semibold mb-2">CSV File Contents (first 10 rows)</h2>
        <CsvTable {csvData} rows={10} />
    </div>
{:else}
    <h1 class="text-3xl font-bold text-center mt-8 mb-4 text-blue-700">TroopTrack Money Account Report Generator</h1>
    <p>
        Visit your TroopTrack Money Book page and export the report as a CSV file to upload here.
    </p>
    <InputFile bind:csvData />
{/if}

