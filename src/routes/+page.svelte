<script lang="ts">
	import { onMount } from 'svelte';
	import { appState, setupBridge, syncState, setWelcome } from '$lib/stores/appState.svelte';
	import WelcomeOverlay from '$lib/components/welcome/WelcomeOverlay.svelte';
	import HeaderOverlay from '$lib/components/controls/HeaderOverlay.svelte';
	import ModeSelector from '$lib/components/controls/ModeSelector.svelte';

	import AnimationToggle from '$lib/components/controls/AnimationToggle.svelte';
	import AboutButton from '$lib/components/controls/AboutButton.svelte';
	import ViewToggle from '$lib/components/controls/ViewToggle.svelte';
	import ResetControls from '$lib/components/controls/ResetControls.svelte';
	import ZoomButtons from '$lib/components/controls/ZoomButtons.svelte';

	onMount(() => {
		setupBridge();

		// Listen for p5 welcome toggle events (legacy bridge)
		const welcomeHandler = (e: Event) => {
			const visible = (e as CustomEvent).detail.visible;
			setWelcome(visible);
		};
		document.addEventListener('igsWelcomeToggle', welcomeHandler);

		return () => {
			document.removeEventListener('igsWelcomeToggle', welcomeHandler);
		};
	});

	// Keep window bridge in sync whenever reactive state changes
	$effect(() => {
		void appState.view;
		void appState.mode;
		void appState.space;
		void appState.family;
		void appState.individuals;
		void appState.animate;
		void appState.welcome;
		void appState.grayScaleToggle;
		syncState();
	});

	function dismissWelcome() {
		setWelcome(false);
		(window as any)._igsWelcomeDismiss();
	}
</script>

<!-- Header overlay: family headers, individual names, gallery labels, grid lines -->
<HeaderOverlay />

<!-- Top-center: mode selector + view all -->
<div class="pointer-events-auto fixed top-2 left-1/2 z-50 flex -translate-x-1/2 items-center gap-2">
	<ModeSelector />
	{#if appState.view === 'zoom'}
		<ViewToggle />
	{/if}
</div>

<!-- Top-right icons: reset (small multiple only) + animation + about -->
<div class="pointer-events-auto fixed top-2 right-3 z-50 flex items-center gap-2">
	{#if appState.view === 'smallMultiple'}
		<ResetControls />
	{/if}
	<AnimationToggle />
	<AboutButton />
</div>

<!-- Zoom buttons (small multiple view) -->
<ZoomButtons />

<!-- Welcome overlay -->
<WelcomeOverlay visible={appState.welcome} ondismiss={dismissWelcome} />
