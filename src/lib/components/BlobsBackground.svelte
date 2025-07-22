<script lang="ts">
	import { onMount } from 'svelte';
	import { Spring } from 'svelte/motion';
	import { browser } from '$app/environment';

	let {
		settings = {
			blobCount: 5,
			colors: ['bg-pink-400', 'bg-purple-400', 'bg-blue-400'],
			size: 800,
			movement: 'attract', // or 'repel'
			strength: 0.03,
			blur: 'blur-3xl',
			opacity: 'opacity-70',
			stiffness: 0.1,
			damping: 0.25,
			maxDistance: 200,
			minDistance: 50,
			minBlobSpacing: 300
		}
	} = $props();

	let blobs = $state<
		{ id: number; x: Spring<number>; y: Spring<number>; origin: { x: number; y: number } }[]
	>([]);

	let mouse = new Spring({ x: 0, y: 0 }, { stiffness: 0.2, damping: 0.3 });

	let hasMoved = false;

	onMount(() => {
		if (!browser) return;

		const positions = generateSpacedBlobs(settings.blobCount, settings.minBlobSpacing ?? 300);

		blobs = positions.map((pos, i) => ({
			id: i,
			x: new Spring(pos.x, { stiffness: settings.stiffness, damping: settings.damping }),
			y: new Spring(pos.y, { stiffness: settings.stiffness, damping: settings.damping }),
			origin: pos
		}));

		let idleTimeout: ReturnType<typeof setTimeout> | null = null;
		let isIdle = false;
		let mouseX = window.innerWidth / 2;
		let mouseY = window.innerHeight / 2;

		const handleMouseMove = (e: MouseEvent) => {
			mouseX = e.clientX;
			mouseY = e.clientY;
			mouse.target = { x: mouseX, y: mouseY };
			hasMoved = true;

			// Reset idle status
			if (idleTimeout) clearTimeout(idleTimeout);
			isIdle = false;

			idleTimeout = setTimeout(() => {
				isIdle = true;
			}, 300);
		};

		window.addEventListener('mousemove', handleMouseMove);

		// Start animation loop
		const loop = () => {
			for (const blob of blobs) {
				let targetX: number;
				let targetY: number;
				if (hasMoved) {
					if (isIdle) {
						// Return to origin (before: direct snap)
						const dx = blob.origin.x - blob.x.current;
						const dy = blob.origin.y - blob.y.current;
						const dist = Math.sqrt(dx * dx + dy * dy);
						const angle = Math.atan2(dy, dx);

						targetX = blob.x.current + Math.cos(angle) * dist * settings.strength;
						targetY = blob.y.current + Math.sin(angle) * dist * settings.strength;
					} else {
						// Move toward or away from cursor
						const dx = mouseX - blob.x.current;
						const dy = mouseY - blob.y.current;
						const dist = Math.sqrt(dx * dx + dy * dy);
						const clampedDist = Math.min(
							Math.max(dist, settings.minDistance ?? 50, 0),
							settings.maxDistance ?? 200
						);
						const angle = Math.atan2(dy, dx);
						const offset = settings.movement === 'repel' ? -1 : 1;

						targetX = blob.x.current + Math.cos(angle) * clampedDist * settings.strength * offset;
						targetY = blob.y.current + Math.sin(angle) * clampedDist * settings.strength * offset;
					}

					// Clamp within screen
					blob.x.target = Math.min(Math.max(targetX, 0), window.innerWidth);
					blob.y.target = Math.min(Math.max(targetY, 0), window.innerHeight);
				}
			}

			requestAnimationFrame(loop);
		};
		loop();

		return () => {
			window.removeEventListener('mousemove', handleMouseMove);
		};
	});

	function generateSpacedBlobs(count: number, spacing: number) {
		const positions: { x: number; y: number }[] = [];

		while (positions.length < count) {
			const candidate = {
				x: Math.random() * window.innerWidth,
				y: Math.random() * window.innerHeight
			};

			let tooClose = false;
			for (const pos of positions) {
				const dx = pos.x - candidate.x;
				const dy = pos.y - candidate.y;
				const dist = Math.sqrt(dx * dx + dy * dy);
				if (dist < spacing) {
					tooClose = true;
					break;
				}
			}

			if (!tooClose) positions.push(candidate);
		}

		return positions;
	}
</script>

<div class="absolute inset-0 -z-10 overflow-hidden">
	{#each blobs as blob, i}
		<div
			class={`absolute rounded-full mix-blend-multiply transition-all duration-300 ease-out ${settings.colors[i % settings.colors.length]} ${settings.opacity} ${settings.blur}`}
			style:transform={`translate(calc(${blob.x.current}px - 50%), calc(${blob.y.current}px - 50%))`}
			style:width={`${settings.size}px`}
			style:height={`${settings.size}px`}
		></div>
	{/each}
</div>
