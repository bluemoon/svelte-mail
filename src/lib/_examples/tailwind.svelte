<script>
	import { Button, Hr, Html, Text, Tailwind } from '$lib';

	export let name = 'World';
</script>

<Tailwind>
	<Html lang="en">
		<Text class="text-xl leading-10">
			Hello, {name}!
		</Text>

		<Hr />

		<Button href="https://svelte.dev">Visit Svelte</Button>
	</Html>
</Tailwind>
