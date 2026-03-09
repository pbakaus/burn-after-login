<script lang="ts">
	import { onMount } from 'svelte';

	let visible = $state(false);
	let el: HTMLElement;

	const categories = [
		{
			title: 'Languages & Frameworks',
			items: ['Node.js', 'Python', 'Ruby', 'Go', 'Next.js', 'Django', 'Rails', 'Flask', 'FastAPI', 'Express', 'SvelteKit']
		},
		{
			title: 'Auth Systems',
			items: ['NextAuth', 'Clerk', 'Supabase Auth', 'Firebase Auth', 'Passport.js', 'Devise', 'Auth0', 'Custom']
		},
		{
			title: 'Browser Automation',
			items: ['Claude in Chrome', 'Playwright MCP', 'Chrome DevTools MCP', 'agent-browser', 'Stagehand', 'Puppeteer', 'Cypress', 'Selenium']
		}
	];

	onMount(() => {
		const observer = new IntersectionObserver(
			([entry]) => {
				if (entry.isIntersecting) {
					visible = true;
					observer.disconnect();
				}
			},
			{ threshold: 0.1 }
		);
		observer.observe(el);
		return () => observer.disconnect();
	});
</script>

<section class="stacks" bind:this={el}>
	<div class="inner" class:visible>
		<h2>Known Targets</h2>
		<p class="lead">Works with any combination of these stacks.</p>

		<div class="grid">
			{#each categories as cat, ci}
				<div class="card" style="animation-delay: {ci * 0.15}s">
					<h3>{cat.title}</h3>
					<div class="tags">
						{#each cat.items as item}
							<span class="tag">{item}</span>
						{/each}
					</div>
				</div>
			{/each}
		</div>
	</div>
</section>

<style>
	.stacks {
		padding: 80px var(--pad) 100px;
	}

	.inner {
		max-width: var(--col);
		margin: 0 auto;
		opacity: 0;
		transform: translateY(20px);
		transition: opacity 0.6s, transform 0.6s;
	}

	.inner.visible {
		opacity: 1;
		transform: translateY(0);
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

	.grid {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		gap: 20px;
	}

	.card {
		background: var(--bg-subtle);
		border: 1px solid var(--border);
		border-radius: 8px;
		padding: 24px;
		transition: border-color 0.2s, box-shadow 0.2s;
	}

	.card:hover {
		border-color: var(--amber);
		box-shadow: 0 0 20px rgba(245, 158, 11, 0.06);
	}

	h3 {
		font-family: var(--font-mono);
		font-size: 12px;
		font-weight: 500;
		letter-spacing: 0.08em;
		text-transform: uppercase;
		color: var(--amber);
		margin-bottom: 16px;
	}

	.tags {
		display: flex;
		flex-wrap: wrap;
		gap: 6px;
	}

	.tag {
		font-size: 13px;
		color: var(--text);
		background: rgba(255, 255, 255, 0.04);
		padding: 4px 10px;
		border-radius: 4px;
		border: 1px solid rgba(255, 255, 255, 0.06);
	}

	@media (max-width: 800px) {
		.grid {
			grid-template-columns: 1fr;
		}
	}
</style>
