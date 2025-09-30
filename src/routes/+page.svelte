<script lang="ts">
    let csvFile: File | null = null;

    function handleFileUpload(event: Event) {
        const input = event.target as HTMLInputElement;
        if (input.files && input.files.length > 0) {
            csvFile = input.files[0];
        }
    }
</script>

<h1 class="text-3xl font-bold text-center mt-8 mb-4 text-blue-700">TroopTrack Money Account Report Generator</h1>
<div class="flex justify-center my-6">
    <label class="cursor-pointer bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700">
        Upload CSV
        <input type="file" accept=".csv" class="hidden" on:change={handleFileUpload} />
    </label>
</div>

{#if csvFile}
    <div class="max-w-2xl mx-auto my-6 p-4 bg-gray-100 rounded shadow">
        <h2 class="text-xl font-semibold mb-2">CSV File Contents</h2>
        <pre class="overflow-x-auto whitespace-pre-wrap">
            {#await csvFile.text() then text}
                {text}
            {/await}
        </pre>
    </div>
{/if}
