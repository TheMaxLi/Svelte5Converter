<script lang="ts">
	import ShinyText from '$lib/components/ShinyText.svelte';

	let text = $state(`This is a shiny text component!`);
</script>

<div class="p-6 space-y-4 max-w-4xl mx-auto">
	<h2 class="text-xl font-semibold">Shiny Text Component</h2>
	<textarea bind:value={text} class="bg-black text-white w-full h-[100px]"></textarea>
	<ShinyText {text} speed={3} className="text-4xl" />
</div>
