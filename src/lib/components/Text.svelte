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

	interface $$Props extends Omit<HTMLAttributes<HTMLParagraphElement>, 'style'> {
		style?: StandardLonghandProperties & StandardShorthandProperties & StandardProperties;
	}

	export let style: $$Props['style'] = {};
	let className: string | null | undefined = undefined;
	export { className as class };

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
		fontSize: '14px',
		lineHeight: '24px',
		margin: '16px 0',
		...style,
		...tailwindStyle
	};
</script>

<p style={styleToString(styleDefault)} {...$$restProps} class={className}>
	<slot />
</p>
