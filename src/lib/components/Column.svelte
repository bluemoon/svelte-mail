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

	interface $$Props extends Omit<HTMLAttributes<HTMLTableCellElement>, 'style'> {
		style?: StandardLonghandProperties & StandardProperties & StandardShorthandProperties;
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
		className = undefined;
	}

	const styleDefault = {
		display: 'inline-flex',
		justifyContent: 'center',
		alignItems: 'center',
		...style,
		...tailwindStyle
	};
</script>

<td style={styleToString(styleDefault)} role="presentation" {...$$restProps} class={className}>
	<slot />
</td>
