<script lang="ts">
	import { onMount } from 'svelte';

	let visible = $state(false);
	let currentLine = $state(0);
	let burnPhase = $state(false);
	let flashPhase = $state(false);
	let el: HTMLElement;
	let scrollContainer: HTMLElement;

	interface Line {
		text: string;
		cls: string;
		delay: number;
	}

	const lines: Line[] = [
		{ text: '$ /burn-after-login', cls: 'cmd', delay: 400 },
		{ text: '', cls: 'blank', delay: 200 },
		{ text: '🔍 Scanning codebase...', cls: 'cyan', delay: 600 },
		{ text: '', cls: 'blank', delay: 100 },
		{ text: '  ✓ Development mode: NODE_ENV detected', cls: 'ok', delay: 300 },
		{ text: '  ✓ Auth system: NextAuth v5 (session-based)', cls: 'ok', delay: 300 },
		{ text: '  ✓ Test users: demo@test.com (admin), viewer@test.com (viewer)', cls: 'ok', delay: 300 },
		{ text: '  ✓ Browser tools: Claude in Chrome, Playwright MCP', cls: 'ok', delay: 400 },
		{ text: '', cls: 'blank', delay: 200 },
		{ text: '📝 Creating dev auth shortcuts...', cls: 'cyan', delay: 500 },
		{ text: '', cls: 'blank', delay: 100 },
		{ text: '  ✓ Created app/api/dev-login/route.ts', cls: 'ok', delay: 250 },
		{ text: '    → GET /dev-login?email=...&password=...', cls: 'dim', delay: 150 },
		{ text: '    → Returns 404 in production', cls: 'dim', delay: 150 },
		{ text: '  ✓ Updated CLAUDE.md — added "Dev Authentication" section', cls: 'ok', delay: 250 },
		{ text: '  ✓ Updated AGENTS.md — added browser auth examples', cls: 'ok', delay: 250 },
		{ text: '', cls: 'blank', delay: 300 },
		{ text: '────────────────────────────────────────', cls: 'dim', delay: 100 },
		{ text: '  Mission complete. All shortcuts operational.', cls: 'done', delay: 500 },
		{ text: '────────────────────────────────────────', cls: 'dim', delay: 100 },
		{ text: '', cls: 'blank', delay: 400 },
		{ text: '  This skill has served its purpose.', cls: 'text', delay: 400 },
		{ text: '  Self-destruct? (y/n) y', cls: 'cmd-y', delay: 800 },
		{ text: '', cls: 'blank', delay: 400 },
		{ text: '  💥 3...', cls: 'countdown', delay: 700 },
		{ text: '  💥 2...', cls: 'countdown', delay: 700 },
		{ text: '  💥 1...', cls: 'countdown', delay: 700 },
		{ text: '', cls: 'blank', delay: 200 },
		{ text: '  burn-after-login has been removed.', cls: 'removed', delay: 300 },
		{ text: '  Your dev auth shortcuts remain intact.', cls: 'text', delay: 200 },
		{ text: '  Good luck out there, agent.', cls: 'final', delay: 0 },
	];

	function playAnimation() {
		currentLine = 0;
		burnPhase = false;
		flashPhase = false;

		let lineIndex = 0;
		function showNext() {
			if (lineIndex >= lines.length) {
				setTimeout(playAnimation, 6000);
				return;
			}
			currentLine = lineIndex + 1;
			const line = lines[lineIndex];

			if (line.cls === 'removed' && !flashPhase) {
				flashPhase = true;
				burnPhase = true;
				setTimeout(() => { flashPhase = false; }, 300);
			}

			if (scrollContainer) {
				requestAnimationFrame(() => {
					scrollContainer.scrollTop = scrollContainer.scrollHeight;
				});
			}

			lineIndex++;
			setTimeout(showNext, lines[lineIndex - 1].delay);
		}
		showNext();
	}

	onMount(() => {
		const observer = new IntersectionObserver(
			([entry]) => {
				if (entry.isIntersecting && !visible) {
					visible = true;
					observer.disconnect();
					setTimeout(playAnimation, 400);
				}
			},
			{ threshold: 0.2 }
		);
		observer.observe(el);
		return () => observer.disconnect();
	});
</script>

<section class="terminal-section" bind:this={el}>
	<div class="inner">
		<div class="section-line"></div>
		<div class="exhibit">INTERCEPTED TRANSMISSION</div>
		<h2>Field Test</h2>
		<p class="lead">Watch the skill in action.</p>

		<div class="terminal-window" class:burn={burnPhase}>
			<div class="title-bar">
				<div class="dots">
					<span class="dot red"></span>
					<span class="dot yellow"></span>
					<span class="dot green"></span>
				</div>
				<span class="title">burn-after-login</span>
			</div>
			<div class="terminal-body" bind:this={scrollContainer}>
				<div class="flash" class:active={flashPhase}></div>
				<div class="scanlines"></div>
				{#each lines.slice(0, currentLine) as line}
					<div class="line {line.cls}">
						{#if line.text === ''}
							&nbsp;
						{:else}
							{line.text}
						{/if}
					</div>
				{/each}
				{#if currentLine > 0 && currentLine < lines.length}
					<span class="cursor-block">█</span>
				{/if}
			</div>
		</div>
	</div>
</section>

<style>
	.terminal-section {
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
		margin-bottom: 8px;
	}

	.lead {
		font-size: 1rem;
		color: var(--ink-muted);
		margin-bottom: 28px;
	}

	.terminal-window {
		background: var(--terminal-bg);
		border-radius: 8px;
		overflow: hidden;
		box-shadow:
			0 4px 20px rgba(0, 0, 0, 0.15),
			0 0 0 1px rgba(0, 0, 0, 0.08);
		transition: box-shadow 0.5s;
	}

	.terminal-window.burn {
		box-shadow:
			0 4px 20px rgba(0, 0, 0, 0.15),
			0 0 0 1px rgba(0, 0, 0, 0.08),
			0 0 40px rgba(185, 28, 28, 0.15);
	}

	.title-bar {
		display: flex;
		align-items: center;
		padding: 10px 16px;
		background: rgba(255, 255, 255, 0.03);
		border-bottom: 1px solid rgba(255, 255, 255, 0.05);
	}

	.dots {
		display: flex;
		gap: 6px;
		margin-right: 16px;
	}

	.dots .dot {
		width: 10px;
		height: 10px;
		border-radius: 50%;
	}

	.dots .red { background: #ff5f57; }
	.dots .yellow { background: #febc2e; }
	.dots .green { background: #28c840; }

	.title {
		font-family: var(--font-mono);
		font-size: 11px;
		color: var(--terminal-muted);
	}

	.terminal-body {
		position: relative;
		padding: 16px 20px;
		min-height: 360px;
		max-height: 440px;
		overflow-y: auto;
		font-family: var(--font-mono);
		font-size: 12.5px;
		line-height: 1.7;
		scrollbar-width: none;
	}

	.terminal-body::-webkit-scrollbar {
		display: none;
	}

	.flash {
		position: absolute;
		inset: 0;
		background: white;
		opacity: 0;
		pointer-events: none;
		z-index: 10;
		transition: opacity 0.3s;
	}

	.flash.active {
		opacity: 0.12;
	}

	.scanlines {
		position: absolute;
		inset: 0;
		pointer-events: none;
		background: repeating-linear-gradient(
			to bottom,
			transparent 0px,
			transparent 2px,
			rgba(0, 0, 0, 0.04) 2px,
			rgba(0, 0, 0, 0.04) 4px
		);
		z-index: 5;
	}

	.line {
		animation: lineIn 0.12s ease-out;
		white-space: pre-wrap;
		word-break: break-all;
	}

	@keyframes lineIn {
		from { opacity: 0; transform: translateY(2px); }
		to { opacity: 1; transform: translateY(0); }
	}

	.cmd { color: #f5f5f5; }
	.cmd-y { color: #f5f5f5; }
	.cyan { color: #78c8de; }
	.ok { color: #7ee68a; }
	.dim { color: var(--terminal-muted); }
	.text { color: #a8a29e; }
	.done { color: #7ee68a; font-weight: 700; }
	.countdown { color: #f59e0b; font-weight: 700; font-size: 13px; }
	.removed { color: #ef4444; }
	.final { color: #f59e0b; font-style: italic; }

	.cursor-block {
		color: #f59e0b;
		animation: blockBlink 0.8s steps(1) infinite;
	}

	@keyframes blockBlink {
		0%, 50% { opacity: 1; }
		51%, 100% { opacity: 0; }
	}

	@media (max-width: 680px) {
		.terminal-body {
			padding: 14px;
			font-size: 11px;
			min-height: 300px;
		}
	}
</style>
