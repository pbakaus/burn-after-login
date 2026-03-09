<footer>
	<div class="inner">
		<div class="left">
			<span class="wordmark">burn-after-login</span>
			<span class="sep">·</span>
			<span class="license">MIT Licensed</span>
		</div>
		<div class="right">
			<a href="https://github.com/pbakaus/burn-after-login" target="_blank" rel="noopener">GitHub</a>
		</div>
	</div>
</footer>

<style>
	footer {
		padding: 24px var(--pad);
		border-top: 1px solid var(--paper-border);
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
		font-size: 12px;
		color: var(--ink-muted);
	}

	.sep {
		color: var(--paper-border);
	}

	.license {
		font-size: 12px;
		color: var(--ink-faint);
	}

	.right {
		display: flex;
		gap: 16px;
	}

	.right a {
		font-size: 12px;
		color: var(--ink-muted);
		text-decoration: none;
	}

	.right a:hover {
		color: var(--ink);
	}

	@media (max-width: 600px) {
		.inner {
			flex-direction: column;
			gap: 12px;
		}
	}
</style>
