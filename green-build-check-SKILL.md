---
name: green-build-check
description: Before building any nontrivial tool, app, script, integration, automation, or feature from scratch, use this skill to check whether something that already does the job exists — commercial, free, or open-source. Building from scratch when a solution already exists burns tokens and compute for no reason, so this is a sustainability check as much as a time-saver. Trigger this whenever the user asks Claude to "build," "create," "make," or "write" a tool/app/script/bot/integration/dashboard/automation, even if they don't explicitly ask for a comparison first. Search the web, GitHub, and forums (Reddit, Hacker News, Stack Overflow) before writing implementation code. Skip it for trivial one-off scripts, tiny glue code, or when the user explicitly says to just build it / skip the search.
---

# Green Build Check

Before writing implementation code for something substantial, check whether it already exists. Don't silently skip this — it's a required first step, not an optional nicety.

Building custom when a ready-made solution already exists is wasted compute and wasted tokens — reusing what's already out there is the leaner, greener path. That's the whole point of this skill: cheap searches now beat an expensive, redundant build later.

## When to run this

Run the check when the ask is a standalone tool, app, script, bot, integration, dashboard, browser extension, CLI, library, or automation — something with a name and a purpose of its own.

Skip it (and just build) when:
- The user explicitly says "just build it," "skip the search," "I know this doesn't exist," or similar.
- It's a small, throwaway, or highly personal piece of glue code (e.g. "write a script to rename these files," "add a button to my existing app").
- The user is modifying/extending something that already exists in their own codebase.
- This is clearly a learning exercise ("build X to learn how Y works").

If borderline, run a quick check anyway — it's cheap and the user can wave it off.

## Workflow

### 1. Name the problem, not the implementation

Before searching, restate what the user actually needs in one sentence, stripped of their specific tech choices. "A tool that watches my GitHub repo and posts a Slack summary of new PRs daily" — not "a Python script using PyGithub and the Slack SDK." Searching for the underlying job-to-be-done surfaces more relevant prior art than searching for the stack.

### 2. Search in parallel, across three lanes

Use `WebSearch` (or equivalent) for all three — don't rely on training data, which will be stale on what currently exists.

**Commercial / hosted products**
- Query: `<problem> tool`, `<problem> app`, `<problem> saas`
- Look for existing products that solve this out of the box, including free tiers.

**Open source / GitHub**
- Query: `<problem> github`, and directly `site:github.com <problem>`
- If a `gh` CLI or GitHub search tool is available, use `gh search repos <keywords> --sort=stars` for a cleaner signal on maturity (stars, last commit, open issues).
- Note actively maintained vs. abandoned — a repo with no commits in 2+ years is a weaker recommendation even if it matches.

**Forums / community opinion**
- Query: `<problem> reddit`, `<problem> site:news.ycombinator.com`, `<problem> stackoverflow`
- These surface what people actually used and complained about — often better signal than marketing pages for "does this really work."

Run at least one query per lane before concluding nothing exists. If the first queries return nothing relevant, reformulate with different phrasing once before giving up on a lane.

### 3. Compile a short findings summary

Present findings before writing any code, as a compact list — not a full report:

```
Found a few things that might already cover this:

**Existing tools**
- [Name](link) — what it does, free/paid, why it fits or falls short

**Open source**
- [repo](link) — ⭐ stars, last updated, what it covers, license

**What people say**
- One-line takeaway from forum discussion, if there's a clear consensus

My take: [use X as-is / fork Y and modify / nothing quite fits, worth building custom because Z]
```

Keep this tight — 3-6 bullets total, not an essay. If truly nothing relevant turns up after a real search, say so plainly ("Didn't find an existing tool that does this — looks like it's worth building") and move straight to building.

### 4. Let the user decide, then act

Don't build custom by default just because something turned up. Ask which direction they want:
- Use an existing option as-is
- Fork/extend an existing open-source project
- Build custom (and if so, whether to borrow ideas/architecture from what was found)

If the user's intent is already obvious from context (e.g. they've already said "even if something exists I want to build my own for learning/control/customization reasons"), skip the question and proceed with a one-line acknowledgment of what was found instead.

## Notes

- Favor recency in search phrasing — the tool landscape moves fast; a good option today may not have existed when a similar idea was last discussed.
- Don't let this become a stalling tactic — one round of parallel searches across the three lanes is enough. This is a fast sanity check, not a market research project.
- License matters for OSS finds intended for reuse — flag it (MIT/Apache-friendly vs. GPL/AGPL vs. no license) if the user might redistribute or commercialize what they build.
