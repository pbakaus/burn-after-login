<script lang="ts">
	import { onMount } from 'svelte';

	let displayText = $state('');
	let typingDone = $state(false);
	let firstVisit = $state(false);
	let showHeadline = $state(false);
	let mounted = $state(false);
	let copied = $state(false);

	// Segments: text to type, then optional pause (ms) before next segment
	// \n creates a line break in the output
	const segments = [
		{ text: 'Good morning, agent.', pause: 600 },
		{ text: '\nAI browser automation tools are being stopped at login walls across the world.', pause: 400 },
		{ text: '\nYour mission, should you choose to accept it...', pause: 0 },
	];

	// Flat briefing for return visits
	const briefingFlat = segments.map(s => s.text).join('');

	const cmd = 'npx skills add pbakaus/burn-after-login';

	function copyCommand() {
		navigator.clipboard.writeText(cmd);
		copied = true;
		setTimeout(() => { copied = false; }, 2500);
	}

	function typeSegments() {
		let segIdx = 0;
		let charIdx = 0;
		let current = '';

		function typeNext() {
			const seg = segments[segIdx];
			if (charIdx < seg.text.length) {
				charIdx++;
				displayText = current + seg.text.slice(0, charIdx);
				setTimeout(typeNext, 25);
			} else {
				// Segment done
				current += seg.text;
				segIdx++;
				if (segIdx < segments.length) {
					// Pause before next segment
					charIdx = 0;
					setTimeout(typeNext, seg.pause);
				} else {
					// All done
					finishTyping();
				}
			}
		}

		typeNext();
	}

	function finishTyping() {
		typingDone = true;
		setTimeout(() => {
			showHeadline = true;
			setTimeout(() => {
				document.documentElement.classList.remove('intro-typing');
				document.documentElement.classList.add('intro-reveal');
				setTimeout(() => {
					document.documentElement.classList.remove('intro-reveal');
				}, 1200);
			}, 500);
		}, 500);
	}

	onMount(() => {
		mounted = true;
		const visited = localStorage.getItem('bal-visited');

		if (visited) {
			typingDone = true;
			showHeadline = true;
			return;
		}

		// First visit — dramatic intro
		firstVisit = true;
		document.documentElement.classList.add('intro-typing');
		localStorage.setItem('bal-visited', '1');
		typeSegments();
	});
</script>

<section class="hero">
	<div class="watermark">CLASSIFIED</div>
	<div class="inner">
		<!-- First visit: typing takes center stage -->
		{#if firstVisit}
			<div class="briefing" class:done={typingDone}>
				<p class="typing-text">
					{#each displayText.split('\n') as line, i}
						{#if i > 0}<br/>{/if}{line}
					{/each}{#if !typingDone}<span class="cursor">|</span>{/if}
				</p>
			</div>
		{/if}

		<!-- Return visit: briefing as muted static text -->
		{#if mounted && !firstVisit}
			<p class="briefing-static">
				{#each briefingFlat.split('\n') as line, i}
					{#if i > 0}<br/>{/if}{line}
				{/each}
			</p>
		{/if}

		<div
			class="headline"
			class:show={showHeadline}
			class:first-reveal={firstVisit && showHeadline}
		>
			<div class="stamp-row">
				<span class="classified" class:show={showHeadline}>CLASSIFIED</span>
			</div>
			<h1>burn-after-login</h1>
			<p class="subtitle">A self-destructing skill for dev authentication</p>
			<p class="tagline">Install it. Run it. It creates dev-only auth shortcuts for your AI browser agents, updates your agent instructions, then <em>destroys itself</em>.</p>

			<div class="cta" class:show={showHeadline}>
				<button class="cmd-box" onclick={copyCommand}>
					<code>{cmd}</code>
					<span class="copy-label">{copied ? 'Copied — good luck, agent' : 'Copy'}</span>
				</button>
				<p class="hint">Works with 20+ compatible agents — Claude Code, Cursor, Gemini CLI, Copilot, and more.</p>
			</div>
		</div>
	</div>
</section>

<style>
	.hero {
		position: relative;
		padding: 100px var(--pad) 96px;
		min-height: 50vh;
	}

	.watermark {
		position: absolute;
		top: 40vh;
		left: 50%;
		transform: translate(-50%, -50%) rotate(-25deg);
		font-family: var(--font-sans);
		font-size: clamp(80px, 15vw, 160px);
		font-weight: 700;
		letter-spacing: 0.1em;
		color: var(--stamp-red);
		opacity: 0.03;
		pointer-events: none;
		white-space: nowrap;
		user-select: none;
	}

	.inner {
		max-width: var(--col);
		margin: 0 auto;
		position: relative;
	}

	/* First visit typing — large, prominent, with generous spacing */
	.briefing {
		padding-top: 12vh;
		padding-bottom: 4vh;
		transition: opacity 0.6s ease, filter 0.6s ease;
	}

	.briefing.done {
		opacity: 0;
		filter: blur(4px);
		pointer-events: none;
		position: absolute;
		width: 100%;
	}

	.typing-text {
		font-family: var(--font-mono);
		font-size: clamp(1rem, 2.2vw, 1.2rem);
		color: var(--ink-light);
		line-height: 1.8;
		max-width: 580px;
	}

	.cursor {
		animation: blink 0.8s steps(1) infinite;
		color: var(--stamp-red);
	}

	@keyframes blink {
		0%, 50% { opacity: 1; }
		51%, 100% { opacity: 0; }
	}

	/* Return visit: muted static briefing */
	.briefing-static {
		font-family: var(--font-mono);
		font-size: 13px;
		color: var(--ink-faint);
		line-height: 1.7;
		max-width: 560px;
		margin-bottom: 24px;
	}

	/* Headline — visible by default for SSR */
	.headline {
		opacity: 1;
		transform: translateY(0);
	}

	/* Hidden until show */
	.headline:not(.show) {
		opacity: 0;
		pointer-events: none;
		height: 0;
		overflow: hidden;
	}

	/* First visit: animate in dramatically */
	.headline.first-reveal {
		animation: headline-in 0.8s cubic-bezier(0.16, 1, 0.3, 1) both;
	}

	@keyframes headline-in {
		from { opacity: 0; transform: translateY(24px); filter: blur(6px); }
		to { opacity: 1; transform: translateY(0); filter: blur(0); }
	}

	.stamp-row {
		margin-bottom: 12px;
	}

	.classified {
		display: inline-block;
		font-family: var(--font-sans);
		font-size: 11px;
		font-weight: 600;
		letter-spacing: 0.2em;
		color: var(--stamp-red);
		border: 2px solid var(--stamp-red);
		padding: 3px 12px;
		transform: rotate(-2deg) scale(0.5);
		opacity: 0;
		transition: all 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
	}

	.classified.show {
		transform: rotate(-2deg) scale(1);
		opacity: 0.7;
	}

	h1 {
		font-family: var(--font-mono);
		font-size: clamp(2.2rem, 6vw, 4rem);
		font-weight: 700;
		color: var(--ink);
		letter-spacing: -0.03em;
		line-height: 1.1;
		margin-bottom: 12px;
	}

	.subtitle {
		font-family: var(--font-serif);
		font-size: clamp(1rem, 2vw, 1.2rem);
		font-style: italic;
		color: var(--ink-muted);
		margin-bottom: 20px;
	}

	.tagline {
		font-size: 1rem;
		color: var(--ink-light);
		max-width: 540px;
		line-height: 1.7;
		margin-bottom: 32px;
	}

	.tagline em {
		color: var(--stamp-red);
		font-style: normal;
		font-weight: 500;
	}

	/* CTA */
	.cta {
		opacity: 0;
		transform: translateY(8px);
		transition: opacity 0.6s 0.3s, transform 0.6s 0.3s;
	}

	.cta.show {
		opacity: 1;
		transform: translateY(0);
	}

	.cmd-box {
		display: inline-flex;
		align-items: center;
		gap: 16px;
		background: var(--ink);
		border: none;
		border-radius: 6px;
		padding: 12px 20px;
		cursor: pointer;
		transition: background 0.2s, transform 0.15s;
		font-family: inherit;
	}

	.cmd-box:hover {
		background: var(--ink-light);
		transform: translateY(-1px);
	}

	.cmd-box:active {
		transform: translateY(0);
	}

	.cmd-box code {
		font-family: var(--font-mono);
		font-size: 14px;
		color: #f5f0e8;
	}

	.copy-label {
		font-size: 12px;
		color: rgba(245, 240, 232, 0.5);
		white-space: nowrap;
		min-width: 55px;
	}

	.hint {
		margin-top: 12px;
		font-size: 13px;
		color: var(--ink-muted);
	}

	.hint a {
		color: var(--ink-muted);
	}

	.hint a:hover {
		color: var(--ink);
	}

	@media (max-width: 680px) {
		.hero {
			padding-top: 80px;
		}

		.briefing {
			padding-top: 6vh;
		}
	}
</style>
