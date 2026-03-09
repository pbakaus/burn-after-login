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

<section class="stacks" bind:this={el}>
	<div class="inner" class:visible>
		<div class="section-line"></div>
		<div class="exhibit">KNOWN TARGETS</div>
		<h2>Compatibility</h2>
		<p class="body">
			Works with <strong>Node.js, Python, Ruby, Go</strong> and any web framework — Next.js, Django, Rails, Flask, FastAPI, Express, SvelteKit.
			Supports <strong>NextAuth, Clerk, Supabase, Firebase, Auth0, Passport, Devise</strong>, and custom auth.
			Detects <strong>Claude in Chrome, Playwright MCP, Chrome DevTools MCP, agent-browser, Stagehand, Puppeteer, Cypress, Selenium</strong>.
		</p>
	</div>
</section>

<style>
	.stacks {
		padding: 48px var(--pad) 48px;
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
		margin-bottom: 12px;
	}

	.body {
		font-size: 0.95rem;
		color: var(--ink-muted);
		line-height: 1.8;
		max-width: 620px;
	}

	.body strong {
		color: var(--ink);
		font-weight: 500;
	}
</style>
