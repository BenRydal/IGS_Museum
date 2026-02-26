<script lang="ts">
	import { onMount } from 'svelte';
	import { appState, setFamily } from '$lib/stores/appState.svelte';

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
	const familyY = $derived(h / 12.75);          // zoomFamilyY
	const dotSize = 9;                              // mapButtonSize = 9

	// Family X positions — zoomFamilyX1..X4
	function familyX(id: number): number {
		if (id === 0) return w / 11.4;
		if (id === 1) return w / 3.34;
		if (id === 2) return w / 1.735;
		return w / 1.24;
	}

	const families = [
		{ id: 0, label: 'Bluegrass' },
		{ id: 1, label: 'Gayle' },
		{ id: 2, label: 'Business' },
		{ id: 3, label: 'Mom' }
	];
</script>

{#if appState.view === 'zoom'}
	{#each families as { id, label }}
		{@const cx = familyX(id)}
		<button
			class="fixed z-50 flex cursor-pointer items-center justify-center rounded-full border-2 p-0 transition-all duration-150
				{appState.family === id
				? 'border-gray-500 bg-gray-500'
				: 'border-gray-500 bg-transparent hover:bg-gray-300'}"
			style="
				left: {cx - dotSize / 2}px;
				top: {familyY - dotSize / 2}px;
				width: {dotSize}px;
				height: {dotSize}px;
			"
			title={label}
			onclick={() => setFamily(id)}
		>
		</button>
	{/each}
{/if}
