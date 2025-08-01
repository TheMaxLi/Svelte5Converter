<script>
	let text = 'INDUSTRY MEDIA GROUP · INDUSTRY MEDIA GROUP · ';
</script>

<div class="bg-black text-white px-6 py-8">
	<div
		class="max-w-screen-xl mx-auto grid grid-cols-2 md:grid-cols-4 gap-8 text-sm text-neutral-400"
	>
		<div class="col-span-2 md:col-span-1">
			<p>©2025. Industry Media Group</p>
		</div>

		<div>
			<p class="font-semibold text-neutral-500 mb-2">Contact</p>
			<ul class="space-y-1">
				<li><a href="#" class="hover:text-white">Email</a></li>
				<li><a href="#" class="hover:text-white">Call Us</a></li>
				<li><a href="#" class="hover:text-white">Make a Request</a></li>
			</ul>
		</div>

		<div>
			<p class="font-semibold text-neutral-500 mb-2">Legal</p>
			<ul class="space-y-1">
				<li><a href="#" class="hover:text-white">Cookie Policy</a></li>
				<li><a href="#" class="hover:text-white">Privacy Policy</a></li>
				<li><a href="#" class="hover:text-white">Terms of Service</a></li>
			</ul>
		</div>

		<div>
			<p class="font-semibold text-neutral-500 mb-2">Social</p>
			<ul class="space-y-1">
				<li><a href="#" class="hover:text-white">Instagram</a></li>
				<li><a href="#" class="hover:text-white">Youtube</a></li>
				<li><a href="#" class="hover:text-white">LinkedIn</a></li>
			</ul>
		</div>
	</div>

	<div class="overflow-hidden mt-20">
		<div
			class="whitespace-nowrap animate-scroll font-extrabold text-[22vw] leading-none text-white"
		>
			{@html text.repeat(10)}
		</div>
	</div>
</div>

<style>
	@keyframes scroll {
		0% {
			transform: translateX(0);
		}
		100% {
			transform: translateX(-50%);
		}
	}

	.animate-scroll {
		display: inline-block;
		animation: scroll 280s linear infinite;
	}
</style>
