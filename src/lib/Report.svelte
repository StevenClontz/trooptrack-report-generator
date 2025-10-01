<script lang="ts">
    import currency from "currency.js";
    import { KEYS } from "./constants";
    export let accountName:string
    export let startDateString: string;
    export let endDateString: string;
    let startDate = new Date(`${startDateString}T00:00:00`);
    let endDate = new Date(`${endDateString}T00:00:00`);
    export let csvData: Record<string, string>[] | null = null;
    let reportData = csvData?.filter(row => {
        return row["Account name"] === accountName && new Date(`${row["Activity on"]}T00:00:00`) >= startDate && new Date(`${row["Activity on"]}T00:00:00`) <= endDate;
    }).reverse();
    // Calculate starting balance: sum of "Value" before startDate
    let startingBalance = 0;
    if (csvData) {
        startingBalance = csvData
            .filter(row => row["Account name"] === accountName && new Date(`${row["Activity on"]}T00:00:00`) < startDate)
            .reduce((sum, row) => sum + currency(row["Value"]).value, 0);
    }
    // Calculate ending balance: sum of "Value" before endDate
    let endingBalance = startingBalance;
    if (csvData) {
        endingBalance = csvData
            .filter(row => row["Account name"] === accountName && new Date(`${row["Activity on"]}T00:00:00`) <= endDate)
            .reduce((sum, row) => sum + currency(row["Value"]).value, 0);
    }
</script>

<div style="break-after: page;">
{#if reportData && reportData.length > 0}
<p>
    Starting balance on {startDate.toLocaleDateString()}: {currency(startingBalance).format()}
</p>
<p>
    Ending balance on {endDate.toLocaleDateString()}: {currency(endingBalance).format()}
</p>
<table>
    <thead>
        <tr>
            {#each KEYS as header}
                <th class="border px-2 py-1 bg-gray-200">{header}</th>
            {/each}
        </tr>
    </thead>
    <tbody>
        {#each reportData as row}
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
{:else}
    <p>No transactions found between {startDate.toLocaleDateString()} and {endDate.toLocaleDateString()}.</p>
{/if}
</div>