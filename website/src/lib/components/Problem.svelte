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
	<div class="dossier" class:visible>
		<div class="paper">
			<div class="burn-edge burn-top"></div>
			<div class="burn-edge burn-bottom"></div>

			<div class="exhibit">EXHIBIT A</div>

			<h2>The Problem</h2>

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
					<span class="redacted" title="Hover to declassify">fumbles with form fields</span>
				</div>
				<div class="item">
					<span class="label">Agent</span>
					<span class="redacted" title="Hover to declassify">fails CAPTCHA, loses session</span>
				</div>
				<div class="item">
					<span class="label">Agent</span>
					<span class="redacted" title="Hover to declassify">retries 3 more times</span>
				</div>
				<div class="item">
					<span class="label">Result</span>
					<span class="value failed">MISSION FAILED</span>
				</div>
			</div>

			<p class="conclusion">
				Browser automation tools — Claude in Chrome, Playwright MCP, agent-browser,
				Stagehand, Cursor's browser mode — all hit the same wall.
				<strong>Login forms were designed to stop bots. Your agents are bots.</strong>
			</p>
		</div>
	</div>
</section>

<style>
	.problem {
		padding: 40px var(--pad) 100px;
		display: flex;
		justify-content: center;
	}

	.dossier {
		max-width: var(--col);
		width: 100%;
		opacity: 0;
		transform: translateY(30px);
		transition: opacity 0.8s, transform 0.8s cubic-bezier(0.16, 1, 0.3, 1);
	}

	.dossier.visible {
		opacity: 1;
		transform: translateY(0);
	}

	.paper {
		position: relative;
		background: var(--paper);
		color: #1e1a15;
		padding: 56px 48px;
		border-radius: 2px;
		box-shadow:
			0 4px 24px rgba(0, 0, 0, 0.3),
			0 1px 2px rgba(0, 0, 0, 0.2);
	}

	.burn-edge {
		position: absolute;
		left: 0;
		right: 0;
		height: 20px;
		pointer-events: none;
	}

	.burn-top {
		top: -1px;
		background: linear-gradient(
			to bottom,
			var(--bg) 0%,
			transparent 30%
		);
		mask-image: url("data:image/svg+xml,%3Csvg width='800' height='20' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M0,8 Q20,2 40,10 T80,6 T120,12 T160,4 T200,10 T240,8 T280,14 T320,6 T360,10 T400,4 T440,12 T480,8 T520,14 T560,6 T600,10 T640,8 T680,12 T720,4 T760,10 T800,8 L800,0 L0,0 Z' fill='white'/%3E%3C/svg%3E");
		mask-size: 100% 100%;
		-webkit-mask-image: url("data:image/svg+xml,%3Csvg width='800' height='20' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M0,8 Q20,2 40,10 T80,6 T120,12 T160,4 T200,10 T240,8 T280,14 T320,6 T360,10 T400,4 T440,12 T480,8 T520,14 T560,6 T600,10 T640,8 T680,12 T720,4 T760,10 T800,8 L800,0 L0,0 Z' fill='white'/%3E%3C/svg%3E");
		-webkit-mask-size: 100% 100%;
	}

	.burn-bottom {
		bottom: -1px;
		background: linear-gradient(
			to top,
			var(--bg) 0%,
			transparent 30%
		);
		mask-image: url("data:image/svg+xml,%3Csvg width='800' height='20' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M0,12 Q20,18 40,10 T80,14 T120,8 T160,16 T200,10 T240,12 T280,6 T320,14 T360,10 T400,16 T440,8 T480,12 T520,6 T560,14 T600,10 T640,12 T680,8 T720,16 T760,10 T800,12 L800,20 L0,20 Z' fill='white'/%3E%3C/svg%3E");
		mask-size: 100% 100%;
		-webkit-mask-image: url("data:image/svg+xml,%3Csvg width='800' height='20' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M0,12 Q20,18 40,10 T80,14 T120,8 T160,16 T200,10 T240,12 T280,6 T320,14 T360,10 T400,16 T440,8 T480,12 T520,6 T560,14 T600,10 T640,12 T680,8 T720,16 T760,10 T800,12 L800,20 L0,20 Z' fill='white'/%3E%3C/svg%3E");
		-webkit-mask-size: 100% 100%;
	}

	.exhibit {
		font-family: var(--font-mono);
		font-size: 11px;
		letter-spacing: 0.15em;
		color: var(--red);
		opacity: 0.6;
		margin-bottom: 24px;
	}

	h2 {
		font-family: var(--font-serif);
		font-size: clamp(1.8rem, 4vw, 2.6rem);
		font-weight: 700;
		color: #1e1a15;
		margin-bottom: 16px;
		line-height: 1.2;
	}

	.lead {
		font-size: 1.05rem;
		color: #44403c;
		margin-bottom: 32px;
		line-height: 1.6;
	}

	.evidence {
		display: flex;
		flex-direction: column;
		gap: 0;
		margin-bottom: 32px;
		border: 1px solid #d6cfc4;
		border-radius: 4px;
		overflow: hidden;
	}

	.item {
		display: flex;
		align-items: center;
		gap: 16px;
		padding: 10px 16px;
		border-bottom: 1px solid #e6e0d6;
		font-size: 14px;
	}

	.item:last-child {
		border-bottom: none;
	}

	.label {
		font-family: var(--font-mono);
		font-size: 11px;
		font-weight: 500;
		text-transform: uppercase;
		letter-spacing: 0.08em;
		color: #7a7268;
		min-width: 60px;
		flex-shrink: 0;
	}

	.redacted {
		background: #1e1a15;
		color: #1e1a15;
		padding: 1px 6px;
		border-radius: 2px;
		cursor: help;
		transition: background 0.3s, color 0.3s;
		font-size: 14px;
	}

	.redacted:hover {
		background: transparent;
		color: #44403c;
	}

	.redirect {
		font-family: var(--font-mono);
		color: #b45309;
		font-size: 13px;
	}

	.failed {
		font-family: var(--font-mono);
		font-weight: 700;
		color: var(--red);
		font-size: 13px;
		letter-spacing: 0.05em;
	}

	.conclusion {
		font-size: 0.95rem;
		color: #57534e;
		line-height: 1.7;
	}

	.conclusion strong {
		color: #1e1a15;
	}

	@media (max-width: 680px) {
		.paper {
			padding: 40px 24px;
		}

		.item {
			flex-direction: column;
			gap: 4px;
			align-items: flex-start;
		}
	}
</style>
