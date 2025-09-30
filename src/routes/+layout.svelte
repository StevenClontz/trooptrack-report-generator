<script lang="ts">
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';
	
	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<title>TroopTrack Money Account Report Generator</title>
</svelte:head>

<main class="min-h-screen bg-gray-50 flex flex-col items-center justify-center p-4">
	{@render children?.()}
</main>
