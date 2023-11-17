<script lang="ts">
	import { TAILWIND_CONTEXT } from '$lib/utils/tailwind';
	import { setContext } from 'svelte';
	import type { TailwindConfig } from 'tw-to-css';

	export let config: TailwindConfig = {};
	setContext(TAILWIND_CONTEXT, config);
</script>

<slot />
