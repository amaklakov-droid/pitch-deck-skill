# AGENTS.md

This repository contains **Pitch Deck Advisor**, an open-source Agent Skill
(the [agentskills.io](https://agentskills.io) `SKILL.md` format, originally
introduced by Anthropic for Claude). It teaches an AI agent to build, review,
and improve investor pitch decks for startups, with deep specialization in
high-tech / deep-tech projects.

## Where the skill lives

```
pitch-deck-advisor/
├── SKILL.md                      # entry point: frontmatter + core workflow
└── references/
    ├── slide-guide.md            # per-slide guidance and failure modes
    ├── deep-tech.md              # deep tech / hard tech specifics
    └── review-checklist.md       # full checklist, 18 red flags, stage calibration
```

`SKILL.md` is loaded when the skill triggers; the `references/` files are
read on demand (progressive disclosure).

## How to install it

- Any agent supporting the open Agent Skills standard (Claude Code, OpenAI
  Codex, GitHub Copilot, Cursor, Gemini CLI, OpenCode, Goose, and others):
  `npx skills add amaklakov-droid/pitch-deck-skill`
- Claude Code plugin: `/plugin marketplace add amaklakov-droid/pitch-deck-skill`
  then `/plugin install pitch-deck-advisor@pitch-deck-skill`
- Manual: copy `pitch-deck-advisor/` into your agent's skills directory
  (for example `~/.claude/skills/`, `~/.codex/skills/`, `.cursor/skills/`,
  `.github/skills/`, `.gemini/skills/`, `.opencode/skills/`).

See [README.md](README.md) (English) or [README.ru.md](README.ru.md) (Russian).

## When to use this skill

Trigger it whenever the user wants to create a pitch deck, investor
presentation, or fundraising materials; asks to review or red-flag check an
existing deck; needs help with individual slides (problem, market, traction,
team, ask); or asks how to pitch VCs, angels, or accelerators, even without
the words "pitch deck" ("help me raise a seed round", "prepare for demo day",
"презентация для инвесторов", "питч-дек").

## Contributing guidelines for agents

- Keep `pitch-deck-advisor/SKILL.md` in English and under ~500 lines. Put
  depth into `references/`.
- The skill must answer in the user's language; the instructions stay in
  English for cross-model reliability.
- Documentation is bilingual: any README change goes into both `README.md`
  and `README.ru.md`.
- Bump `version` in `.claude-plugin/plugin.json` and
  `.claude-plugin/marketplace.json` together when the skill changes.
- No build step, no tests. Validate the plugin with `claude plugin validate .`.
