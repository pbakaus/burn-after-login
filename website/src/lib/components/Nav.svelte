<script lang="ts">
	import { onMount } from 'svelte';

	let scrolled = $state(false);

	onMount(() => {
		const onScroll = () => {
			scrolled = window.scrollY > 20;
		};
		window.addEventListener('scroll', onScroll, { passive: true });
		return () => window.removeEventListener('scroll', onScroll);
	});
</script>

<nav class:scrolled>
	<div class="inner">
		<a href="/" class="wordmark">burn-after-login</a>
		<div class="links">
			<a href="https://github.com/pbakaus/burn-after-login" target="_blank" rel="noopener">GitHub</a>
			<a href="https://agentskills.io" target="_blank" rel="noopener">Agent Skills</a>
		</div>
	</div>
</nav>

<style>
	nav {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		z-index: 100;
		padding: 0 var(--pad);
		transition: background 0.3s, box-shadow 0.3s, backdrop-filter 0.3s;
	}

	nav.scrolled {
		background: rgba(12, 10, 9, 0.85);
		backdrop-filter: blur(12px);
		-webkit-backdrop-filter: blur(12px);
		box-shadow: 0 1px 0 var(--border);
	}

	.inner {
		max-width: var(--col);
		margin: 0 auto;
		display: flex;
		align-items: center;
		justify-content: space-between;
		height: 56px;
	}

	.wordmark {
		font-family: var(--font-mono);
		font-size: 14px;
		font-weight: 500;
		color: var(--text);
		text-decoration: none;
		letter-spacing: -0.02em;
	}

	.wordmark:hover {
		color: var(--amber);
	}

	.links {
		display: flex;
		gap: 24px;
	}

	.links a {
		font-size: 13px;
		color: var(--muted);
		text-decoration: none;
	}

	.links a:hover {
		color: var(--text);
	}
</style>
