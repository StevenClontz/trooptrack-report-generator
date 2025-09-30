<script lang="ts">
    export let csvData: Record<string, string>[] | null = null;
    export let chosenAccountNames: string[] = [];

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
</script>

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