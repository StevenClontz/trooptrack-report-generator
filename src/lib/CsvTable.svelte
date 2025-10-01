<script lang="ts">
    import currency from "currency.js";
    import { KEYS } from "./constants";
    export let csvData: any[] = [];
    export let rows = 10;
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
        {#each csvData.slice(0, rows) as row}
            <tr>
                {#each KEYS as key}
                    <td class="border px-2 py-1">
                        {#if key === "Value" || key === "Balance"}
                            {currency(row[key]).format()}
                        {:else}
                            {row[key]}
                        {/if}
                    </td>
                {/each}
            </tr>
        {/each}
    </tbody>
</table>
