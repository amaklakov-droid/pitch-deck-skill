# Pitch Deck Advisor — Agent Skill

🇷🇺 [Документация на русском](README.ru.md)

An open-source [Agent Skill](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)
that teaches Claude (and other LLMs supporting the skill format) how to
**build, review, and improve investor pitch decks** — with deep specialization
in **high-tech / deep-tech projects**.

The skill distills guidance from Y Combinator (Kevin Hale's "Legible, Simple,
Obvious"), Sequoia Capital's business-plan template (the one behind Airbnb's
famous deck), a16z practice, DocSend's research on 200,000+ investor
interactions, and deep-tech funds (Khosla Ventures, Julian Capital / Deep
Checks, Hard2beat).

## What it does

- **Create mode** — builds a slide-by-slide deck outline (assertion headlines,
  ~50-word slide content, speaker notes) around the 5–7 ideas an investor must
  remember, calibrated to your stage (pre-seed / seed / Series A).
- **Review mode** — audits an existing deck: the 3-minute test, 18 automatic
  red flags (NDA requests, exit slides, "no competitors", valuation, vanity
  metrics…), structural gaps, and form issues — each with a concrete fix.
- **Slide surgery** — rewrites individual slides (problem, market sizing,
  traction, team, ask).
- **Deep-tech aware** — scientific vs. engineering risk, "proven vs. to
  prove", IP/moat slides, milestone-based derisking, blended FOAK financing,
  beachhead markets.

## Structure

```
pitch-deck-advisor/
├── SKILL.md                        # Core workflow, principles, 12-slide canon
└── references/
    ├── slide-guide.md              # Detailed per-slide guidance & failure modes
    ├── deep-tech.md                # Deep tech / hard tech specifics
    └── review-checklist.md         # Full checklist, 18 red flags, stage calibration
```

The skill uses progressive disclosure: the model loads `SKILL.md` when
triggered and reads reference files only when needed, keeping context usage
low.

## Installation

### Claude Code (CLI / desktop)

Personal (available in all your projects):

```bash
git clone https://github.com/amaklakov-droid/pitch-deck-skill.git
mkdir -p ~/.claude/skills
rm -rf ~/.claude/skills/pitch-deck-advisor
cp -R pitch-deck-skill/pitch-deck-advisor ~/.claude/skills/pitch-deck-advisor
```

> Note the explicit destination folder name: on macOS, `cp` copies a
> directory's *contents* (not the directory itself) when the source path ends
> with a `/` — which shell tab-completion adds automatically.

Or per-project: copy `pitch-deck-advisor/` into `<project>/.claude/skills/`.

### Claude.ai / Claude Desktop

1. Zip the `pitch-deck-advisor/` folder (the zip must contain `SKILL.md` at
   the folder root).
2. Go to **Settings → Capabilities → Skills → Upload skill**.

### Claude API / Agent SDK

Upload the folder via the [Skills API](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)
or pass it to the Agent SDK's skills directory.

### Other LLMs

The skill is plain Markdown with YAML frontmatter. For any model that accepts
system prompts or file attachments: use `SKILL.md` as the system prompt and
attach the three files from `references/` (or inline them when the
conversation touches deep tech or deck review).

## Usage

Once installed, the skill triggers automatically. Just talk about your deck:

- *"Help me build a pitch deck for my seed round — we're a battery-materials
  startup with two paid pilots."*
- *"Review this deck and be brutal"* (attach the PDF)
- *"Rewrite my problem slide — investors keep zoning out on it."*
- *"How should my deck change between pre-seed and Series A?"*

The skill answers in your language (English, Russian, or any other), while its
internal instructions stay in English for reliability across models.

## Why the skill is in English but the docs are bilingual

`SKILL.md` is an instruction set for the model, not for humans — English
instructions are followed most reliably across LLMs, and an English skill
still produces decks in whatever language you speak. READMEs are for humans,
so they ship in English and Russian (more languages welcome — PRs open).

## Sources

Y Combinator ([How to Design a Better Pitch Deck](https://www.ycombinator.com/blog/how-to-design-a-better-pitch-deck),
[How to Pitch Your Startup](https://www.ycombinator.com/library/6q-how-to-pitch-your-startup)) ·
[Sequoia — Writing a Business Plan](https://sequoiacap.com/article/writing-a-business-plan/) ·
[DocSend Pitch Deck Research](https://www.dropbox.com/resources/docsend-pitch-deck-research) ·
[Hard2beat — Deeptech Pitch Deck](https://hard2beat.vc/insights-resources/10-slides-you-need-in-your-deeptech-startup-pitch-deck/) ·
[Deep Checks / Julian Capital](https://www.deepchecks.vc/guides/pitching-investors) ·
[CRV — What Investors Look For](https://www.crv.com/content/seed-funding-pitch-deck) ·
[Pitch Deck Hunt](https://www.pitchdeckhunt.com/)

The full analytical note the skill was distilled from is in
[docs/analytical-note.ru.md](docs/analytical-note.ru.md) (Russian).

## Contributing

Issues and PRs are welcome: new language docs, updated fund guidance, better
slide patterns, eval cases. Keep `SKILL.md` in English and under ~500 lines;
put depth into `references/`.

## License

[MIT](LICENSE)
