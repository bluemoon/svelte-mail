<script lang="ts">
	import type {
		StandardLonghandProperties,
		StandardProperties,
		StandardShorthandProperties
	} from 'csstype';
	import { styleToString } from '$lib/utils';
	import type { HTMLAttributes } from 'svelte/elements';
	import { getContext, hasContext } from 'svelte';
	import { TAILWIND_CONTEXT } from '$lib/utils/tailwind';
	import { tailwindToCSS } from 'tw-to-css';

	interface $$Props extends Omit<HTMLAttributes<HTMLAnchorElement>, 'style'> {
		style?: StandardLonghandProperties & StandardShorthandProperties & StandardProperties;
		target?: string;
		href: string;
	}

	export let style: $$Props['style'] = {};
	let className: string | null | undefined = undefined;
	export { className as class };
	export let target = '_blank';
	export let href = '';

	let tailwindStyle = {};
	if (hasContext(TAILWIND_CONTEXT) && className) {
		const { twj } = tailwindToCSS({
			config: getContext(TAILWIND_CONTEXT)
		});

		tailwindStyle = twj(className);
		// we consumed it, no need to add it twice
		className = undefined;
	}

	const styleDefault = {
		color: '#067df7',
		textDecoration: 'none',
		...style,
		...tailwindStyle
	};
</script>

<a {...$$restProps} {href} {target} style={styleToString(styleDefault)} class={className}>
	<slot />
</a>
