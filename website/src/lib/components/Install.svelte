<script lang="ts">
	import { onMount } from 'svelte';

	let visible = $state(false);
	let el: HTMLElement;

	onMount(() => {
		const observer = new IntersectionObserver(
			([entry]) => {
				if (entry.isIntersecting) {
					visible = true;
					observer.disconnect();
				}
			},
			{ threshold: 0.2 }
		);
		observer.observe(el);
		return () => observer.disconnect();
	});
</script>

<section class="install" bind:this={el}>
	<div class="inner" class:visible>
		<div class="section-line"></div>
		<div class="exhibit">MISSION ACCEPTANCE</div>
		<h2>Accept Your Mission</h2>

		<div class="step">
			<span class="step-num">1.</span>
			<p class="step-text">Install the skill:</p>
		</div>
		<code class="install-cmd">npx skills add pbakaus/burn-after-login</code>

		<div class="step">
			<span class="step-num">2.</span>
			<p class="step-text">Then invoke it in your agent of choice:</p>
		</div>

		<div class="usage-grid">
			<div class="usage-item">
				<span class="tool">Claude Code</span>
				<code>/burn-after-login</code>
			</div>
			<div class="usage-item">
				<span class="tool">Cursor</span>
				<code>/burn-after-login</code>
			</div>
			<div class="usage-item">
				<span class="tool">Others</span>
				<code>per your tool's conventions</code>
			</div>
		</div>

		<p class="footnote">This website will not self-destruct. But the skill will.</p>
	</div>
</section>

<style>
	.install {
		padding: 48px var(--pad) 64px;
	}

	.inner {
		max-width: var(--col);
		margin: 0 auto;
		opacity: 0;
		transform: translateY(16px);
		transition: opacity 0.6s, transform 0.6s;
	}

	.inner.visible {
		opacity: 1;
		transform: translateY(0);
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
		margin-bottom: 16px;
	}

	.step {
		display: flex;
		align-items: baseline;
		gap: 8px;
		margin-bottom: 8px;
	}

	.step-num {
		font-family: var(--font-mono);
		font-size: 13px;
		font-weight: 600;
		color: var(--ink-muted);
	}

	.step-text {
		font-size: 0.95rem;
		color: var(--ink-muted);
	}

	.install-cmd {
		display: block;
		font-family: var(--font-mono);
		font-size: 13px;
		color: var(--ink);
		background: var(--paper-dark);
		padding: 10px 16px;
		border-radius: 4px;
		border: 1px solid var(--paper-border);
		margin-bottom: 28px;
	}

	.usage-grid {
		display: flex;
		gap: 24px;
		flex-wrap: wrap;
		margin-bottom: 40px;
	}

	.usage-item {
		display: flex;
		flex-direction: column;
		gap: 4px;
	}

	.tool {
		font-family: var(--font-mono);
		font-size: 10px;
		font-weight: 500;
		text-transform: uppercase;
		letter-spacing: 0.08em;
		color: var(--ink-muted);
	}

	.usage-item code {
		font-family: var(--font-mono);
		font-size: 13px;
		color: var(--ink);
		background: var(--paper-dark);
		padding: 6px 12px;
		border-radius: 4px;
		border: 1px solid var(--paper-border);
	}

	.footnote {
		font-family: var(--font-serif);
		font-style: italic;
		font-size: 0.9rem;
		color: var(--ink-faint);
	}

	@media (max-width: 680px) {
		.usage-grid {
			flex-direction: column;
			gap: 16px;
		}
	}
</style>
