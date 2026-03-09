<script lang="ts">
	import { onMount } from 'svelte';

	let visible = $state(false);
	let progress = $state(0);
	let el: HTMLElement;
	let pathEl: SVGPathElement;
	let pathLength = $state(0);

	const waypoints = [
		{ title: 'Install', desc: 'Add the skill' },
		{ title: 'Scan', desc: 'Analyze auth' },
		{ title: 'Create', desc: 'Build shortcuts' },
		{ title: 'Detect', desc: 'Find browsers' },
		{ title: 'Document', desc: 'Update agents' },
		{ title: 'Report', desc: 'Summarize' },
		{ title: 'Self-destruct', desc: 'Remove traces' },
	];

	// Waypoint positions (matching SVG path at key points)
	const positions = [
		{ x: 80, y: 50 },
		{ x: 310, y: 50 },
		{ x: 540, y: 50 },
		{ x: 540, y: 180 },
		{ x: 310, y: 180 },
		{ x: 80, y: 180 },
		{ x: 80, y: 310 },
	];

	onMount(() => {
		if (pathEl) {
			pathLength = pathEl.getTotalLength();
		}

		const observer = new IntersectionObserver(
			([entry]) => {
				if (entry.isIntersecting) {
					visible = true;
					observer.disconnect();
					animatePath();
				}
			},
			{ threshold: 0.15 }
		);
		observer.observe(el);
		return () => observer.disconnect();
	});

	function animatePath() {
		const duration = 2400;
		const start = performance.now();

		function tick(now: number) {
			const elapsed = now - start;
			progress = Math.min(elapsed / duration, 1);
			if (progress < 1) {
				requestAnimationFrame(tick);
			}
		}
		requestAnimationFrame(tick);
	}

	// Which waypoints are "reached" based on progress
	function isReached(index: number): boolean {
		return progress > index / (waypoints.length - 1) * 0.85;
	}
</script>

<section class="operation" bind:this={el}>
	<div class="inner">
		<div class="section-line"></div>
		<div class="exhibit">OPERATION BRIEFING</div>
		<h2 class:visible>Mission Route</h2>

		<div class="map" class:visible>
			<svg viewBox="0 0 620 360" preserveAspectRatio="xMidYMid meet">
				<!-- Background path (full route, faint) -->
				<path
					class="route-bg"
					d="M 80,50 H 540 Q 590,50 590,100 V 130 Q 590,180 540,180 H 80 Q 30,180 30,230 V 260 Q 30,310 80,310"
					fill="none"
				/>
				<!-- Animated path (draws on scroll) -->
				<path
					bind:this={pathEl}
					class="route-line"
					d="M 80,50 H 540 Q 590,50 590,100 V 130 Q 590,180 540,180 H 80 Q 30,180 30,230 V 260 Q 30,310 80,310"
					fill="none"
					style:stroke-dasharray={pathLength}
					style:stroke-dashoffset={pathLength * (1 - progress)}
				/>

				<!-- Waypoint circles -->
				{#each positions as pos, i}
					<circle
						cx={pos.x}
						cy={pos.y}
						r={i === 6 ? 8 : 6}
						class="pip"
						class:active={isReached(i)}
						class:burn={i === 6 && isReached(i)}
					/>
					<!-- Step number -->
					<text
						x={pos.x}
						y={pos.y}
						class="step-num"
						class:active={isReached(i)}
					>
						{String(i + 1).padStart(2, '0')}
					</text>
				{/each}
			</svg>

			<!-- Labels positioned over the SVG -->
			<div class="labels">
				{#each waypoints as wp, i}
					<div
						class="label"
						class:active={isReached(i)}
						class:burn={i === 6}
						style="--lx: {positions[i].x}; --ly: {positions[i].y}"
					>
						<span class="wp-title">{wp.title}</span>
						<span class="wp-desc">{wp.desc}</span>
					</div>
				{/each}
			</div>
		</div>
	</div>
</section>

<style>
	.operation {
		padding: 48px var(--pad) 72px;
	}

	.inner {
		max-width: var(--col);
		margin: 0 auto;
	}

	.section-line {
		width: 40px;
		height: 1px;
		background: var(--paper-border);
		margin-bottom: 24px;
	}

	.exhibit {
		font-family: var(--font-mono);
		font-size: 11px;
		letter-spacing: 0.15em;
		color: var(--stamp-red);
		opacity: 0.6;
		margin-bottom: 16px;
	}

	h2 {
		font-family: var(--font-serif);
		font-size: clamp(1.6rem, 3.5vw, 2.2rem);
		color: var(--ink);
		margin-bottom: 32px;
		opacity: 0;
		transform: translateY(12px);
		transition: opacity 0.6s, transform 0.6s;
	}

	h2.visible {
		opacity: 1;
		transform: translateY(0);
	}

	/* Map container */
	.map {
		position: relative;
		opacity: 0;
		transition: opacity 0.6s 0.2s;
	}

	.map.visible {
		opacity: 1;
	}

	svg {
		width: 100%;
		height: auto;
		display: block;
	}

	/* Background path — the full route faintly visible */
	.route-bg {
		stroke: var(--paper-border);
		stroke-width: 2;
		stroke-dasharray: 6 4;
	}

	/* Animated path — draws on as you watch */
	.route-line {
		stroke: var(--ink-muted);
		stroke-width: 2.5;
		stroke-linecap: round;
		transition: stroke-dashoffset 0.05s linear;
	}

	/* Waypoint circles */
	.pip {
		fill: var(--paper);
		stroke: var(--paper-border);
		stroke-width: 2.5;
		transition: fill 0.3s, stroke 0.3s;
	}

	.pip.active {
		fill: var(--ink-muted);
		stroke: var(--ink-muted);
	}

	.pip.burn {
		fill: var(--stamp-red);
		stroke: var(--stamp-red);
	}

	/* Step numbers inside circles */
	.step-num {
		font-family: var(--font-mono);
		font-size: 7px;
		fill: transparent;
		text-anchor: middle;
		dominant-baseline: central;
		font-weight: 600;
		transition: fill 0.3s;
	}

	.step-num.active {
		fill: var(--paper);
	}

	/* Labels overlay */
	.labels {
		position: absolute;
		inset: 0;
		pointer-events: none;
	}

	.label {
		position: absolute;
		/* Convert SVG viewBox coords to percentages */
		left: calc(var(--lx) / 620 * 100%);
		top: calc(var(--ly) / 360 * 100%);
		transform: translate(-50%, 18px);
		display: flex;
		flex-direction: column;
		align-items: center;
		text-align: center;
		opacity: 0;
		transition: opacity 0.4s;
		width: 110px;
	}

	.label.active {
		opacity: 1;
	}

	.wp-title {
		font-family: var(--font-sans);
		font-size: 12px;
		font-weight: 600;
		color: var(--ink);
		line-height: 1.3;
	}

	.label.burn .wp-title {
		color: var(--stamp-red);
	}

	.wp-desc {
		font-size: 10px;
		color: var(--ink-muted);
		line-height: 1.3;
	}

	/* Mobile: simplified vertical list */
	@media (max-width: 600px) {
		.map svg {
			display: none;
		}

		.labels {
			position: static;
			display: flex;
			flex-direction: column;
		}

		.label {
			position: static;
			transform: none;
			width: auto;
			flex-direction: row;
			align-items: center;
			text-align: left;
			gap: 12px;
			padding: 12px 0 12px 24px;
			border-left: 2px dashed var(--paper-border);
			opacity: 1;
		}

		.label.active {
			border-left-color: var(--ink-faint);
		}

		.label.burn {
			border-left-color: var(--stamp-red);
		}

		.label:last-child {
			border-left-color: transparent;
		}
	}
</style>
