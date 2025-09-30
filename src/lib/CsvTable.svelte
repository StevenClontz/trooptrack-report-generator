<script lang="ts">
    export let csvData: any[] = [];
    const KEYS = [
        "Account name",
        "Activity on",
        "Value",
        "Description",
        "Details",
        "Balance",
    ]
</script>

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