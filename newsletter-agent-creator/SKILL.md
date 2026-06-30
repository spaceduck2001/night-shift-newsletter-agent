---
name: newsletter-agent-creator
description: "Meta-skill: interview the user, then generate a personalized daily newsletter agent tailored to their topic, sources, taste, and cadence — ready to drop into routines. The no-friction front door to the newsletter-agent skill. Triggers on: 'build me a newsletter agent', 'I want a daily brief but not sure where to start', 'set up a newsletter agent for me', 'make me an agent that emails me about X'."
category: Automation
audience: marketplace
---

# Newsletter Agent Creator (meta-skill)

The front door. Most people don't bounce off building an agent because the config is hard — they bounce because they don't know *what their newsletter should even be.* This skill removes that. It interviews the user, then generates *their* personalized `newsletter-agent` — topic, sources, taste rules, cadence, format — ready to drop into routines.

It pairs with `newsletter-agent` (the static skill): this one **asks the questions and generates**; that one **runs**. The output of this skill IS a filled-in `newsletter-agent` instance.

> **Why a meta-skill beats a blank template:** the static skill makes the user fill in blanks (friction, doubt — "is this the right topic?"). This one does the thinking *with* them. For the goal — get a fast rep, build confidence — lower friction means more people actually finish. And the interview is exactly where the user injects *their* bias, which is the whole point: the agent ends up answering to them, not to whoever's newsletter they'd otherwise follow.

---

## Step 0 — Load before starting

- `skills/newsletter-agent/SKILL.md` — the target skill this one generates an instance of. You must know its shape (Job / Gather / Decide / Deliver, the config block, the email template) to fill it in.
- If running inside a brand context, `brands/{brand}/brand-voice.md` for the brief's tone.

---

## The interview (ask these, adapt to their answers)

Ask conversationally, not as a form. 4 core questions, 2 optional. Stop when you have enough to generate — don't interrogate.

**Core:**
1. **Topic.** "What's one subject you wish you just stayed on top of, without doing the work yourself? Could be your industry, a competitor, a market, a hobby, AI news — anything you'd want a daily pulse on." → sets `TOPIC`.
2. **What 'matters' to you.** "When you scan news on this, what actually matters to you — and what's noise you'd want filtered out?" → sets the **curation bias** (the most important answer — this is where their bias goes in).
3. **Sources / trust.** "Anywhere specific you trust for this, or anywhere you want avoided?" → tunes the gather + dedupe step. (Optional if they don't know — default to broad web search.)
4. **Cadence + delivery.** "Every morning, or a few times a week? What time? Where should it land — which email?" → sets `SEND_TIME`, schedule, `RECIPIENT`.

**Optional (ask only if useful):**
5. **Depth.** "Quick headlines, or a few sentences of why-it-matters on each?" → sets summary length + `ITEMS`.
6. **Voice.** "Want the brief neutral, or with a point of view?" → sets brief tone.

---

## Generate

From the answers, produce a ready-to-run `newsletter-agent` instance:

1. **Fill the config block** (`TOPIC`, `RECIPIENT`, `SEND_TIME`, `ITEMS`) from answers 1, 4, 5.
2. **Write the curation bias** into the Decide step — turn answer 2 into explicit "keep / drop" rules. This is the personalization that matters most. Example: "Keep: funding rounds, product launches, regulatory moves. Drop: opinion pieces, rumors, anything older than 24h."
3. **Tune the Gather step** from answer 3 (preferred/avoided sources).
4. **Set the brief tone** from answer 6.
5. **Output the schedule snippet** for routines (cron line) per the user's cadence.
6. **Hand back:** the filled config + curation rules + the schedule line, and a one-line "here's your agent, run it once to test, then turn on the schedule."

---

## Output format

```
## Your Newsletter Agent: {TOPIC}

CONFIG
  TOPIC      = "{topic}"
  RECIPIENT  = "{email}"
  SEND_TIME  = "{time}"
  ITEMS      = {n}

YOUR CURATION BIAS (what 'matters' to you)
  Keep:  {from answer 2}
  Drop:  {from answer 2}
  Sources: {preferred / avoided from answer 3}

SCHEDULE (drop into routines)
  {cron line}

Run it once to test. Read the email. Then turn on the schedule.
```

---

## Rules

1. **The interview is the product.** Don't skip it and guess. The tailored curation bias (answer 2) is what makes the output theirs, not generic.
2. **Stop interviewing when you have enough.** 4 answers is usually plenty. Don't make it a chore — the whole pitch is "fast and easy."
3. **Generate a real, runnable instance** — not advice about how to build one. Output the filled config + rules + schedule line, ready to paste.
4. **Their bias, not yours.** Encode what *they* said matters. Don't substitute your own editorial judgment for theirs.
5. **One topic per agent.** Two topics → run the interview twice, generate two agents.

---

## Pairs with

- `newsletter-agent` — the skill this generates an instance of (the runtime).
- Episode: "Build a Daily Newsletter Agent" (`newsletter-agent-explainer`) — this meta-skill is the on-screen hero: the interview is more compelling to watch than editing a config block, and it shows the "your bias" point live.
