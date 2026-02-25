<script lang="ts">
	import { onMount } from 'svelte';
	import WelcomeOverlay from '$lib/components/welcome/WelcomeOverlay.svelte';

	let showWelcome = $state(true);

	onMount(() => {
		(window as any)._igsSvelteWelcome = true;

		const handler = (e: Event) => {
			showWelcome = (e as CustomEvent).detail.visible;
		};
		document.addEventListener('igsWelcomeToggle', handler);
		return () => document.removeEventListener('igsWelcomeToggle', handler);
	});

	function dismiss() {
		showWelcome = false;
		(window as any)._igsWelcomeDismiss();
	}
</script>

<WelcomeOverlay visible={showWelcome} ondismiss={dismiss} />
