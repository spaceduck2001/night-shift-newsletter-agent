---
name: newsletter-agent-creator
description: "Meta-skill: build a personalized daily newsletter agent by running the SHIP method WITH the user — Show what good looks like (pick from examples → success checklist), Hook up the inputs (brand visual + voice as memory, plus researched & recommended data sources + tools), Instruct via an agent-drafted playbook with guardrails the user confirms, then Publish through 3 test rounds and schedule it. The no-friction front door to the newsletter-agent skill. Triggers on: 'build me a newsletter agent', 'I want a daily brief but not sure where to start', 'set up a newsletter agent for me', 'make me an agent that emails me about X'."
category: Automation
audience: marketplace
---

# Newsletter Agent Creator (meta-skill)

The front door. Most people don't bounce off building an agent because the config is hard — they bounce because they don't know *what good even looks like*, and nobody wants to design a newsletter, pick sources, and write rules from scratch. This skill removes all of it. It runs the **SHIP method** *with* the user and does the heavy lifting *for* them — researching, drafting, recommending — and only ever asks them to **confirm**. The output is *their* personalized, tested `newsletter-agent`.

It pairs with `newsletter-agent` (the runtime): this one **runs SHIP and generates**; that one **runs on a schedule**. The output of this skill IS a filled-in, tested, runnable `newsletter-agent` instance.

> **The core idea: the agent predefines, the user confirms.** The agent does the thinking — it proposes examples, researches sources, drafts the recipe and the guardrails, runs the tests. The user reacts. Lower friction = more people actually finish, and a fast finished rep beats a perfect plan they never ship.

---

## Step 0 — Load before starting

- `newsletter-agent/SKILL.md` — the target runtime this generates. Know its shape so you can fill it in.
- The **design guidelines** in this doc (below).
- If running inside a brand context, the brand's visual + voice docs (`brands/{brand}/brand-visuals.md`, `brand-voice.md`).

---

## The method: SHIP (done *with* the user, not handed to them)

Four moves — **Show · Hook · Instruct · Publish** — plus a load-and-analyze beat between Hook and Instruct. Walk them in order. At each step the agent does the work and the user confirms.

---

### Pick the topic (before anything else)

You can't Show examples without a subject, so settle the topic first.

- **Ask:** *"What's one subject you wish you just stayed on top of, without doing the work yourself?"*
- **If they don't know** (a real stuck point), don't push — *discover it.* Ask a few light questions about their professional and personal life: their role and what they're responsible for, what they already keep half an eye on, a market or competitor that affects them, a hobby they'd love a daily pulse on. From that, **recommend 2–3 candidate topics** and let them pick. The goal is one topic they'll actually want to open every day.

---

### S — SHOW · lock what "good" looks like, first

Make success concrete before anything else. Three paths in, depending on the user:

- **They have a vision** → capture it. Turn it into the success sentence + checklist below.
- **They have an example they love** → ask them to paste it, and **extract the success criteria from it** (length, format, tone, sectioning, how it opens).
- **They don't know what they want** (most common) → *don't make them invent it.* Show them **three example briefs to choose from**, plus a short list of elements to think about. They pick one (or mix two), and you derive success from the choice.

**The three example archetypes** — the agent **generates a real, rendered sample of each on the user's actual topic**, so they can scan and read three tangible briefs, not pick a category off a menu. (Use real-world newsletters — Morning Brew, theSkimm, Lenny's — as *inspiration* for the formats, but what the user sees is a fresh sample built for them, populated with plausible items on their topic.)
1. **The Quick Brief** — punchy and skimmable. 5–6 one-line items, bold headline + a single *why-it-matters* line. For "just give me the signal in 2 minutes." (Morning Brew / theSkimm energy.)
2. **The Editorial** — fewer, deeper. 3–4 items, each with 2–3 sentences of context and a point of view. For "depth over breadth." (A curator with a take.)
3. **The Lead + Hits** — a "big one" up top with real context, then 4–5 rapid-fire hits below. The hybrid. For "one thing that matters + stay current."

**Key elements to have them weigh** (so the choice is informed): length / how many items · depth per item (one-liner vs. paragraph) · voice (neutral vs. opinionated) · sections vs. flat list · how visual it is · how often it lands.

**Then define success** from their pick:
- **One sentence:** e.g. *"A scannable morning brief on {TOPIC} — ~5–8 items, headline + gist + a one-line why-it-matters, every source linked, read in under 3 minutes."*
- **A checklist** every send must clear:
  - [ ] On-topic, today's news (nothing older than ~24h)
  - [ ] Curated, not dumped — deduped, signal over hot-takes
  - [ ] Every item has a **why it matters** line
  - [ ] Every item **links its source**
  - [ ] Matches the chosen format + brand voice
  - [ ] Scannable, reads in under 3 minutes

> Once success is locked, the agent can predefine almost everything that follows. That's the point of Show.

---

### H — HOOK · the inputs (the meat of the build)

This is the biggest step. Once the outcome and success are locked, the agent **guides the user through the inputs needed to hit it** — and again, it proposes; the user confirms. Two families: *memory* (what it knows / how it should look and sound) and *data sources* (where the information comes from), plus the *tools* it acts with.

**Memory inputs — brand identity.** If brand assets exist, pull from them. **If they don't, ask** — a couple of light questions, or offer a clean default to accept (don't make a first-timer stall here):
- **Brand visual** — colors, fonts, styles. (From `brand-visuals.md` / brand assets; otherwise ask "got brand colors/fonts, or want a clean editorial default?")
- **Brand voice** — tone, pacing, personality, sentence structure, language. (From `brand-voice.md`; otherwise ask a quick "neutral, or with a point of view? formal or casual?")
- Any context docs the agent should lean on (positioning, glossary, past issues).

**Data sources — researched & recommended by the agent, then confirmed:**
The agent **researches and recommends the best 3–5 sources** for `{TOPIC}`, then the user confirms ~3. How the sources relate **depends on the topic** — name the role of each:
- **Corroborate** — overlapping sources you cross-check against each other (kills hallucination + single-outlet bias). Right for hard news, claims, numbers.
- **Enrich / additive** — sources that cover *different* angles or slices and complement each other (e.g. one for results, one for analysis, one for the human-interest/community take). Right for broad or multi-faceted topics.
- Most topics want a mix of both. The agent picks the right blend and says why.

The recommended set must include:
- **At least one free source** — Reddit, official-site scraping, or RSS feeds.
- **At least one connection-based source** — something via MCP / API. Favor data-rich providers (Tavily, Apify, and similar — free or paid).
- Round out with whatever else fits the topic (newsletters, lab blogs, a niche aggregator).
For each recommendation, say *why* it's good for this topic and *what it costs / what it needs* (free vs. a key/connection). The user approves the final set.

**Tools — what it acts with:** web search/fetch, email send (Gmail / GWS), a scheduler.

---

### Step 0 (build-time) — LOAD & ANALYZE INPUTS

With everything hooked up, the agent **loads and analyzes all the inputs** before writing the recipe: read the brand visual + voice, test that each confirmed source actually returns usable data, and note the shape of what comes back. This is the agent making sure the inputs can actually produce the success-criteria output — *before* it commits to a playbook.

---

### I — INSTRUCT · what to do with the inputs (drafted by the agent, confirmed by the user)

Now define the work. Because success is locked and inputs are analyzed, **the agent creates the playbook** — the steps that turn these inputs into the success output — plus the **rules, constraints, and guardrails** that keep inputs landing on the desired output. Then it confirms with the user.

**The playbook (agent drafts):**
1. **Gather** — pull from the confirmed sources. **Lookback window = the schedule cadence:** a daily routine looks at the last 24h, a weekly routine at the prior week. (Keep research and schedule in sync — see Publish.)
2. **Decide** — apply the keep/drop taste rules, dedupe, rank, take the top `{ITEMS}`. Where sources overlap, corroborate a claim before trusting it; where they're additive, weave the different angles together.
3. **Format** — lay it out in the chosen archetype, in brand visual + voice, per the design guidelines below.
4. **Deliver** — send to `{RECIPIENT}` at `{SEND_TIME}`.

**Rules / constraints / guardrails (agent drafts):**
- Never fabricate a story or source; if a claim only appears once, flag it or drop it.
- Cross-check across the confirmed sources before including an item.
- Stay in brand voice and brand visual; hit the design spec.
- Curate, don't dump — `{ITEMS}` is a ceiling, not a quota.
- On a quiet day, send a short "quiet day on {TOPIC}" note rather than padding.

Then **confirm the playbook + guardrails** with the user. They nudge; they don't author it.

---

### What makes a good newsletter (design guidelines the agent applies)

- **Visual** — clean, editorial, calm. One accent color, lots of white space. A publication, not a marketing blast. (Use brand colors/fonts where they exist.)
- **Layout / hierarchy** — header (title + date) → items as a scannable list → each item = **bold headline**, gist, a *why-it-matters* line, then the source link. Most important first. No walls of text.
- **Source placement** — source name + link *directly under each item*, visible and verifiable — not in a footer.
- **Fonts** — brand fonts, or a web-safe email stack (`-apple-system`, Segoe UI, Arial). Two faces max.
- **Color** — neutral text on light, **one** accent for headlines/links. High contrast.
- **Spacing** — roomy line-height (1.4–1.6), clear gaps between items, comfortable margins.
- **Email constraint** — inline styles only (clients strip `<style>`). Start from `newsletter-agent/email-template.html`.

---

### P — PUBLISH · test it 3 times, then schedule it

Don't ship blind. Run **three test rounds** so the agent earns trust before it goes live:

1. **Three practice runs** — run the agent against recent history (e.g. each of the last 3 days). Each round, show the real output and check it against the Show checklist.
2. **Incorporate feedback every round** — the agent folds in the user's preferences and lessons across *all* aspects: curation, sources, voice, visual, layout, length. By round three it should be clearing the checklist on its own.
3. **Schedule it (live, on screen)** — set the routine and show it being created. **Cadence and research window move together:** a daily routine researches the last 24h, a weekly routine the prior week. Example: *every morning at 7* (daily), or *every Monday at 7:00 AM* (weekly digest). (In Wilson OS: a cron in `cron-registry.json` + `tools/sync_crons.py`. Outside it: any scheduler or a Claude scheduled run.)
4. **Hand back:** *"Here's your agent. It cleared the checklist three runs straight, it's in your voice and brand, and it runs at {SCHEDULE}. Swap the topic anytime."*

---

## Output format

```
## Your Newsletter Agent: {TOPIC}

SUCCESS (chosen archetype + what good looks like)
  Format:    {Quick Brief | Editorial | Lead + Hits}
  One-liner: {success sentence}
  Checklist: {the boxes — all must pass each send}

MEMORY
  Brand visual: {colors / fonts / style}
  Brand voice:  {tone / pacing / personality / structure}
  Taste — Keep: {...}   Drop: {...}

DATA SOURCES (cross-checked)
  1. {source} — free ({reddit/RSS/scrape})
  2. {source} — connection ({MCP/API: Tavily/Apify/...})
  3. {source} — {why it fits}

CONFIG
  RECIPIENT = "{email}"   SEND_TIME = "{time}"   ITEMS = {n}

PLAYBOOK + GUARDRAILS
  Gather → Decide (cross-check ≥2) → Format (brand) → Deliver
  Guardrails: no fabrication · verify before include · stay in brand · curate not dump

SCHEDULE
  {cron line}   e.g. Mondays 07:00  /  daily 07:00

Tested 3 rounds against recent days, cleared the checklist, scheduled. Done.
```

---

## Rules

1. **Show first, and don't make them invent it.** If they're unsure, give them 3 examples to choose from; derive success from the choice.
2. **The agent predefines; the user confirms.** Research the sources, draft the recipe, write the guardrails, run the tests — then ask "good?" Don't interrogate.
3. **Hook is the meat.** Brand visual + voice (memory) AND researched data sources (≥1 free, ≥1 connection, cross-checked). Spend the time here.
4. **Their taste, not yours.** Encode what *they* said matters.
5. **Test before you ship.** Three practice rounds, feedback folded in each time, then schedule on screen.
6. **One topic per agent.**

---

## Pairs with

- `newsletter-agent` — the runtime this generates an instance of.
- Episode: "Build a Daily Newsletter Agent" (`newsletter-agent-explainer`) — SHIP is the on-screen method, and this skill is the hero: watching the agent run Show → Hook → Instruct → Publish *with* the host is the whole demo.
