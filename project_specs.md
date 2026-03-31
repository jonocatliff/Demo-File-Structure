# Project Specs — LinkedIn Posting Skill

## What It Does
A 3-step LinkedIn content pipeline skill for Jono Catliff:
1. **Idea Mining** — Uses Apify to scrape trending LinkedIn posts in the AI automation space, surfaces the best-performing ideas
2. **Post Creation** — Creates a winning LinkedIn post based on the mined ideas, using Jono's voice and proven patterns
3. **Analytics Update** — Tracks daily LinkedIn post performance in Airtable so historical winners inform future posts

## Who Uses It
Jono Catliff — run via Claude Code `/linkedin-posting` skill

## Tech Stack
- **Language:** Python 3.9+
- **AI:** Claude API (`claude-sonnet-4-6`) via `anthropic` SDK
- **Web Scraping:** Apify API (actor: `curious_coder/linkedin-post-search-scraper`)
- **Data:** Airtable (existing base from `.env`)
- **Notifications:** Slack (via MCP)

## How Each Step Works

### Step 1: Idea Mining (Apify)
- Call Apify's LinkedIn Post Search Scraper actor via REST API
- Search for posts about: AI automation, AI agents, Claude, GPT, workflow automation, no-code AI
- Filter for high-engagement posts (sort by reactions/comments)
- Return top 10 posts with: author, post text, engagement metrics, URL
- Present ideas to Jono for selection/inspiration

**Apify API Flow:**
```
POST https://api.apify.com/v2/acts/curious_coder~linkedin-post-search-scraper/run-sync-get-dataset-items
Authorization: Bearer {APIFY_API}
Body: { search URL, limit, filters }
```

### Step 2: Post Creation
- Take selected idea/angle from Step 1
- Pull Jono's voice references (`references/jono/`)
- Pull LinkedIn winning patterns (`references/linkedin/`)
- Pull content principles (`references/rules/content-principles.md`)
- Generate post using Claude API with all context
- Score against 9/10 quality gate (Rule 14)
- Iterate until it passes

### Step 3: Analytics Update
- Jono provides engagement numbers for recent posts (or we pull from Airtable)
- Update Airtable LinkedIn content records with: impressions, likes, comments, reposts, saves
- Recalculate leaderboard rankings
- Surface top 5 performers and any patterns spotted

## Data Models

### Airtable — LinkedIn Content Table (fields needed)
- Post Text
- Date Published
- Topic/Angle
- Source Idea (what inspired it)
- Hook Type
- CTA Type
- Impressions
- Likes
- Comments
- Reposts
- Saves
- Engagement Rate
- Score (1-10)
- Status (Draft / Published / Archived)
- Split Test Variable (if any)
- Split Test Value (if any)

### Airtable — LinkedIn Leaderboard View
- Sorted by engagement rate, filterable by date range
- Used by Step 1 and Step 2 to inform what's working

## Third-Party Services
| Service | Purpose | Auth |
|---------|---------|------|
| Apify | LinkedIn post scraping | `APIFY_API` from `.env` |
| Airtable | Content storage + analytics | MCP tools |
| Slack | Notifications | MCP tools |
| Claude API | Post generation | `anthropic` SDK |

## What "Done" Looks Like
- [ ] Running `/linkedin-posting` walks through all 3 steps
- [ ] Step 1 returns real trending posts from Apify
- [ ] Step 2 produces a 9/10+ LinkedIn post in Jono's voice
- [ ] Step 3 updates Airtable with performance data
- [ ] Analytics leaderboard is queryable for future sessions
- [ ] Skill file (SKILL.md) documents the full workflow
