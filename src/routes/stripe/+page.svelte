<script lang="ts">
    import currency from "currency.js";
    let amount:string = "$0.00"
    const stripeAmount = (amount:string) => {
        return currency(amount).add(0.3).multiply(1/0.971).format()
    }
</script>

<input
    type="text"
    bind:value={amount}
    class="border border-gray-300 rounded px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
/>

<p>
    Amount to charge via Stripe: {stripeAmount(amount)}
</p>