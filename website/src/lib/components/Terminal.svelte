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
				// Loop after pause
				setTimeout(playAnimation, 6000);
				return;
			}
			currentLine = lineIndex + 1;
			const line = lines[lineIndex];

			// Trigger burn flash at countdown end
			if (line.cls === 'removed' && !flashPhase) {
				flashPhase = true;
				burnPhase = true;
				setTimeout(() => {
					flashPhase = false;
				}, 300);
			}

			// Scroll to bottom
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
				{#each lines.slice(0, currentLine) as line, i}
					<div class="line {line.cls}" style="animation-delay: {i * 0.02}s">
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
		padding: 80px var(--pad) 100px;
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
	}

	.lead {
		font-size: 1.05rem;
		color: var(--muted);
		margin-bottom: 40px;
	}

	.terminal-window {
		background: #13131a;
		border-radius: 10px;
		overflow: hidden;
		box-shadow:
			0 8px 40px rgba(0, 0, 0, 0.4),
			0 0 0 1px rgba(255, 255, 255, 0.05);
		transition: box-shadow 0.5s;
	}

	.terminal-window.burn {
		box-shadow:
			0 8px 40px rgba(0, 0, 0, 0.4),
			0 0 0 1px rgba(255, 255, 255, 0.05),
			0 0 60px rgba(220, 38, 38, 0.2);
	}

	.title-bar {
		display: flex;
		align-items: center;
		padding: 12px 16px;
		background: rgba(255, 255, 255, 0.03);
		border-bottom: 1px solid rgba(255, 255, 255, 0.05);
	}

	.dots {
		display: flex;
		gap: 7px;
		margin-right: 16px;
	}

	.dots .dot {
		width: 11px;
		height: 11px;
		border-radius: 50%;
	}

	.dots .red { background: #ff5f57; }
	.dots .yellow { background: #febc2e; }
	.dots .green { background: #28c840; }

	.title {
		font-family: var(--font-mono);
		font-size: 12px;
		color: var(--muted);
	}

	.terminal-body {
		position: relative;
		padding: 20px 24px;
		min-height: 400px;
		max-height: 480px;
		overflow-y: auto;
		font-family: var(--font-mono);
		font-size: 13px;
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
		opacity: 0.15;
	}

	.scanlines {
		position: absolute;
		inset: 0;
		pointer-events: none;
		background: repeating-linear-gradient(
			to bottom,
			transparent 0px,
			transparent 2px,
			rgba(0, 0, 0, 0.05) 2px,
			rgba(0, 0, 0, 0.05) 4px
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
	.dim { color: #4a4a5a; }
	.text { color: #a8a29e; }
	.done { color: #7ee68a; font-weight: 700; }
	.countdown { color: var(--amber); font-weight: 700; font-size: 14px; }
	.removed { color: var(--red); }
	.final { color: var(--amber); font-style: italic; }

	.cursor-block {
		color: var(--amber);
		animation: blockBlink 0.8s steps(1) infinite;
	}

	@keyframes blockBlink {
		0%, 50% { opacity: 1; }
		51%, 100% { opacity: 0; }
	}

	@media (max-width: 680px) {
		.terminal-body {
			padding: 16px;
			font-size: 11px;
			min-height: 340px;
		}
	}
</style>
