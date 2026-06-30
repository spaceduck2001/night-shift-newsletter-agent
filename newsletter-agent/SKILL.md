---
name: newsletter-agent
description: "Build a daily newsletter agent: pick a topic, the agent searches the day's top stories, curates and summarizes them, formats a clean email, and sends it to your inbox every morning. Triggers on: 'newsletter agent', 'daily brief on X', 'email me about X every morning', 'build my first agent'."
category: Automation
audience: marketplace
---

# Newsletter Agent

Your first useful agent. Pick a topic you care about. Every morning, this agent goes out, finds the day's best stories on it, curates them down to what matters, writes a clean brief, and emails it to you. Coffee optional, recommended.

This is the on-ramp skill from the Night Shift episode "Build a Daily Newsletter Agent." It teaches the four moves behind every agent — **job, gather, decide, deliver** — on a build that's fun enough to actually finish.

---

## What you get

- A daily email brief on any topic you choose
- ~5-8 curated items, each with a one-line "why it matters"
- Sent to your inbox at a time you pick
- A format you control (you can make it longer, shorter, punchier)

## The four pieces (the mental model you keep)

| Piece | In this skill | In every agent |
|---|---|---|
| **Job** | "Brief me on `{TOPIC}` every morning." | The instruction / goal |
| **Gather** | Web search for today's top stories | Tools / data access |
| **Decide** | Curate, rank, summarize — your taste lives here | Reasoning / judgment |
| **Deliver** | Format as email, send via GWS | Output / action |

---

## Setup (one time)

### Step 0 — Pick your topic

`TOPIC` is the only one you have to touch — the creator fills the rest when it builds your instance.

```
TOPIC     = "AI and Claude news"   # or "World Cup", "commercial real estate", whatever you'll read
RECIPIENT = "you@example.com"
CADENCE   = "daily"                # "daily" or "weekly" — sets the research lookback (see Gather)
SEND_TIME = "07:00"                # daily → every morning; weekly → e.g. Mondays 07:00
ITEMS     = 6                      # how many stories per brief
SOURCES   = []                     # the confirmed source set (the creator researches + fills this)
BRAND     = {}                     # voice + visual (the creator fills this; else a clean default)
```

Pick something you actually enjoy. The fun is what gets you to finish the build — and a finished agent beats a perfect idea you never shipped.

---

## Run (what the agent does each morning)

### Step 1 — Gather

Pull the top stories on `{TOPIC}` from your `{SOURCES}` (or broad web search if none are set). **Lookback window = `{CADENCE}`:** daily looks at the last 24h, weekly at the prior week — keep research in sync with the schedule.

```
For each source in SOURCES (else web search):
  Pull: title, source, link, published time, a 1-2 sentence gist
  Window: last 24h (daily) / last 7 days (weekly)
Target: 12-20 raw candidates before curation
```

### Step 2 — Decide (curation — the taste step)

From the raw candidates, keep the best `{ITEMS}`. Rank by:
- **Signal over noise** — real developments beat hot takes and rumors
- **Freshness** — within the lookback window, not recycled
- **Variety** — don't return six versions of the same story; dedupe
- **Relevance to `{TOPIC}`** — drop the tangents

**Use the sources the way they were set up:** where they overlap, *corroborate* a claim across them before trusting it (never run a shaky single-source item); where they're additive, *weave* the different angles together. For each keeper, write a tight summary (2-3 sentences) plus a one-line **"Why it matters."** That "why" line is the value — it's the part a raw feed can't give you.

### Step 3 — Format

Build a clean, scannable HTML email — title, date, then each item as a bolded headline + summary + why-line + source link. No walls of text. **Apply `{BRAND}`:** write the summaries in the brand voice (tone, pacing), and style the template in the brand visual (colors, fonts) where one is set; otherwise use the clean editorial default. (Template: `email-template.html` in this skill.)

### Step 4 — Deliver

Send via the GWS CLI (already authenticated). Build the MIME, base64url-encode it, send:

```bash
MIME="From: me\r\nTo: {RECIPIENT}\r\nSubject: Your {TOPIC} brief — $(date +%Y-%m-%d)\r\nMIME-Version: 1.0\r\nContent-Type: text/html; charset=utf-8\r\n\r\n${HTML_BODY}"
ENCODED=$(echo -n "$MIME" | base64 -w 0 | tr '+/' '-_' | tr -d '=')
gws gmail users messages send --params '{"userId": "me"}' --json "{\"raw\": \"${ENCODED}\"}"
```

(Full GWS pattern: `docs/context/gws-cli.md`. Same send path the `daily-brief` skill uses.)

---

## Make it daily (the schedule)

Run it once by hand first and check the email. When it looks right, schedule it.

In Wilson OS, add a cron to your agent's `cron-registry.json` and sync:

```json
{
  "id": "newsletter-{topic-slug}",
  "schedule": "0 7 * * *",
  "command": "run newsletter-agent for {TOPIC}",
  "enabled": true
}
```
```bash
tools/sync_crons.py --agent <yourname> --apply
```

(Cron changes are approval-gated — see `docs/architecture/cron-execution.md`. Outside Wilson OS, any scheduler works: cron, Task Scheduler, a Claude scheduled run.)

---

## Swap the topic (10 seconds)

Change `TOPIC` and re-run. The whole agent re-points at the new subject — World Cup today, your industry tomorrow. That's the lesson: the four pieces stay; only the job changes.

---

## Rules

1. **Never fabricate a story or a source.** If search returns nothing solid, send a short "quiet day on `{TOPIC}`" note rather than inventing items.
2. **Always link the source.** A brief the reader can't verify isn't a brief.
3. **Curate, don't dump.** The value is in what you leave out. `{ITEMS}` is a ceiling, not a quota — fewer strong items beats padding.
4. **One topic per agent.** Want two topics? Run two newsletter agents. Keeps each brief focused.
5. **Test before you schedule.** Run once, read the email, then turn on the cron.

---

## Extend (after your first win)

- Add a second source type (RSS, a specific site, an API) to the gather step.
- Add a "trend over the week" section that remembers what it sent.
- Point the same four pieces at something that saves you real work — a competitor watch, a deal-flow digest, an inbox triage. That's the next episode.
