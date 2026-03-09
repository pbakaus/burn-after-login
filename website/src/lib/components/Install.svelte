<script lang="ts">
	import { onMount } from 'svelte';

	let visible = $state(false);
	let copied = $state(false);
	let el: HTMLElement;

	const cmd = 'npx skills add pbakaus/burn-after-login';

	function copyCommand() {
		navigator.clipboard.writeText(cmd);
		copied = true;
		setTimeout(() => {
			copied = false;
		}, 2500);
	}

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

<section class="install" bind:this={el}>
	<div class="inner" class:visible>
		<h2>Accept Your Mission</h2>

		<button class="cmd-box" onclick={copyCommand}>
			<code>{cmd}</code>
			<span class="copy-label">{copied ? 'Copied — good luck, agent' : 'Copy'}</span>
		</button>

		<div class="usage">
			<p>Then invoke the skill:</p>
			<div class="usage-grid">
				<div class="usage-item">
					<span class="tool">Claude Code</span>
					<code>/burn-after-login</code>
				</div>
				<div class="usage-item">
					<span class="tool">Cursor</span>
					<code>@burn-after-login</code>
				</div>
				<div class="usage-item">
					<span class="tool">Other agents</span>
					<code>per your tool's conventions</code>
				</div>
			</div>
		</div>

		<p class="footnote">This website will not self-destruct. But the skill will.</p>
	</div>
</section>

<style>
	.install {
		padding: 100px var(--pad) 80px;
		text-align: center;
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
		margin-bottom: 40px;
	}

	.cmd-box {
		display: inline-flex;
		align-items: center;
		gap: 20px;
		background: var(--bg-subtle);
		border: 1px solid var(--amber);
		border-radius: 10px;
		padding: 18px 28px;
		cursor: pointer;
		transition: background 0.2s, box-shadow 0.2s;
		font-family: inherit;
	}

	.cmd-box:hover {
		background: rgba(245, 158, 11, 0.05);
		box-shadow: 0 0 30px rgba(245, 158, 11, 0.1);
	}

	.cmd-box code {
		font-family: var(--font-mono);
		font-size: 16px;
		color: var(--amber);
	}

	.copy-label {
		font-size: 13px;
		color: var(--muted);
		white-space: nowrap;
		min-width: 65px;
		transition: color 0.2s;
	}

	.cmd-box:hover .copy-label {
		color: var(--text);
	}

	.usage {
		margin-top: 48px;
		margin-bottom: 56px;
	}

	.usage > p {
		font-size: 14px;
		color: var(--muted);
		margin-bottom: 20px;
	}

	.usage-grid {
		display: flex;
		justify-content: center;
		gap: 32px;
		flex-wrap: wrap;
	}

	.usage-item {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 6px;
	}

	.tool {
		font-size: 12px;
		font-weight: 500;
		text-transform: uppercase;
		letter-spacing: 0.08em;
		color: var(--muted);
	}

	.usage-item code {
		font-family: var(--font-mono);
		font-size: 14px;
		color: var(--text);
		background: var(--bg-subtle);
		padding: 6px 14px;
		border-radius: 6px;
		border: 1px solid var(--border);
	}

	.footnote {
		font-family: var(--font-serif);
		font-style: italic;
		font-size: 0.95rem;
		color: var(--faint);
	}

	@media (max-width: 680px) {
		.cmd-box {
			flex-direction: column;
			gap: 10px;
			width: 100%;
			padding: 16px 20px;
		}

		.cmd-box code {
			font-size: 13px;
		}

		.usage-grid {
			flex-direction: column;
			align-items: center;
			gap: 20px;
		}
	}
</style>
