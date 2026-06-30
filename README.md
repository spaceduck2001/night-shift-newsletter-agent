# The Night Shift — Newsletter Agent

Your first useful AI agent. Pick a topic you care about. Every morning, this agent goes out, finds the day's best stories on it, curates them down to what actually matters, writes a clean brief, and emails it to you. Coffee optional.

This is the build from **The Night Shift** episode *"Build a Daily Newsletter Agent."* Two skills:

| Skill | What it does |
|---|---|
| [`newsletter-agent-creator`](./newsletter-agent-creator/SKILL.md) | The front door. It runs the **SHIP** method with you — Shows you what good looks like, Hooks up the sources, drafts the recipe for you to confirm, then Publishes. Hands you *your* configured agent. |
| [`newsletter-agent`](./newsletter-agent/SKILL.md) | The runtime the creator generates. Job → Gather → Decide → Deliver. The four moves behind every agent. |

Run the creator once, confirm a few things, and it hands you a working agent tailored to you — your topic, your taste, your schedule, looking the way a good brief should.

## The method: SHIP

Every skill you build follows four moves — **SHIP**:

- **Show** — lock what *good* looks like first: a one-line definition, a success checklist, and a real example. (Got a newsletter you love? Paste it and the agent extracts the criteria.)
- **Hook** — connect what it needs. *Memory* sources (your topic, your taste) **and** *tools* (web search, email, a scheduler).
- **Instruct** — the agent drafts the recipe *and* the design for you, and you just confirm. You shouldn't have to design a newsletter from scratch.
- **Publish** — run it once, check it against the checklist, then set it loose to run on its own.

## How to use it

1. **Get the skills.** Clone or download this repo (green **Code** button → Download ZIP).
   ```bash
   git clone https://github.com/spaceduck2001/night-shift-newsletter-agent.git
   ```
2. **Add them to your Claude.** A skill is just a folder with a `SKILL.md` — adding one takes a few seconds:
   - **claude.ai:** Settings → **Customize → Skills → Upload**. Zip the skill folder first (the folder must sit at the root of the zip), then upload it.
   - **Claude Cowork:** **Customize** (left sidebar) → **+** → **Skills** tab → upload the zipped folder.
   - **Claude Code:** drop the `newsletter-agent-creator/` and `newsletter-agent/` folders into `~/.claude/skills/`.

   Works on **Free, Pro, and Max** — uploaded skills stay private to your account. (Team/Enterprise admins can provision them org-wide.)
3. **Run the creator.** Ask Claude: *"build me a newsletter agent."* It runs SHIP with you and generates your agent.
4. **Run it once to test,** read the email, then turn on the schedule.

> **Heads up:** skills can include instructions and scripts, so treat any skill you install (including this one) like code — give the `SKILL.md` a read before you add it.

## What you get

- A daily email brief on any topic you choose
- ~5–8 curated items, each with a one-line *why it matters*
- Sent to your inbox at a time you pick
- A clean, scannable layout — editorial, not a marketing blast

## License

[MIT](./LICENSE) — use it, change it, ship it. Build & ship what works.

---

*Built on The Night Shift. New builds drop in the [Night Shift AI Lab](https://www.skool.com) community — one real agent at a time.*
