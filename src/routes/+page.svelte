<script lang="ts">
    import Papa from 'papaparse';
    import AccountSelect from '$lib/AccountSelect.svelte';
    import CsvTable from '$lib/CsvTable.svelte';
    import DatePicker from '$lib/DatePicker.svelte';
    let csvFile: File | null = null;
    let csvData: Record<string, string>[] | null = null;
    let chosenAccountNames: string[] = [];
    let startDate: Date = new Date();
    let endDate: Date = new Date();

    function handleFileUpload(event: Event) {
        const input = event.target as HTMLInputElement;
        if (input.files && input.files.length > 0) {
            csvFile = input.files[0];
            Papa.parse(csvFile, {
                header: true,
                complete: function(results) {
                    csvData = results.data as Record<string, string>[];
                }
            });
        }
    }
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
    <div class="flex justify-center my-6">
        <label class="cursor-pointer bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700">
            Upload CSV
            <input type="file" accept=".csv" class="hidden" on:change={handleFileUpload} />
        </label>
    </div>
{/if}
