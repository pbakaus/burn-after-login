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
		<a href="/" class="wordmark">
			<svg class="logo" width="20" height="20" viewBox="0 0 32 32" fill="none" xmlns="http://www.w3.org/2000/svg">
				<rect width="32" height="32" rx="6" fill="#b91c1c"/>
				<path d="M13.5 24c-2-1.5-3.5-4-3.5-6.5 0-3.5 2.5-7 4.5-9.5.5 2 1.5 3.5 3 4.5-.2-2.5.5-5 2-7.5 2 3 4.5 6.5 4.5 11 0 3.5-2 6.5-5 8-1 .5-1.5-.5-1-1.5.5-1 .5-2.5 0-3.5-.8 1.5-2 3-2 4.5 0 .5-.3 1-1 .8-.5-.1-1-.5-1.5-.8z" fill="#fff" opacity="0.95"/>
			</svg>
			burn-after-login
		</a>
		<div class="links">
			<a href="https://github.com/pbakaus/burn-after-login" target="_blank" rel="noopener">GitHub</a>
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
		transition: background 0.3s, box-shadow 0.3s;
	}

	nav.scrolled {
		background: rgba(245, 240, 232, 0.92);
		backdrop-filter: blur(10px);
		-webkit-backdrop-filter: blur(10px);
		box-shadow: 0 1px 0 var(--paper-border);
	}

	.inner {
		max-width: var(--col);
		margin: 0 auto;
		display: flex;
		align-items: center;
		justify-content: space-between;
		height: 52px;
	}

	.wordmark {
		font-family: var(--font-mono);
		font-size: 13px;
		font-weight: 500;
		color: var(--ink);
		text-decoration: none;
		letter-spacing: -0.02em;
		display: flex;
		align-items: center;
		gap: 8px;
	}

	.logo {
		flex-shrink: 0;
	}

	.wordmark:hover {
		color: var(--stamp-red);
	}

	.links {
		display: flex;
		gap: 20px;
	}

	.links a {
		font-size: 13px;
		color: var(--ink-muted);
		text-decoration: none;
	}

	.links a:hover {
		color: var(--ink);
	}
</style>
