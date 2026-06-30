---
name: newsletter-agent-creator
description: "Meta-skill: build a personalized daily newsletter agent by running the SHIP method WITH the user — Show what good looks like (success sentence + checklist + example), Hook up the sources (memory + tools), Instruct via an agent-drafted recipe the user just confirms, then Publish. The no-friction front door to the newsletter-agent skill. Triggers on: 'build me a newsletter agent', 'I want a daily brief but not sure where to start', 'set up a newsletter agent for me', 'make me an agent that emails me about X'."
category: Automation
audience: marketplace
---

# Newsletter Agent Creator (meta-skill)

The front door. Most people don't bounce off building an agent because the config is hard — they bounce because they don't know *what good even looks like*, and nobody wants to design a newsletter from scratch. This skill removes both. It runs the **SHIP method** *with* the user and does most of the work *for* them, confirming as it goes — and hands back *their* personalized `newsletter-agent`.

It pairs with `newsletter-agent` (the runtime): this one **runs SHIP and generates**; that one **runs daily**. The output of this skill IS a filled-in, runnable `newsletter-agent` instance.

> **The core idea: the agent predefines, the user confirms.** Once we lock what success looks like (Show), the agent already knows most of the recipe — the sources to wire, the curation logic, even how the email should look. So it *drafts* all of it and just asks the user "good?" — instead of making them author it. Lower friction = more people actually finish. And a fast, finished rep beats a perfect plan they never ship.

---

## Step 0 — Load before starting

- `newsletter-agent/SKILL.md` — the target runtime this generates. Know its shape (Job / Gather / Decide / Deliver, the config block, the email template) so you can fill it in.
- The **design guidelines** in this doc (below) — you'll apply them when you draft the look.
- If running inside a brand context, `brands/{brand}/brand-voice.md` for the brief's tone.

---

## The method: SHIP (done *with* the user, not handed to them)

Build the agent in four moves — **Show · Hook · Instruct · Publish**. Walk them in order. At each step you do the heavy lifting and the user just reacts.

---

### S — SHOW · lock what "good" looks like, first

Before any config, make success concrete. Three things do that — lead with all three:

1. **Define success in one sentence.** Offer a default, let them tweak:
   > "A scannable morning brief on **{TOPIC}** — about 5–8 items, each a headline + a two-sentence gist + a one-line *why it matters*, every source linked, the whole thing readable in under three minutes."

2. **A success checklist** — the bar *every* send has to clear:
   - [ ] On-topic, and it's today's news (nothing older than ~24h)
   - [ ] Curated, not dumped — deduped, signal over hot-takes
   - [ ] Every item has a **"why it matters"** line
   - [ ] Every item **links its source**
   - [ ] Scannable — headline-first, no walls of text
   - [ ] Reads in under 3 minutes

3. **A concrete example.** Show one finished example brief so they *see* the target — OR, better, ask: *"Got a newsletter you already love? Paste it."* Then **extract the success criteria from their example** (format, length, tone, sectioning, how it opens) and fold them into the checklist above. An example teaches faster than any description — and the agent should be able to read one and infer what "good" means here.

> **Why Show comes first:** once the target is locked, the agent can predefine almost everything that follows. The user reacts to a strong default instead of designing from a blank page.

---

### H — HOOK · wire up the sources (memory **and** tools)

"Sources" is two different things — name both:

**Memory sources** — what the agent *knows* and works from:
- **Topic** — the subject it tracks.
- **Taste** — what *matters* vs. what's noise. This is the single most important input; it's where the user's bias goes in, and it's what makes the brief theirs instead of generic.
- **Trusted / avoided outlets**, or any context doc they want it leaning on (optional).

**Tools** — what the agent *uses to act*:
- **Web search** — to gather the day's candidates.
- **Email send** (Gmail / GWS) — to deliver the brief.
- **Scheduler** — to run it every morning on its own.

Confirm each connection. (In the episode, this is the "hooking it up" beat — the agent's memory and its reach. It's where most of the setup time used to go; here the agent walks it.)

---

### I — INSTRUCT · the recipe, drafted by the agent and confirmed by the user

This is the shift from the old "fill in a form" approach. Because Show locked the target, **the agent already knows most of the recipe.** So *draft* it and ask the user to confirm — don't interrogate.

**The playbook (you draft, they confirm):**
1. **Gather** — web search `{TOPIC}`, last 24h, 12–20 raw candidates.
2. **Decide** — apply the taste rules (keep X / drop Y), dedupe, rank, take the top `{ITEMS}`.
3. **Format** — lay it out per the design guidelines below.
4. **Deliver** — send to `{RECIPIENT}` at `{SEND_TIME}`.

**The taste rules** — turn their "what matters" into explicit **keep / drop**. Propose a first draft from the topic (e.g. for AI news: *keep* launches, funding, model releases; *drop* rumors, hot-takes, "X is dead" pieces). The user nudges it; you don't make them write it cold.

**The design spec** — predefine it from the guidelines below and confirm. Most people don't want to pick fonts and spacing — they want it to *look good*. So the agent picks, shows, and they approve.

---

### What makes a good newsletter (the design guidelines the agent applies)

Use these to draft the look, then confirm. The user reacts; they don't spec it.

- **Visual aesthetics** — clean, editorial, calm. One accent color, lots of white space. Reads like a publication, not a marketing blast.
- **Layout / hierarchy** — header (title + date) → items as a scannable list → each item = **bold headline**, two-sentence gist, a *why-it-matters* line (italic or accent color), then the source link. Most important item first. Never a wall of text.
- **Source placement** — source name + link sits *directly under each item*, visible and verifiable — not dumped in a footer.
- **Fonts** — a web-safe stack for email reliability (`-apple-system`, Segoe UI, Arial). One weight for headlines, regular for body. Two faces max.
- **Color** — neutral text on a light background, **one** accent for headlines/links. High contrast, accessible.
- **Spacing** — generous line-height (1.4–1.6), clear gaps between items, comfortable margins. Breathing room is what makes it scannable.
- **Length** — 5–8 items. A brief, not a digest.
- **Email constraint** — inline styles only (clients strip `<style>`). Start from `newsletter-agent/email-template.html`.

---

### P — PUBLISH · ship it

1. **Generate the configured agent** — fill the runtime's config block + the keep/drop taste rules + the design spec + the schedule line.
2. **Run it once now** — produce a *real* sample brief and show it. Check it against the Show checklist, line by line.
3. **If it clears the bar, ship it** — turn on the schedule, or hand over the cron line / download so it runs daily on its own. (In Wilson OS: a cron in `cron-registry.json` + `tools/sync_crons.py`. Outside it: any scheduler, or a Claude scheduled run.)
4. **Hand back:** *"Here's your agent. It clears the checklist, it runs at {SEND_TIME} every morning, and you can swap the topic anytime."*

---

## Output format

```
## Your Newsletter Agent: {TOPIC}

SUCCESS (what good looks like)
  One-liner: {success sentence}
  Checklist: {the 6 boxes — all must pass each send}

CONFIG
  TOPIC      = "{topic}"
  RECIPIENT  = "{email}"
  SEND_TIME  = "{time}"
  ITEMS      = {n}

MEMORY — your curation bias (what 'matters')
  Keep:    {from taste}
  Drop:    {from taste}
  Sources: {trusted / avoided}

DESIGN (predefined, confirmed)
  Layout: header → scannable items (headline · gist · why-it-matters · source link)
  Accent: {color}   Fonts: {stack}   Spacing: roomy (1.5 line-height)

SCHEDULE (drop into routines)
  {cron line}

Sample brief generated + checked against the checklist. Run it once, read it, then turn the schedule on.
```

---

## Rules

1. **Show first.** Lock success before building — the sentence + checklist + example *are* the spec. If the user gives an example, extract the criteria from it.
2. **The agent predefines; the user confirms.** Draft the recipe, the taste rules, and the design — then ask "good?" Don't interrogate. Lower friction is the whole point.
3. **Their taste, not yours.** Encode what *they* said matters. Don't substitute your editorial judgment for theirs.
4. **Generate a real instance + a real sample send** — not advice about how to build one. Output the filled config, rules, design, schedule, and one checked sample.
5. **One topic per agent.** Two topics → run SHIP twice, generate two agents.

---

## Pairs with

- `newsletter-agent` — the runtime this generates an instance of.
- Episode: "Build a Daily Newsletter Agent" (`newsletter-agent-explainer`) — SHIP is the on-screen method, and this skill is the hero: watching the agent run Show→Hook→Instruct→Publish *with* the host is the whole demo.
