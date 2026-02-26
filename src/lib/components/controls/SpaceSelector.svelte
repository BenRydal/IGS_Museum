<script lang="ts">
	import { onMount } from 'svelte';
	import { appState, setSpace } from '$lib/stores/appState.svelte';

	let w = $state(0);
	let h = $state(0);

	onMount(() => {
		w = window.innerWidth;
		h = window.innerHeight;
		const onResize = () => {
			w = window.innerWidth;
			h = window.innerHeight;
		};
		window.addEventListener('resize', onResize);
		return () => window.removeEventListener('resize', onResize);
	});

	// Matches positionButtons() in main.js exactly
	const spaceX = $derived(w / 110);              // zoomSpaceX
	const dotSize = 9;                              // mapButtonSize = 9

	// Space Y positions — zoomSpaceY1..Y3
	function spaceY(id: number): number {
		if (id === 0) return h / 3;       // zoomSpaceY1
		if (id === 1) return h / 1.59;    // zoomSpaceY2
		return h / 1.1;                    // zoomSpaceY3
	}

	const spaces = [
		{ id: 0, label: 'Walkway' },
		{ id: 1, label: 'Bluegrass' },
		{ id: 2, label: 'Rotunda' }
	];
</script>

{#if appState.view === 'zoom'}
	{#each spaces as { id, label }}
		{@const cy = spaceY(id)}
		<button
			class="fixed z-50 flex cursor-pointer items-center justify-center rounded-full border-2 p-0 transition-all duration-150
				{appState.space === id
				? 'border-gray-500 bg-gray-500'
				: 'border-gray-500 bg-transparent hover:bg-gray-300'}"
			style="
				left: {spaceX - dotSize / 2}px;
				top: {cy - dotSize / 2}px;
				width: {dotSize}px;
				height: {dotSize}px;
			"
			title={label}
			onclick={() => setSpace(id)}
		>
		</button>
	{/each}
{/if}
