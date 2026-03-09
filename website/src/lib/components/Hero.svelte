<script lang="ts">
	import { onMount } from 'svelte';

	let phase = $state<'typing' | 'reveal' | 'done'>('typing');
	let charIndex = $state(0);
	let showStamp = $state(false);
	let showCta = $state(false);

	const briefing = "Good morning, agent. AI browser automation tools are being stopped at login walls across the world. Your mission, should you choose to accept it...";

	onMount(() => {
		const interval = setInterval(() => {
			if (charIndex < briefing.length) {
				charIndex++;
			} else {
				clearInterval(interval);
				setTimeout(() => {
					phase = 'reveal';
					setTimeout(() => {
						phase = 'done';
						showStamp = true;
						setTimeout(() => {
							showCta = true;
						}, 400);
					}, 600);
				}, 800);
			}
		}, 28);

		return () => clearInterval(interval);
	});
</script>

<section class="hero">
	<div class="grain"></div>
	<div class="inner">
		<div class="briefing" class:fade-out={phase === 'reveal'} class:hidden={phase === 'done'}>
			<p class="typing-text">
				{briefing.slice(0, charIndex)}<span class="cursor">|</span>
			</p>
		</div>

		<div class="headline" class:visible={phase === 'done'}>
			<div class="stamp-area">
				<span class="classified" class:show={showStamp}>CLASSIFIED</span>
			</div>
			<h1>burn-after-login</h1>
			<p class="subtitle">A self-destructing skill for dev authentication</p>
			<p class="tagline">Install it. Run it. It creates dev-only auth shortcuts for your AI browser agents, updates your agent instructions, then <em>destroys itself</em>.</p>

			<div class="cta" class:show={showCta}>
				<div class="install-cmd">
					<code>npx skills add pbakaus/burn-after-login</code>
				</div>
				<p class="hint">Works with <a href="https://agentskills.io" target="_blank" rel="noopener">20+ compatible agents</a> — Claude Code, Cursor, Gemini CLI, Copilot, and more.</p>
			</div>
		</div>
	</div>
</section>

<style>
	.hero {
		position: relative;
		min-height: 100vh;
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 80px var(--pad) 60px;
		overflow: hidden;
	}

	.grain {
		position: absolute;
		inset: 0;
		opacity: 0.03;
		background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)'/%3E%3C/svg%3E");
		background-size: 256px 256px;
		pointer-events: none;
	}

	.inner {
		max-width: var(--col);
		width: 100%;
		text-align: center;
		position: relative;
	}

	/* Briefing typing phase */
	.briefing {
		min-height: 200px;
		display: flex;
		align-items: center;
		justify-content: center;
		transition: opacity 0.5s;
	}

	.briefing.fade-out {
		opacity: 0;
	}

	.briefing.hidden {
		display: none;
	}

	.typing-text {
		font-family: var(--font-mono);
		font-size: clamp(1rem, 2.5vw, 1.25rem);
		color: var(--amber);
		line-height: 1.8;
		max-width: 600px;
	}

	.cursor {
		animation: blink 0.8s steps(1) infinite;
		color: var(--amber);
	}

	@keyframes blink {
		0%, 50% { opacity: 1; }
		51%, 100% { opacity: 0; }
	}

	/* Headline reveal phase */
	.headline {
		opacity: 0;
		transform: translateY(20px);
		transition: opacity 0.8s cubic-bezier(0.16, 1, 0.3, 1), transform 0.8s cubic-bezier(0.16, 1, 0.3, 1);
		pointer-events: none;
	}

	.headline.visible {
		opacity: 1;
		transform: translateY(0);
		pointer-events: auto;
	}

	.stamp-area {
		height: 60px;
		display: flex;
		align-items: center;
		justify-content: center;
		margin-bottom: 16px;
	}

	.classified {
		display: inline-block;
		font-family: var(--font-sans);
		font-size: 13px;
		font-weight: 600;
		letter-spacing: 0.2em;
		color: var(--red);
		border: 2px solid var(--red);
		padding: 4px 16px;
		transform: rotate(-3deg) scale(0.5);
		opacity: 0;
		transition: all 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
	}

	.classified.show {
		transform: rotate(-3deg) scale(1);
		opacity: 0.8;
	}

	h1 {
		font-family: var(--font-mono);
		font-size: clamp(2.5rem, 7vw, 5rem);
		font-weight: 700;
		color: var(--text-bright);
		letter-spacing: -0.03em;
		line-height: 1.1;
		margin-bottom: 16px;
	}

	.subtitle {
		font-family: var(--font-serif);
		font-size: clamp(1.1rem, 2.5vw, 1.4rem);
		font-style: italic;
		color: var(--muted);
		margin-bottom: 28px;
	}

	.tagline {
		font-size: 1rem;
		color: var(--text);
		max-width: 540px;
		margin: 0 auto 48px;
		line-height: 1.7;
	}

	.tagline em {
		color: var(--red);
		font-style: normal;
		font-weight: 500;
	}

	/* CTA */
	.cta {
		opacity: 0;
		transform: translateY(12px);
		transition: opacity 0.6s, transform 0.6s;
	}

	.cta.show {
		opacity: 1;
		transform: translateY(0);
	}

	.install-cmd {
		display: inline-flex;
		align-items: center;
		background: var(--bg-subtle);
		border: 1px solid var(--border);
		border-radius: 8px;
		padding: 14px 28px;
		transition: border-color 0.2s;
	}

	.install-cmd:hover {
		border-color: var(--amber);
	}

	.install-cmd code {
		font-family: var(--font-mono);
		font-size: 15px;
		color: var(--amber);
	}

	.hint {
		margin-top: 16px;
		font-size: 13px;
		color: var(--muted);
	}

	.hint a {
		color: var(--muted);
		text-decoration: underline;
	}

	.hint a:hover {
		color: var(--text);
	}

	@media (max-width: 680px) {
		.hero {
			min-height: 90vh;
			padding-top: 100px;
		}

		.stamp-area {
			height: 48px;
		}
	}
</style>
