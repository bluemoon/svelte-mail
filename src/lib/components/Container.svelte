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

	interface $$Props extends Omit<HTMLAttributes<HTMLDivElement>, 'style'> {
		style?: StandardLonghandProperties & StandardProperties & StandardShorthandProperties;
	}

	export let style: $$Props['style'] = {};
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

	const styles = { maxWidth: '37.5em', ...style, ...tailwindStyle };
	const inlineStyle = styleToString(styles);
</script>

<div>
	{@html `<!--[if mso | IE]>
          <table role="presentation" width="100%" align="center" style="${inlineStyle}" class="${className}"><tr><td></td><td style="width:37.5em;background:#ffffff">
        <![endif]-->`}
</div>
<div {...$$restProps} style={inlineStyle} class={className}>
	<slot />
</div>
<div>
	{@html `<!--[if mso | IE]>
          </td><td></td></tr></table>
          <![endif]-->`}
</div>
