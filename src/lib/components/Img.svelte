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

	interface $$Props extends Omit<HTMLAttributes<HTMLImageElement>, 'style'> {
		style?: StandardProperties & StandardLonghandProperties & StandardShorthandProperties;
		alt: string;
		src: string;
		width: string;
		height: string;
	}

	export let style: $$Props['style'] = {};
	let className: string | null | undefined = undefined;
	export { className as class };
	export let alt = '';
	export let src = '';
	export let width = '0';
	export let height = '0';

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
		display: 'block',
		outline: 'none',
		border: 'none',
		textDecoration: 'none',
		...style,
		...tailwindStyle
	};
</script>

<img
	{alt}
	{src}
	{width}
	{height}
	style={styleToString(styleDefault)}
	{...$$restProps}
	class={className}
/>
