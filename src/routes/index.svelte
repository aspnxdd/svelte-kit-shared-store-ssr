<script context="module" lang="ts">
	import type { Load } from '@sveltejs/kit';
	import { get } from 'svelte/store';
	import { count, dateNow } from '../lib/store';

	// we need hydrate to be false for this to work
	export const hydrate = false;

	export const load: Load = async ({ fetch }) => {
		const FIVE_SEC_MS = 5 * 1000;
		setInterval(() => {
			if (Date.now() - get(dateNow) > FIVE_SEC_MS) {
				count.update((n) => n + 1);
				dateNow.update(Date.now);
			}
		}, FIVE_SEC_MS);

		return {};
	};
</script>

<script lang="ts">
	let countValue: number = 0;

	count.subscribe((value) => {
		countValue = value;
	});
</script>

<main>
	<span>
		{countValue}
	</span>
</main>

<style>
	main {
		display: flex;
		justify-content: center;
		align-items: center;
	}

	span {
		font-size: 15rem;
	}


</style>
