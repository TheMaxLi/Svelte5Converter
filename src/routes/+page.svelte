<script lang="ts">
	let value = $state(``);

	let code = $derived.by(() => {
		let lines = value.split('\n');

		lines = lines.map((l) => {
			let newLine = l
				.replace('\t', '')
				.replace('export', '')
				.replace('let', '')
				.replace('=', ':')
				.replace('false', 'boolean')
				.replace("''", 'string')
				.replace('""', 'string');

			if (
				newLine
					.split(':')
					.some((char, index) => newLine.indexOf(char) !== newLine.lastIndexOf(char))
			) {
				newLine = newLine.slice(0, newLine.lastIndexOf(':'));
			}
			return newLine;
		});

		return `type Props = { \n${lines.join('\n')} \n}`;
	});

	let props = $derived.by(() => {
		let lines = value.split('\n');

		lines = lines.map((l) =>
			l
				.replace('\t', '')
				.replace('export', '')
				.replace('let', '')
				.replace('=', ':')
				.replace('false', 'boolean')
				.replace("''", 'string')
				.replace('""', 'string')
				.slice(0, l.indexOf(':'))
		);
		return `let {${lines.map((l) => l.slice(0, l.indexOf(':')))}} : Props = $props()`;
	});
</script>

<section class="flex justify-between flex-row w-full">
	<textarea bind:value class="bg-black text-white w-xl h-[500px]"></textarea>
	<button class="w-[250px]">
		<img
			src="https://media.istockphoto.com/id/1296789917/photo/red-arrow-on-white-background-isolated-3d-illustration.jpg?s=612x612&w=0&k=20&c=VDZKzBETbyiAu0j_VuVQnZ4VCTiESX7m_AW4w-sUG9U="
			alt=""
		/>
	</button>
	<div>
		<pre class="bg-black w-5xl text-white aspect-video">{code} <br /> <br />{props} </pre>
	</div>
</section>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<style>
	section {
		display: flex;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}
</style>
