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
			{ threshold: 0.15 }
		);
		observer.observe(el);
		return () => observer.disconnect();
	});
</script>

<section class="problem" bind:this={el}>
	<div class="inner" class:visible>
		<div class="section-line"></div>
		<div class="exhibit">EXHIBIT A</div>

		<h2>Intelligence Report</h2>

		<p class="lead">
			Every time an AI agent encounters a login form, the mission goes sideways.
		</p>

		<div class="evidence">
			<div class="item">
				<span class="label">Agent</span>
				<span class="redacted" title="Hover to declassify">navigates to dashboard</span>
			</div>
			<div class="item">
				<span class="label">Server</span>
				<span class="value redirect">302 → /login</span>
			</div>
			<div class="item">
				<span class="label">Agent</span>
				<span class="redacted">fumbles with form fields</span>
			</div>
			<div class="item">
				<span class="label">Agent</span>
				<span class="redacted">fails CAPTCHA, loses session</span>
			</div>
			<div class="item">
				<span class="label">Agent</span>
				<span class="redacted">retries 3 more times</span>
			</div>
			<div class="item">
				<span class="label">Result</span>
				<span class="value failed">MISSION FAILED</span>
			</div>
		</div>

		<p class="declassify-hint">[ hover redacted bars to declassify ]</p>

		<p class="conclusion">
			Browser automation tools — Claude in Chrome, Playwright MCP, agent-browser,
			Stagehand, Cursor's browser mode — all hit the same wall.
			<strong>Login forms were designed to stop bots. Your agents are bots.</strong>
		</p>
	</div>
</section>

<style>
	.problem {
		padding: 24px var(--pad) 72px;
	}

	.inner {
		max-width: var(--col);
		margin: 0 auto;
		opacity: 0;
		transform: translateY(20px);
		transition: opacity 0.8s, transform 0.8s cubic-bezier(0.16, 1, 0.3, 1);
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
		font-weight: 700;
		color: var(--ink);
		margin-bottom: 12px;
		line-height: 1.2;
	}

	.lead {
		font-size: 1rem;
		color: var(--ink-light);
		margin-bottom: 28px;
		line-height: 1.6;
	}

	.evidence {
		display: flex;
		flex-direction: column;
		margin-bottom: 12px;
		border: 1px solid var(--paper-border);
		border-radius: 3px;
		overflow: hidden;
	}

	.item {
		display: flex;
		align-items: center;
		gap: 16px;
		padding: 9px 16px;
		border-bottom: 1px solid var(--paper-dark);
		font-size: 14px;
	}

	.item:last-child {
		border-bottom: none;
	}

	.label {
		font-family: var(--font-mono);
		font-size: 10px;
		font-weight: 500;
		text-transform: uppercase;
		letter-spacing: 0.08em;
		color: var(--ink-muted);
		min-width: 55px;
		flex-shrink: 0;
	}

	.redacted {
		background: var(--ink);
		color: var(--ink);
		padding: 1px 6px;
		border-radius: 2px;
		cursor: pointer;
		transition: background 0.3s, color 0.3s;
		font-size: 14px;
	}

	.redacted:hover {
		background: transparent;
		color: var(--ink-light);
	}

	.declassify-hint {
		font-family: var(--font-mono);
		font-size: 11px;
		color: var(--ink-faint);
		margin-bottom: 24px;
		letter-spacing: 0.02em;
	}

	.redirect {
		font-family: var(--font-mono);
		color: var(--highlight);
		font-size: 13px;
	}

	.failed {
		font-family: var(--font-mono);
		font-weight: 700;
		color: var(--stamp-red);
		font-size: 13px;
		letter-spacing: 0.05em;
	}

	.conclusion {
		font-size: 0.95rem;
		color: var(--ink-muted);
		line-height: 1.7;
	}

	.conclusion strong {
		color: var(--ink);
	}

	@media (max-width: 680px) {
		.item {
			flex-direction: column;
			gap: 4px;
			align-items: flex-start;
		}
	}
</style>
