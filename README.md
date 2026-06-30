# The Night Shift — Newsletter Agent

Your first useful AI agent. Pick a topic you care about. Every morning, this agent goes out, finds the day's best stories on it, curates them down to what actually matters, writes a clean brief, and emails it to you. Coffee optional.

This is the build from **The Night Shift** episode *"Build a Daily Newsletter Agent."* Two skills:

| Skill | What it does |
|---|---|
| [`newsletter-agent-creator`](./newsletter-agent-creator/SKILL.md) | The front door. It **interviews you** — topic, what matters vs. noise, sources, when — then generates *your* configured agent. No blank config to fill in. |
| [`newsletter-agent`](./newsletter-agent/SKILL.md) | The runtime the creator generates. Job → Gather → Decide → Deliver. The four moves behind every agent. |

Run the creator once, answer four questions out loud, and it hands you a working agent tailored to you — your topic, your taste, your schedule.

## The method: SHIP

Every skill you build follows four moves — **SHIP**:

- **Show** — give it an example of the output you want.
- **Hook** — connect it to what it needs (your topic, sources, tools). This is its *memory*.
- **Instruct** — your taste. What matters, what's noise. This is what makes it *yours*.
- **Publish** — set it loose so it runs on its own. This is its *routine*.

## How to use it

1. **Get the skills.** Clone or download this repo.
   ```bash
   git clone https://github.com/spaceduck2001/night-shift-newsletter-agent.git
   ```
2. **Add them to your Claude.** Drop the `newsletter-agent-creator/` and `newsletter-agent/` folders into your Claude skills directory (e.g. `~/.claude/skills/` for Claude Code), or add them as skills in your Claude workspace.
3. **Run the creator.** Ask Claude to *"build me a newsletter agent"* (or run the `newsletter-agent-creator` skill). It interviews you and generates your agent.
4. **Run it once to test**, read the email, then turn on the schedule.

> **Note:** the exact "add a skill" step differs slightly across Claude surfaces (Claude Code, Claude.ai, Cowork). If yours isn't covered above, the skill files are plain Markdown — paste the contents into Claude and ask it to follow them. Setup-step verification for Claude Cowork specifically is in progress.

## What you get

- A daily email brief on any topic you choose
- ~5–8 curated items, each with a one-line *why it matters*
- Sent to your inbox at a time you pick
- A format you control

## License

[MIT](./LICENSE) — use it, change it, ship it. Build & ship what works.

---

*Built on The Night Shift. New builds drop in the [Night Shift AI Lab](https://www.skool.com) community — one real agent at a time.*
