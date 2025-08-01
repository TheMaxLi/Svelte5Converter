<script lang="ts">
	let text = 'INDUSTRY MEDIA GROUP · INDUSTRY MEDIA GROUP · ';
</script>

<div class="w-[100vw] h-[787px] bg-black overflow-hidden relative">
	<div
		class="flex justify-between pt-14 px-15 text-3xl font-normal font-['Neue_Haas_Grotesk_Text_Pro']"
	>
		<div class=" text-neutral-500">©2025. Industry Media Group</div>
		<div class="flex gap-24">
			<div class="w-60 flex flex-col gap-5">
				<p class="text-neutral-500">Contact</p>
				<div class="text-white flex flex-col gap-5">
					<a href="#" class="hover:text-white">Email</a>
					<a href="#" class="hover:text-white">Call Us</a>
					<a href="#" class="hover:text-white">Make a Request</a>
				</div>
			</div>
			<div class="w-60 flex flex-col gap-5">
				<p class="text-neutral-500">Contact</p>
				<div class="text-white flex flex-col gap-5">
					<a href="#" class="hover:text-white">Legal</a>
					<a href="#" class="hover:text-white">Cookie Policy</a>
					<a href="#" class="hover:text-white">Privacy Policy</a>
					<a href="#" class="hover:text-white">Terms of Service</a>
				</div>
			</div>
			<div class="w-60 flex flex-col gap-5">
				<p class="text-neutral-500">Social</p>
				<div class="text-white flex flex-col gap-5">
					<a href="#" class="hover:text-white">Instagram</a>
					<a href="#" class="hover:text-white">Youtube</a>
					<a href="#" class="hover:text-white">LinkedIn</a>
				</div>
			</div>
		</div>
	</div>
	<div
		class="left-0 top-[547px] absolute justify-center whitespace-nowrap
        animate-scroll font-extrabold text-[22vw] leading-none text-white
        font-['Neue_Haas_Grotesk_Text_Pro']"
	>
		{@html text.repeat(10)}
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
