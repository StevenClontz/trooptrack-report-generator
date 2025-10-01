<script lang="ts">
    import AccountSelect from '$lib/AccountSelect.svelte';
    import CsvTable from '$lib/CsvTable.svelte';
    import DatePicker from '$lib/DatePicker.svelte';
    import InputFile from '$lib/InputFile.svelte';
    import Report from '$lib/Report.svelte';
    let csvData: Record<string, string>[] | null = null;
    let chosenAccountNames: string[] = [];
    let startDateString: string = new Date(new Date().getFullYear(), new Date().getMonth() - 1, 1).toISOString().substring(0, 10);
    let endDateString: string = new Date(new Date().getFullYear(), new Date().getMonth(), 0).toISOString().substring(0, 10);
    let showReport = false;
    const runReport = () => {
        if (chosenAccountNames.length === 0) {
            alert("Please select at least one account.");
            return;
        }
        if (endDateString < startDateString) {
            alert("Please select an end date no earlier than the start date.");
            return;
        }
        showReport = true;
    }
</script>

{#if showReport}
    <h1 class="print:hidden text-3xl font-bold text-center mt-8 mb-4 text-blue-700">TroopTrack Money Account Report Generator</h1>
    <div class="print:hidden">
        <button class="bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700 inline-block" on:click={_=>showReport = false}>Reset Reports</button>
        <button class="bg-green-600 text-white px-4 py-2 rounded hover:bg-green-700 inline-block" on:click={_=>window.print()}>Print Reports</button>
    </div>
    {#each chosenAccountNames as accountName}
        <h2 class="text-2xl font-semibold mt-6 mb-2">{accountName}</h2>
        <Report {csvData} {accountName} {startDateString} {endDateString} />
    {/each}
{:else if csvData}
    <h1 class="text-3xl font-bold text-center mt-8 mb-4 text-blue-700">TroopTrack Money Account Report Generator</h1>
    <AccountSelect {csvData} bind:chosenAccountNames />
    <DatePicker bind:startDateString bind:endDateString />
    <button class="bg-blue-600 text-white px-4 py-2 mt-4 rounded hover:bg-blue-700" on:click={runReport}>Run Report</button>
    <div class="mx-auto my-6 p-4 bg-gray-100 rounded shadow">
        <h2 class="text-xl font-semibold mb-2">CSV File Contents (first 10 rows)</h2>
        <CsvTable {csvData} rows={10} />
    </div>
{:else}
    <h1 class="text-3xl font-bold text-center mt-8 mb-4 text-blue-700">TroopTrack Money Account Report Generator</h1>
    <p>
        Visit your TroopTrack Money Book page and export the report as a CSV file to load it here.
    </p>
    <p>
        (All data is processed locally in your browser and not uploaded anywhere.)
    </p>
    <InputFile bind:csvData />
    <p>
        Source: <a class="underline text-blue-800" href="https://github.com/StevenClontz/trooptrack-report-generator">GitHub</a>
    </p>
{/if}

