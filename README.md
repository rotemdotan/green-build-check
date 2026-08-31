# green-build-check

A Claude Code skill that checks whether something already exists — commercial, free, or open-source — before building it from scratch.

## Why

Building a custom tool when a ready-made solution already covers the job burns tokens and compute for no reason. This skill makes that check automatic: before Claude starts writing implementation code for a nontrivial build, it searches the web, GitHub, and forums (Reddit, Hacker News, Stack Overflow) for prior art, and gives you a short summary so you can decide whether to use something existing, fork it, or build custom.

## What it does

When you ask Claude Code to build, create, make, or write a tool, app, script, bot, integration, dashboard, or automation, the skill:

1. Restates the underlying problem, stripped of your specific tech choices
2. Searches three lanes in parallel:
   - **Commercial / hosted products** — existing tools or SaaS that solve this out of the box
   - **Open source / GitHub** — repos matching the problem, with stars, maintenance status, and license noted
   - **Forums** — what people actually used and recommended on Reddit, Hacker News, and Stack Overflow
3. Presents a short findings summary (3–6 bullets, not a report) before writing any code
4. Lets you choose: use an existing option, fork/extend an open-source project, or build custom

It skips the check for trivial glue code, one-off scripts, or when you explicitly say "just build it."

## Install

Skills for Claude Code live in `~/.claude/skills/` (personal, available in every project) or `.claude/skills/` inside a repo (project-only).

**Clone directly into your personal skills folder:**

```bash
git clone https://github.com/<your-username>/green-build-check ~/.claude/skills/green-build-check
```

**Or, if you're adding it to a specific project only:**

```bash
git clone https://github.com/<your-username>/green-build-check .claude/skills/green-build-check
```

**Or install manually:**

```bash
mkdir -p ~/.claude/skills/green-build-check
cp SKILL.md ~/.claude/skills/green-build-check/SKILL.md
```

No restart needed — Claude Code watches the skills folder and picks up new skills within the current session (start a new session if the top-level `skills/` folder didn't exist before).

### Using it in the Desktop app

The Code tab in Claude Desktop reads the same `~/.claude/skills/` folder as the CLI for local and SSH sessions, so the steps above work there too. It won't appear in the Desktop app's Skills panel (Customize → Skills), which only lists account-synced skills, but it will trigger normally during a Code session.

### Using it via claude.ai (Cowork / cloud sessions)

Cowork and cloud sessions don't read your local `~/.claude/skills/` folder — they load skills enabled for your claude.ai account instead. To use this skill there:

1. Go to **claude.ai → Settings → Capabilities → Skills** (or **Customize → Skills**)
2. Upload `SKILL.md` (or a `.skill` package containing it)
3. Toggle it on

## Verify it's working

Open a project with Claude Code and ask it to build something with an obvious existing equivalent, e.g.:

```
Build me a markdown-to-PDF converter
```

Claude should search for existing tools/libraries first and summarize what it found before writing any code.

## License

Apache 2.0
