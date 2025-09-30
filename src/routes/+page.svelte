<script lang="ts">
    import Papa from 'papaparse';
    let csvFile: File | null = null;
    let csvData: Record<string, string>[] | null = null;
    const KEYS = [
        "Account name",
        "Activity on",
        "Value",
        "Description",
        "Details",
        "Balance",
    ]

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

    function getAccountNames(csvData:Record<string, string>[] | null): string[] {
        if (!csvData) return [];
        const names = new Set<string>();
        for (const row of csvData) {
            if (row["Account name"]) {
                names.add(row["Account name"]);
            }
        }
        return Array.from(names).sort();
    }

    let chosenAccountNames: string[] = [];
</script>

<h1 class="text-3xl font-bold text-center mt-8 mb-4 text-blue-700">TroopTrack Money Account Report Generator</h1>

{#if csvData}
    {#if getAccountNames(csvData)?.length > 1}
        <label class="block mb-2">
            <span class="font-semibold mr-2 block">Select account(s):</span>
            <select multiple bind:value={chosenAccountNames} class="border rounded px-2 py-1 resize-y">
            {#each getAccountNames(csvData) as name}
                <option value={name}>{name}</option>
            {/each}
            </select>
        </label>
    {/if}
    <div class="mx-auto my-6 p-4 bg-gray-100 rounded shadow">
        <h2 class="text-xl font-semibold mb-2">CSV File Contents</h2>
        <table>
            <thead>
                <tr>
                    {#if csvData.length > 0}
                        {#each KEYS as header}
                            <th class="border px-2 py-1 bg-gray-200">{header}</th>
                        {/each}
                    {/if}
                </tr>
            </thead>
            <tbody>
                {#each csvData.slice(0, 10) as row}
                    <tr>
                        {#each KEYS as key}
                            <td class="border px-2 py-1">{row[key]}</td>
                        {/each}
                    </tr>
                {/each}
            </tbody>
        </table>
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
