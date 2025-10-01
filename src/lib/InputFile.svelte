<script lang="ts">
    import Papa from 'papaparse';
    let csvFile: File | null = null;
    export let csvData: Record<string, string>[] | null = null;

    function handleFileDrop(event: DragEvent) {
        event.preventDefault();
        if (event.dataTransfer && event.dataTransfer.files.length > 0) {
            csvFile = event.dataTransfer.files[0];
            Papa.parse(csvFile, {
                header: true,
                complete: function(results) {
                    csvData = results.data as Record<string, string>[];
                }
            });
        }
    }

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

<div class="flex justify-center my-6">
    <label 
        class="cursor-pointer bg-blue-100 text-black-800 px-4 py-2 rounded hover:bg-blue-200"
        on:drop={handleFileDrop}
        on:dragover={e=>e.preventDefault()}>
        Drag & Drop CSV file here, or
        <span class="underline text-blue-800">click to select</span>
        <input type="file" accept=".csv" class="sr-only" on:change={handleFileUpload}/>
    </label>
</div>

<style>
::file-selector-button {
  display: none;
}
input[type="file"] {
    font-size:0;
}
</style>