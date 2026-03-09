<footer>
	<div class="inner">
		<div class="left">
			<span class="wordmark">burn-after-login</span>
			<span class="sep">·</span>
			<span class="license">MIT Licensed</span>
		</div>
		<div class="right">
			<a href="https://github.com/pbakaus/burn-after-login" target="_blank" rel="noopener">GitHub</a>
			<a href="https://agentskills.io" target="_blank" rel="noopener">Agent Skills Spec</a>
			<a href="https://gist.github.com/pbakaus/5483f00447725701fd46e1401477dd8c" target="_blank" rel="noopener">Original Gist</a>
		</div>
	</div>
</footer>

<style>
	footer {
		padding: 32px var(--pad);
		border-top: 1px solid var(--border);
	}

	.inner {
		max-width: var(--col);
		margin: 0 auto;
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.left {
		display: flex;
		align-items: center;
		gap: 8px;
	}

	.wordmark {
		font-family: var(--font-mono);
		font-size: 13px;
		color: var(--muted);
	}

	.sep {
		color: var(--faint);
	}

	.license {
		font-size: 13px;
		color: var(--faint);
	}

	.right {
		display: flex;
		gap: 20px;
	}

	.right a {
		font-size: 13px;
		color: var(--muted);
		text-decoration: none;
	}

	.right a:hover {
		color: var(--text);
	}

	@media (max-width: 600px) {
		.inner {
			flex-direction: column;
			gap: 16px;
		}
	}
</style>
