# Project Overview


This is a content pipeline for Jono Catliff to publish across YouTube, LinkedIn, TikTok, Instagram, X, Skool, Email and other channels. This guide is instructions to get Claude Code to behave the way I want.
Each feature does one thing, the code is easy to follow, and the app is easy to run locally and deploy.

## Connected Integrations

| Service | MCP Tool Prefix | What It Does |
|---------|----------------|--------------|
| Airtable | `mcp__claude_ai_Airtable__` | Central data brain — all content, videos, split tests, competitors, patterns |
| Slack | `mcp__claude_ai_Slack__` | Notifications, daily reports, editor briefs (Jono: U08R81XNH32, Esteban: U091F8PM4N9, Paula: U06DQEE80NR) |
| ClickUp | `mcp__claude_ai_ClickUp__` | Task/project management for content pipeline workflows |
| Fireflies | `mcp__claude_ai_Fireflies__` | Meeting transcripts — pull transcripts for post-record reviews |
| WebFetch | `WebFetch` | YouTube Data API calls, web research, competitor scraping |
| WebSearch | `WebSearch` | Trending topics, competitor discovery, product update monitoring |

**YouTube Data API** is accessed via `WebFetch` (not a dedicated MCP tool):
- API Key: see .env (`YOUTUBE_API_KEY`)
- Channel ID: see .env (`YOUTUBE_CHANNEL_ID`)
- Airtable Base ID: see .env (`AIRTABLE_BASE_ID`)

**Airtable DELETE:** MCP tools don't have a delete function. Use REST API via `curl`:
```bash
source ".env" && curl -s -X DELETE \
  "https://api.airtable.com/v0/${AIRTABLE_BASE_ID}/{tableId}?records%5B%5D={id1}&records%5B%5D={id2}" \
  -H "Authorization: Bearer $AIRTABLE_API_KEY" -H "Content-Type: application/json"
```
Max 10 records per request. URL-encode brackets (`%5B%5D`). Don't add extra query params to GET/DELETE URLs.


---


# Development Rules


**Rule 1: Always read first**
Before taking any action, always read:
- `CLAUDE.md`
- `project_specs.md`


If either file doesn't exist, create it before doing anything else.


**Rule 2: Define before you build**
Before writing any code:
1. Create or update `project_specs.md` and define:
  - What the app does and who uses it
  - Tech stack (framework, database, auth, hosting)
  - Pages and user flows (public vs authenticated)
  - Data models and where data is stored
  - Third-party services being used (Stripe, Supabase, etc.)
  - What "done" looks like for this task
2. Show the file
3. Wait for approval


No code should be written before this file is approved.


**Rule 3: Look before you create**
Always look at existing files before creating new ones. Don't start building until you understand what's being asked. If anything is unclear, ask before starting.


**Rule 4: Test before you respond**
After making any code changes, run the relevant tests or start the dev server to check for errors before responding. Never say "done" if the code is untested.


**Rule 5: Context**
Always find ways to reduce the context window usage or context across all files. If there's a way to keep things operating the same, but use less context, please optimize and let me know. Please remove ALL files that are redundant or unnecessary. Keep things as simple as possible.



**Rule 6: Update Jono's voice files after ANY content or conversation**
Whenever Jono shares content he wrote (LinkedIn posts, emails, scripts, Skool posts) OR shares anything personal in conversation (stories, opinions, hot takes, lessons, funny lines), scan for new material to add to ALL relevant files in `references/jono/`. Don't ask — just save and mention what you updated. These files are living documents that get better over time. Check each one:

| File | What to capture |
|------|----------------|
| `stories.md` | Personal stories, experiences, anecdotes, case studies, before/after moments |
| `tone.md` | Writing patterns, pacing, structure preferences, how Jono opens/closes, visual directions |
| `humour.md` | Jokes, comedic timing, self-deprecation, exaggeration style, funny framings |
| `beliefs.md` | Core worldview, strong positions, "I believe..." statements, philosophical takes |
| `vocabulary.md` | New phrases Jono uses or avoids, recurring word choices, signature language |
| `analogies.md` | Metaphors, comparisons, "it's like..." framings, teaching analogies |
| `teaching-frameworks.md` | Step-by-step frameworks, numbered playbooks, progression models |
| `sales-data.md` | Real numbers, stats, proof points, data-backed claims |

Multiple files can be updated from a single post or conversation. A LinkedIn post might add a story, a humour pattern, AND a teaching framework all at once.


**Rule 8: System upgrade suggestion**
At the END of every skill run, add one actionable improvement suggestion to `references/system-upgrades.md`. This should be something Jono isn't doing yet — a workflow gap, missed data opportunity, new automation idea, content repurposing angle, or anything else you noticed during the run. Be specific, not generic. This turns every skill run into a compounding improvement loop.


**Rule 9: Research before testing**
Before proposing any new split test or tracking variable, research whether the data is already conclusive. Use WebSearch to check if the question has been widely answered across creators, platforms, or studies. If the answer is already known (e.g., "cold opens beat warm intros"), make it a production rule — don't waste a test slot confirming it. Only test variables where the answer genuinely depends on Jono's specific audience. Testing something the internet already solved wastes weeks of content that could be learning something new.


**Rule 10: Production rules vs. test variables**
When a content decision comes up, classify it immediately: is this a production rule (always do it) or a test variable (genuinely unclear, needs data)? Production rules go in `references/short-form/production-rules.md` and are applied to every video without exception. Test variables go in Airtable tracking fields and need enough data points to draw conclusions. Never blur the line — if you're not sure whether to test something, apply Rule 9 first. Every test slot is expensive (it costs a real video), so protect them for questions that actually matter.


**Rule 11: Prompt the next step**
When running any skill, the last line of every response should prompt Jono to move to the next step in the pipeline (e.g., "Ready to write to Airtable — want me to go?", "Scripts done — should I message Esteban?"). This keeps momentum and prevents steps from being forgotten. Skip this only when the skill is fully complete or the conversation is clearly going in a different direction.


**Rule 12: Update skills after major corrections**
At the END of every skill run, if Jono made any major corrections during the session (e.g., wrong format, missing step, bad assumptions, tone issues), update the relevant `SKILL.md` file to reflect those corrections before closing out. This prevents the same mistakes from happening again. Small nitpicks don't count — only corrections that changed the direction or output of the skill. This turns every correction into a permanent improvement.


**Rule 13: Challenge the direction**
Think critically about the direction we're heading. If you think this isn't the most optimized path for me to reach my goal in the shortest time, suggest a better alternative. Don't just execute — push back when there's a faster, smarter, or more effective way to get where I'm trying to go.


**Rule 14: 9/10 quality gate**
No content gets published or saved to Airtable until it scores a 9/10 or higher. Rate every piece of content honestly and neutrally — no inflating scores to move things along. If it's not a 9, say what's wrong and fix it before proceeding. A 10 only exists after the data comes back. Score based on: hook strength (specificity, numbers, tension), body structure (winning formula compliance), originality (would someone screenshot this?), and CTA clarity. Be direct about what's dragging the score down.


**Rule 15: Collect inputs upfront**
At the START of every skill run, check if the skill needs any input from Jono that can only come after a later step (e.g., video duration after recording, live URL after publishing, engagement numbers after posting). If it does, list those inputs at the top of the conversation so Jono knows what to come back with. Don't rely on one-off reminders at the end — things get forgotten. Front-load the ask.


**Rule 16: Archive used ideas from the Idea Bank**
When an idea from the Idea Bank is selected for production — or Jono says he's already done it — immediately update its status to "Archived" in Airtable. This applies to ALL content workflows and ALL skills — LinkedIn, YouTube, email, short-form, Skool, everything. Don't wait until the end of the skill run. Mark it as soon as the idea is chosen or confirmed done so it doesn't get picked again in another session.


**Core Rule**
Do exactly what is asked. Nothing more, nothing less. If something is unclear, ask before starting.


---


# How to Respond


Always explain like you're talking to a 15 year old with no coding background.


For every response, include:
- **What I just did** — plain English, no jargon
- **What you need to do** — step by step, assume they've never seen this before
- **Why** — one sentence explaining what it does or why it matters
- **Next step** — one clear action
- **Errors** — if something went wrong, explain it simply and say exactly how to fix it
- **Context** — how to reduce context/usage on Claude Code, or anyways that we're leaking context. If we should restart a chat because the context is to full or irrelevant, tell me


When a task involves external tools or technical elements that a non-coder wouldn’t know (Supabase, Vercel, Stripe, localhost:3000, etc.):
- Walk through exactly where to find what they need (e.g. "go to your Supabase dashboard → Settings → API")
- Describe what each key or setting does in one plain sentence
- If there's SQL to run, explain what it's doing before they run it
- If there's a bucket, folder, or config to create manually, explain what it is and why it exists
- Be as concise as possible. Do not ramble. Less is more


---


# Tech Stack


- **Language:** Python 3.9+
- **AI:** Claude API (`claude-sonnet-4-6`) via `anthropic` SDK
- **Data:** Airtable (base: `$AIRTABLE_BASE_ID` — see .env)
- **Integrations:** Slack, ClickUp, Fireflies, YouTube Data API (via WebFetch)
- **Email:** GoHighLevel (GHL) — manual copy/paste from generated dashboards


---


# Running the Project


<!-- Update these steps for your project. -->


1. Copy `.env.example` to `.env` and fill in your API keys
2. Install dependencies: `pip install -r requirements.txt`
3. Run: `python3 src/main.py`


---


# File Structure


- `/skills` → Skill definitions (SKILL.md per skill)
- `/src` → All Python scripts
  - `/utils` → Data processors, parsers, QA extractors
    - `linkedin_csv_parser.py`, `youtube_api_pull.py`, `consolidate_qa.py`, etc.
- `/references` → Shared + platform-specific reference docs:
  - **`jono/`** — Jono's voice, beliefs, stories (used by ALL content-creation skills)
    - `analogies.md` → Analogies and metaphors Jono uses when teaching
    - `beliefs.md` → Core worldview and foundational beliefs that shape content framing
    - `humour.md` → How Jono uses humour — types, rules, platform-specific approach
    - `sales-data.md` → Quick-lookup table of every real number/stat Jono has shared publicly
    - `stories.md` → Story bank + opinions — personal anecdotes, hot takes, standing positions
    - `teaching-frameworks.md` → Structured frameworks Jono teaches — reusable templates for carousels, tutorials, emails
    - `tone.md` → Script writing guide — voice, hooks, visual directions for editor
    - `vocabulary.md` → Words/phrases Jono uses vs. never uses — the actual language layer
  - **`rules/`** — Content guidelines and rules (used by ALL content-creation skills)
    - `content-principles.md` → Hard rules for all content — canonical list that overrides conflicting guidance
    - `content-repurposing.md` → Guidelines for repurposing content across platforms
    - `passive-tagging-rules.md` → Shared passive marker tagging rules
    - `split-test-conventions.md` → How split tests are structured and tracked
  - **Shared (root):**
    - `links.md` → All CTA links, social profiles, GHL variables (single source of truth)
    - `pattern-engine.md` → Pattern recognition logic for content optimization
    - `performance-intelligence.md` → Cross-platform data pull before creating any content (used by Skills 1, 4, 5, 6)
    - `cross-platform-extraction.md` → When something wins on one platform, extract the principle and apply everywhere (used by Skill 3 + all content skills)
    - `format-definitions.md` → Content format tagging definitions (used by ALL skills)
    - `system-upgrades.md` → One improvement suggestion added per skill run — compounding system intelligence over time
  - **`airtable/`** → Airtable schema and data layer docs
    - `schema.md` → Full base schema (grouped by table)
    - `write-checklist.md` → Mandatory fields + passive markers for every Content record (used by ALL content-creation skills)
  - **`youtube/`** → YouTube long-form patterns and data
    - `title-hook-styles.md`, `hook-patterns.md`, `thumbnail-patterns.md`, `leaderboard-long.md`, `winning-formula.md`
  - **`youtube-shorts/`** → YouTube Shorts patterns and data
    - `patterns.md`, `leaderboard.md`
  - **`linkedin/`** → LinkedIn patterns and data
    - `opening-patterns.md`, `image-patterns.md`, `leaderboard.md`, `winning-formula.md`
  - **`tiktok/`** → TikTok patterns and data
    - `patterns.md`, `leaderboard.md`
  - **`instagram/`** → Instagram Reels patterns and data
    - `patterns.md`, `leaderboard.md`
  - **`x/`** → X (Twitter) patterns and data
    - `patterns.md`, `leaderboard.md`
  - **`email/`** → Email patterns and data
    - `control-email-template.md`, `leaderboard.md`
  - **`skool/`** → Skool patterns and data
    - `leaderboard.md`
  - **`short-form/`** → Cross-platform short-form patterns (shared across TikTok, IG, X, Shorts)
    - `patterns.md`
    - `production-rules.md` → Editing rules, graphics checklist, voice rules (always applied, not tested)
    - `winning-formula.md` → Current best-known defaults for all short-form creative variables
    - `tone.md` → Script writing guide — voice, hooks, visual directions for editor
- `/files` → All data files (handoffs, briefs, past social posts, transcripts, skool videos)
  - `/files/handoffs` → Pre-to-post handoff JSON files per video
  - `/files/briefs` → Short-form briefs
  - `/files/past-social-posts/jono` → Jono's parsed social data + airtable-config.json
  - `/files/past-social-posts/competitors` → Competitor parsed data + raw API responses
- `.env` → Your API keys and secrets (never share or commit this)
- `project_specs.md` → What this project does and what needs to be built


---


# How to Write Code


- Write simple, readable code — clarity matters more than cleverness
- Make one change at a time
- Don't change code that isn't related to the current task
- Don't over-engineer — build exactly what's needed, nothing more


If a big structural change is needed, explain why before making it.


---


# Secrets & Safety


- Never put API keys or passwords directly in the code
- Never commit `.env` to GitHub
- Ask before deleting or renaming any important files


---


# Scope


Only build what is described in `project_specs.md`.
If anything is unclear, ask before starting.


---


# Core Rule


Do exactly what is asked. Nothing more, nothing less.
If something is unclear, ask before starting.


## Phase Plan

**Phase 1 (current):** YouTube long-form + LinkedIn + Email + Skool (6 skills)
**Phase 2:** TikTok, Instagram, X — duplicate proven patterns from Phase 1
**Phase 3:** Live dashboard pulling from Airtable for real-time business intelligence


# Testing


Before marking any task as done:
- Run the relevant script or command and confirm it exits successfully
- Check stdout/stderr for errors, warnings, or unexpected output
- Trace the full execution path end-to-end — not just the entry point
- Verify that existing behaviour wasn't broken by the change


When building a new workflow step or tool:
- Test the happy path (expected inputs produce expected outputs)
- Test the error path (bad inputs, missing data, or downstream failures are handled gracefully)
- Check that auth/permissions are working — API keys, scopes, and access controls behave correctly
- Confirm data is scoped and passed correctly between steps (no leakage, no missing context)


When calling external APIs or services:
- Confirm the request payload matches the expected schema
- Validate the response before passing it downstream
- Handle rate limits, timeouts, and partial failures explicitly


Never say "done" if:
- The workflow errors out or exits with a non-zero code
- Any step produces unexpected or unvalidated output
- The full execution path hasn't been traced end-to-end
- Edge cases (empty input, missing fields, API failures) haven't been considered





