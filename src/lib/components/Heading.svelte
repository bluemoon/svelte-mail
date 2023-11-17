<script lang="ts">
	import type {
		StandardLonghandProperties,
		StandardProperties,
		StandardShorthandProperties
	} from 'csstype';
	import { styleToString, withMargin } from '$lib/utils';
	import type { HTMLAttributes } from 'svelte/elements';
	import { getContext, hasContext } from 'svelte';
	import { TAILWIND_CONTEXT } from '$lib/utils/tailwind';
	import { tailwindToCSS } from 'tw-to-css';
	interface $$Props extends Omit<HTMLAttributes<HTMLHeadingElement>, 'style'> {
		style?: StandardLonghandProperties & StandardProperties & StandardShorthandProperties;
		as?: 'h1' | 'h2' | 'h3' | 'h4' | 'h5' | 'h6';
		m?: string;
		mx?: string;
		my?: string;
		mt?: string;
		mr?: string;
		mb?: string;
		ml?: string;
	}

	let className: string | undefined = undefined;
	export { className as class };

	let tailwindStyle = {};
	if (hasContext(TAILWIND_CONTEXT) && className) {
		const { twj } = tailwindToCSS({
			config: getContext(TAILWIND_CONTEXT)
		});

		tailwindStyle = twj(className);
		className = undefined;
	}

	export let style: $$Props['style'] = {};
	export let as = 'h1';
</script>

<svelte:element
	this={as}
	style={styleToString({
		...withMargin({
			m: $$props.m,
			mx: $$props.mx,
			my: $$props.my,
			mt: $$props.mt,
			mr: $$props.mr,
			mb: $$props.mb,
			ml: $$props.ml
		}),
		...style,
		...tailwindStyle
	})}
	class={className}
	{...$$restProps}
>
	<slot />
</svelte:element>
