<script lang="ts">
	interface ShinyTextProps {
		text: string;
		disabled?: boolean;
		speed?: number;
		className?: string;
	}
	let { text, disabled = false, speed = 5, className = '' }: ShinyTextProps = $props();

	const animationDuration = `${speed}s`;
</script>

<div
	class="text-lg bg-[#b5b5b5a4] text-transparent bg-clip-text inline-block {disabled
		? ''
		: 'animate-shine'} {className}"
	style="background-image: linear-gradient(120deg, rgba(255, 255, 255, 0) 40%, rgba(255, 255, 255, 0.8) 50%, rgba(255, 255, 255, 0) 60%);
    background-size: 200% 100%;
    -webkit-background-clip: text;
    animation-duration: {animationDuration};"
>
	{text}
</div>
