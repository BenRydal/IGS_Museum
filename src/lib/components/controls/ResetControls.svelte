<script lang="ts">
	import { appState, resetAll, toggleGrayScale } from '$lib/stores/appState.svelte';
</script>

<!-- Reset button -->
<button
	class="flex h-7 w-7 cursor-pointer items-center justify-center rounded-full border-1.5 border-gray-400 bg-transparent text-gray-500 transition-colors duration-150 hover:border-gray-600 hover:text-gray-700"
	onclick={() => resetAll()}
	aria-label="Reset"
>
	<!-- Reset/refresh icon (circular arrow) -->
	<svg width="13" height="13" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
		<path d="M1 1v5h5" />
		<path d="M3.5 10a6 6 0 1 0 1.2-6.2L1 6" />
	</svg>
</button>

<!-- Show/Hide Paths toggle (talk mode only) -->
{#if appState.mode === 'talk'}
	<button
		class="flex h-7 w-7 cursor-pointer items-center justify-center rounded-full transition-colors duration-150
			{appState.grayScaleToggle
			? 'border-1.5 border-gray-700 bg-gray-800 text-white'
			: 'border-1.5 border-gray-400 bg-transparent text-gray-500 hover:border-gray-600 hover:text-gray-700'}"
		onclick={() => toggleGrayScale()}
		aria-label={appState.grayScaleToggle ? 'Hide paths' : 'Show paths'}
	>
		<!-- Path/route icon -->
		<svg width="13" height="13" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
			<path d="M3 14c0-3 4-3 4-6s-4-3-4-6" />
			<path d="M13 14c0-3-4-3-4-6s4-3 4-6" />
		</svg>
	</button>
{/if}
