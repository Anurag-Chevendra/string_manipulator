<script lang="ts">
	import { type TManipulation, applyManipulations } from '$lib/manipulations';
	import ManipulationsPanel from './ManipulationsPanel.svelte';

	import TextArea from '$lib/components/TextArea.svelte';
	import Heading from '$lib/components/Heading.svelte';
	import Link from '$lib/components/Link.svelte';
	import { onMount } from 'svelte';

	let source = 'Try me!\nResult will be updated automatically';
	let result = '';
	let manipulations: TManipulation[] = [
		{
			type: 'replace',
			from: '\\n',
			to: ' -- ',
			id: 'initial'
		}
	];

	onMount(() => {
		document.addEventListener('keydown', (event) => {
			if (event.ctrlKey && event.key === 'Enter') {
				result = applyManipulations(source, manipulations);
			}
		});
	});

	$: result = applyManipulations(source, manipulations);
</script>

<div class="panels">
	<div class="panel">
		<Heading text="Source" />
		<TextArea id="source" bind:value={source} />
	</div>

	<div class="panel">
		<div class="header-with-link">
			<Heading text="Manipulations" />
			<Link link="/docs">Docs</Link>
		</div>
		<ManipulationsPanel bind:manipulations />
	</div>

	<div class="panel">
		<Heading text="Result" />
		<TextArea id="result" bind:value={result} />
	</div>
</div>

<style>
	.panels {
		display: grid;
		grid-template-columns: 1fr auto 1fr;
		gap: 10px;

		padding: 10px;
	}

	.panel {
		display: flex;
		flex-direction: column;
		gap: 10px;
	}

	.header-with-link {
		display: flex;
		flex-direction: row;
		gap: 10px;
		align-items: end;
	}
</style>
