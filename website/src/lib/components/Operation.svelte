<script lang="ts">
	import { onMount } from 'svelte';

	let visible = $state(false);
	let activeStep = $state(-1);
	let el: HTMLElement;

	const steps = [
		{ num: '01', title: 'Install', desc: 'Add the skill to your project with a single command.', icon: '📦' },
		{ num: '02', title: 'Scan', desc: 'Analyzes your auth system, dev credentials, and environment.', icon: '🔍' },
		{ num: '03', title: 'Create', desc: 'Builds dev-only login endpoints and token scripts for your stack.', icon: '🔧' },
		{ num: '04', title: 'Detect', desc: 'Finds your browser automation tools — Playwright, Puppeteer, Chrome, etc.', icon: '🎯' },
		{ num: '05', title: 'Document', desc: 'Updates CLAUDE.md, AGENTS.md, and other agent instructions.', icon: '📝' },
		{ num: '06', title: 'Report', desc: 'Shows you everything it created with copy-paste commands.', icon: '📊' },
		{ num: '07', title: 'Self-destruct', desc: 'Removes itself from your project. No trace left behind.', icon: '💥' },
	];

	onMount(() => {
		const observer = new IntersectionObserver(
			([entry]) => {
				if (entry.isIntersecting) {
					visible = true;
					observer.disconnect();
					steps.forEach((_, i) => {
						setTimeout(() => {
							activeStep = i;
						}, 200 + i * 150);
					});
				}
			},
			{ threshold: 0.1 }
		);
		observer.observe(el);
		return () => observer.disconnect();
	});
</script>

<section class="operation" bind:this={el}>
	<div class="inner">
		<h2 class:visible>The Operation</h2>
		<p class="lead" class:visible>Seven phases. One command. Zero traces.</p>

		<div class="timeline">
			{#each steps as step, i}
				<div class="step" class:active={i <= activeStep} class:burn={i === 6 && i <= activeStep}>
					<div class="step-line">
						<div class="dot">{step.icon}</div>
						{#if i < steps.length - 1}
							<div class="connector" class:burn-line={i === 5 && activeStep >= 6}></div>
						{/if}
					</div>
					<div class="step-content">
						<div class="step-header">
							<span class="step-num">{step.num}</span>
							<h3>{step.title}</h3>
						</div>
						<p>{step.desc}</p>
					</div>
				</div>
			{/each}
		</div>
	</div>
</section>

<style>
	.operation {
		padding: 100px var(--pad);
	}

	.inner {
		max-width: var(--col);
		margin: 0 auto;
	}

	h2 {
		font-family: var(--font-serif);
		font-size: clamp(1.8rem, 4vw, 2.6rem);
		color: var(--text-bright);
		margin-bottom: 8px;
		opacity: 0;
		transform: translateY(20px);
		transition: opacity 0.6s, transform 0.6s;
	}

	h2.visible {
		opacity: 1;
		transform: translateY(0);
	}

	.lead {
		font-size: 1.05rem;
		color: var(--muted);
		margin-bottom: 56px;
		opacity: 0;
		transform: translateY(20px);
		transition: opacity 0.6s 0.1s, transform 0.6s 0.1s;
	}

	.lead.visible {
		opacity: 1;
		transform: translateY(0);
	}

	.timeline {
		display: flex;
		flex-direction: column;
		gap: 0;
	}

	.step {
		display: flex;
		gap: 24px;
		opacity: 0;
		transform: translateX(-16px);
		transition: opacity 0.4s, transform 0.4s cubic-bezier(0.16, 1, 0.3, 1);
	}

	.step.active {
		opacity: 1;
		transform: translateX(0);
	}

	.step-line {
		display: flex;
		flex-direction: column;
		align-items: center;
		flex-shrink: 0;
		width: 40px;
	}

	.dot {
		width: 40px;
		height: 40px;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 18px;
		flex-shrink: 0;
	}

	.connector {
		width: 1px;
		flex: 1;
		min-height: 20px;
		background: var(--faint);
		transition: background 0.5s;
	}

	.burn-line {
		background: linear-gradient(
			to bottom,
			var(--faint) 0%,
			var(--red) 40%,
			var(--amber) 70%,
			transparent 100%
		);
	}

	.step-content {
		padding-bottom: 32px;
	}

	.step-header {
		display: flex;
		align-items: baseline;
		gap: 10px;
		margin-bottom: 4px;
	}

	.step-num {
		font-family: var(--font-mono);
		font-size: 11px;
		color: var(--muted);
		letter-spacing: 0.05em;
	}

	h3 {
		font-family: var(--font-sans);
		font-size: 1rem;
		font-weight: 600;
		color: var(--text-bright);
	}

	.step.burn h3 {
		color: var(--red);
	}

	.step-content p {
		font-size: 14px;
		color: var(--muted);
		line-height: 1.5;
	}

	@media (max-width: 680px) {
		.step {
			gap: 16px;
		}
	}
</style>
