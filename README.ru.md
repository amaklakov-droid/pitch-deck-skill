# Pitch Deck Advisor — Agent Skill

🇬🇧 [Documentation in English](README.md)

Опенсорсный [Agent Skill](https://agentskills.io), который учит Claude,
OpenAI Codex, GitHub Copilot, Cursor, Gemini CLI и любой другой агент с
поддержкой открытого стандарта `SKILL.md` **создавать, проверять и улучшать
инвестиционные питч-деки** — со специализацией на **высокотехнологичных /
deep-tech проектах**.

```bash
npx skills add amaklakov-droid/pitch-deck-skill
```

Skill дистиллирует рекомендации Y Combinator («Legible, Simple, Obvious»
Кевина Хейла), шаблон Sequoia Capital (по которому собран знаменитый дек
Airbnb), практику a16z, исследования DocSend по 200 000+ взаимодействий
инвесторов с деками и рекомендации deep-tech фондов (Khosla Ventures, Julian
Capital / Deep Checks, Hard2beat).

## Что умеет

- **Режим создания** — собирает послайдовую структуру дека (заголовки-тезисы,
  контент в пределах ~50 слов на слайд, заметки для спикера) вокруг 5–7 идей,
  которые инвестор должен запомнить, с калибровкой под стадию (pre-seed /
  seed / Series A).
- **Режим ревью** — аудит готового дека: «тест на 3 минуты», 18 автоматических
  красных флагов (запрос NDA, exit-слайд, «у нас нет конкурентов», valuation,
  метрики тщеславия…), структурные пробелы и проблемы формы — каждая находка
  с конкретным исправлением.
- **Хирургия слайдов** — переписывание отдельных слайдов (проблема, рынок,
  трекшн, команда, ask).
- **Deep-tech специфика** — научный vs инженерный риск, «доказано / осталось
  доказать», слайд IP/moat, milestone-based derisking, смешанное
  финансирование FOAK, beachhead-рынки.

## Структура

```
pitch-deck-advisor/
├── SKILL.md                        # Основной workflow, принципы, канон 12 слайдов
└── references/
    ├── slide-guide.md              # Детальный гид по каждому слайду
    ├── deep-tech.md                # Специфика deep tech / hard tech
    └── review-checklist.md         # Полный чек-лист, 18 красных флагов, калибровка по стадиям
.claude-plugin/
├── plugin.json                     # Манифест плагина Claude Code
└── marketplace.json                # Позволяет репозиторию работать как маркетплейс плагинов
AGENTS.md                           # Гид по репозиторию для AI-агентов и контрибьюторов
llms.txt                            # Машиночитаемый индекс для LLM-краулеров
```

Skill использует progressive disclosure: модель загружает `SKILL.md` при
срабатывании, а справочные файлы читает только по необходимости — это
экономит контекст.

## Установка

Skill соответствует открытому стандарту [Agent Skills](https://agentskills.io),
поэтому одна и та же папка работает в любом агенте с поддержкой `SKILL.md`.

### Быстрая установка (любой агент)

[CLI `skills`](https://github.com/vercel-labs/skills) ставит skill в выбранные
вами агенты (Claude Code, Codex, Copilot, Cursor, Gemini CLI, OpenCode, Goose
и другие):

```bash
npx skills add amaklakov-droid/pitch-deck-skill
```

Глобально (во все проекты) для конкретного агента, например Codex:

```bash
npx skills add amaklakov-droid/pitch-deck-skill -g -a codex
```

### Claude Code (плагин)

Добавьте репозиторий как маркетплейс и установите плагин:

```
/plugin marketplace add amaklakov-droid/pitch-deck-skill
/plugin install pitch-deck-advisor@pitch-deck-skill
```

### Claude Code (ручное копирование)

Персонально (доступен во всех ваших проектах):

```bash
git clone https://github.com/amaklakov-droid/pitch-deck-skill.git
mkdir -p ~/.claude/skills
rm -rf ~/.claude/skills/pitch-deck-advisor
cp -R pitch-deck-skill/pitch-deck-advisor ~/.claude/skills/pitch-deck-advisor
```

> Обратите внимание на явное имя папки-назначения: на macOS `cp` копирует
> *содержимое* папки (а не саму папку), если путь источника заканчивается на
> `/` — а автодополнение по Tab добавляет его автоматически.

Или в конкретный проект: скопируйте папку `pitch-deck-advisor/` в
`<проект>/.claude/skills/`.

### Claude.ai / Claude Desktop

1. Заархивируйте папку `pitch-deck-advisor/` в zip (внутри архива `SKILL.md`
   должен лежать в корне папки).
2. Откройте **Settings → Capabilities → Skills → Upload skill**.

### Claude API / Agent SDK

Загрузите папку через [Skills API](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)
или укажите её в директории skills для Agent SDK.

### OpenAI Codex, GitHub Copilot, Cursor, Gemini CLI, OpenCode (ручное копирование)

Скопируйте `pitch-deck-advisor/` в папку skills нужного агента:

| Агент          | В проекте              | Глобально               |
| -------------- | ---------------------- | ----------------------- |
| OpenAI Codex   | `.codex/skills/`       | `~/.codex/skills/`      |
| GitHub Copilot | `.github/skills/`      | `~/.copilot/skills/`    |
| Cursor         | `.cursor/skills/`      | `~/.cursor/skills/`     |
| Gemini CLI     | `.gemini/skills/`      | `~/.gemini/skills/`     |
| OpenCode       | `.opencode/skills/`    | `~/.config/opencode/skills/` |

Пути соответствуют документации Agent Skills каждого вендора; `npx skills add`
выше подбирает их автоматически.

### Другие LLM

Skill — это обычный Markdown с YAML-frontmatter. Для любой модели с
поддержкой системных промптов или прикрепления файлов: используйте `SKILL.md`
как системный промпт и приложите три файла из `references/` (или вставьте их
в контекст, когда разговор касается deep tech или ревью дека).

## Использование

После установки skill срабатывает автоматически — просто говорите о своём
деке:

- *«Помоги собрать питч-дек для seed-раунда — мы стартап в области материалов
  для батарей, есть два платных пилота»*
- *«Проревьюй этот дек, будь безжалостным»* (приложите PDF)
- *«Перепиши слайд проблемы — инвесторы на нём засыпают»*
- *«Как дек должен измениться между pre-seed и Series A?»*

Skill отвечает на вашем языке (русский, английский или любой другой), при
этом его внутренние инструкции написаны на английском для надёжности работы
с любыми моделями.

## Почему skill на английском, а документация двуязычная

`SKILL.md` — это инструкции для модели, а не для человека. Инструкциям на
английском LLM следуют надёжнее всего, при этом skill на английском спокойно
создаёт деки на русском, если вы общаетесь по-русски. README — для людей,
поэтому они на двух языках (другие языки приветствуются — PR открыты).

## Источники

Y Combinator ([How to Design a Better Pitch Deck](https://www.ycombinator.com/blog/how-to-design-a-better-pitch-deck),
[How to Pitch Your Startup](https://www.ycombinator.com/library/6q-how-to-pitch-your-startup)) ·
[Sequoia — Writing a Business Plan](https://sequoiacap.com/article/writing-a-business-plan/) ·
[DocSend Pitch Deck Research](https://www.dropbox.com/resources/docsend-pitch-deck-research) ·
[Hard2beat — Deeptech Pitch Deck](https://hard2beat.vc/insights-resources/10-slides-you-need-in-your-deeptech-startup-pitch-deck/) ·
[Deep Checks / Julian Capital](https://www.deepchecks.vc/guides/pitching-investors) ·
[CRV — What Investors Look For](https://www.crv.com/content/seed-funding-pitch-deck) ·
[Pitch Deck Hunt](https://www.pitchdeckhunt.com/)

Полная аналитическая записка, из которой дистиллирован skill, — в
[docs/analytical-note.ru.md](docs/analytical-note.ru.md).

## Участие в проекте

Issues и PR приветствуются: документация на новых языках, обновлённые
рекомендации фондов, улучшенные паттерны слайдов, тестовые кейсы. `SKILL.md`
держим на английском и в пределах ~500 строк; глубину выносим в
`references/`.

## Лицензия

[MIT](LICENSE)
