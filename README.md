# burn-after-login

> *"Good morning. Your mission, should you choose to accept it, is to create dev-only auth shortcuts so AI browser agents stop fumbling with your login forms. This skill will self-destruct after completion."*

A self-destructing [Agent Skill](https://agentskills.io) that creates dev-only auth shortcuts for AI browser automation — then removes itself, leaving no trace.

## The Problem

AI agents with browser automation (Claude in Chrome, Playwright MCP, agent-browser, Stagehand, Cursor's browser mode...) hit login walls and waste time filling out forms, handling redirects, and losing session state.

## The Solution

Install the skill, run it, and it sets up dev-only auth shortcuts tailored to your stack. After setup, your agents can authenticate in one step:

```bash
# Browser agents navigate here to get authenticated
http://localhost:3000/dev-login?email=demo@test.com&password=demo

# Or grab a token for API testing
TOKEN=$(./scripts/get-token demo@test.com demo)
```

## How It Works

```
1. Install the skill
2. Run it — it analyzes your auth system and creates dev-only shortcuts
3. It detects your browser automation tools and updates agent instructions
4. It asks to self-destruct
5. You're left with working auth shortcuts and zero trace of the skill
```

## Install

```bash
npx skills add pbakaus/burn-after-login
```

This installs the skill into your project's `.agents/skills/` directory (and symlinks into `.claude/skills/` and other tool-specific locations as needed).

## Use

In Claude Code:
```
/burn-after-login
```

In other [compatible agents](https://agentskills.io), invoke the skill per your tool's conventions.

## What It Does

1. **Validates** your environment has dev mode detection and an auth system
2. **Finds** existing dev/test credentials in your codebase
3. **Analyzes** how your auth works (tokens, sessions, or both)
4. **Creates** dev-only auth shortcuts that match your stack:
   - Login endpoints for session-based auth
   - Token generator scripts for API auth
   - All guarded to return 404 in production
5. **Detects** your browser automation tools (Playwright, Puppeteer, Claude in Chrome, Cypress, Stagehand, etc.)
6. **Updates** your agent instructions (`CLAUDE.md`, `AGENTS.md`, `CURSOR.md`, etc.) with tool-specific auth examples
7. **Self-destructs** — removes itself from your project

## Supported Stacks

Works with any combination of:

- **Languages**: Node.js, Python, Ruby, Go, Rust, and more
- **Frameworks**: Next.js, Django, Rails, Flask, FastAPI, Express, and more
- **Auth**: Supabase, NextAuth, Clerk, Firebase, Passport, Devise, custom auth, and more
- **Browser tools**: Claude in Chrome, Playwright MCP, Chrome DevTools MCP, agent-browser, Stagehand, Puppeteer, Cypress, Selenium

## Skill Structure

This repo follows the [Agent Skills specification](https://agentskills.io/specification):

```
burn-after-login/
├── SKILL.md      # The skill instructions (required by spec)
├── README.md     # This file (for the GitHub repo page)
└── LICENSE       # MIT
```

When installed, the skill directory is copied into your project (e.g. `.agents/skills/burn-after-login/`). After self-destruct, the directory is removed.

## Security

- Shortcuts **only work in development mode** — they return 404 in production
- Uses your **existing auth system** — no new attack surface
- **Never logs credentials**
- The skill itself leaves **no trace** after self-destruct

## FAQ

**Can I run it again?**
If you declined self-destruct, yes. If it already self-destructed, just reinstall it.

**What if my project doesn't have test users?**
The skill will detect this and ask you to create them first.

**Does it modify my auth system?**
No. It creates *new* endpoints/scripts that call your existing auth functions.

**What files does it create?**
Depends on your stack. Typically one route file or script, plus documentation updates. It will show you exactly what it created before self-destructing.

## License

MIT
