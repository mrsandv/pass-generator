<script lang="ts">
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';
	import MoonIcon from '../components/icons/moon.svelte';
	import SunIcon from '../components/icons/sun.svelte';

	let { children } = $props();
	let darkTheme: Boolean = $state(true);
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<header class="">
	<nav class="flex h-[15vh] w-full items-center justify-between bg-zinc-950 p-4">
		<img src="/logo.png" alt="Logo" class="h-30" />
		<button class="text-black hover:text-blue-500" onclick={() => (darkTheme = !darkTheme)}>
			{#if darkTheme}
				<MoonIcon />
			{:else}
				<SunIcon />
			{/if}
		</button>
	</nav>
</header>

<main class="flex h-[calc(100vh-192px)] items-center justify-center">
	<div class="h-2/3 w-full md:w-1/2 lg:w-1/3">
		{@render children?.()}
	</div>
</main>

<footer class="h-24 bg-gray-100 p-2 text-center">
	<p>© {new Date().getFullYear()}</p>
</footer>
