<script lang="ts">
	let input = $state(`export let id: string
export let type: 'button' | 'submit' | 'reset' | null | undefined = 'button'
export let body = 'Are you sure you want to remove this record?'
export let hoverEffect = false
export let config = { mode: 'dev' }
export let custom: MyCustomType
export let values: string[] = []`);

	function inferTypeFromDefault(defaultVal: string): string {
		if (/^(['"]).*\1$/.test(defaultVal)) return 'string';
		if (/^\d+(\.\d+)?$/.test(defaultVal)) return 'number';
		if (/^(true|false)$/.test(defaultVal)) return 'boolean';
		if (/^\[.*\]$/.test(defaultVal)) return 'any[]';
		if (/^\{.*\}$/.test(defaultVal)) return 'Record<string, any>';
		return 'any';
	}

	const output = $derived.by(() => {
		const lines = input
			.split('\n')
			.map((l) => l.trim())
			.filter(Boolean);

		let propsType: string[] = [];
		let destructured: string[] = [];

		for (const line of lines) {
			const match = line.match(/^export let (\w+)(?::\s*([^=]+))?(?:\s*=\s*(.+))?$/);
			if (!match) continue;

			const [, name, explicitType, defaultValRaw] = match;
			const defaultVal = defaultValRaw?.trim();
			let inferredType = 'any';

			if (explicitType) {
				// Custom or known type
				if (defaultVal !== undefined) {
					propsType.push(`\t${name}?: ${explicitType}`);
					destructured.push(`\t${name} = ${defaultVal}`);
				} else {
					propsType.push(`\t${name}: ${explicitType}`);
					destructured.push(`\t${name}`);
				}
			} else if (defaultVal !== undefined) {
				inferredType = inferTypeFromDefault(defaultVal);
				propsType.push(`\t${name}?: ${inferredType}`);
				destructured.push(`\t${name} = ${defaultVal}`);
			} else {
				propsType.push(`\t${name}: any`);
				destructured.push(`\t${name}`);
			}
		}

		return `type Props = {\n${propsType.join('\n')}\n}\n\nlet {\n${destructured.join(',\n')},\n}: Props = $props()`;
	});
</script>

<div class="p-6 space-y-4 max-w-4xl mx-auto">
	<h2 class="text-xl font-semibold">Svelte 5 Prop Converter</h2>

	<textarea class="w-full h-64 p-3 rounded border border-gray-300 font-mono" bind:value={input}
	></textarea>

	<h3 class="font-medium mt-4">Converted Output</h3>
	<pre
		class="bg-gray-100 dark:bg-neutral-900 text-sm p-4 rounded whitespace-pre-wrap border border-gray-300">
		{output}
	</pre>
</div>
