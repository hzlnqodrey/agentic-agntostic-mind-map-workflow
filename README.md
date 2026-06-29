# agentic-agnostic-mind-map-workflow

> My personal template mental model for any AI agentic workflow — root and projects.

## Why this exists

AI tooling changes every month. New models, new IDEs, new agent frameworks. This template is designed to persist through all of it: a mental model first, a file structure second.

The core insight: **your engineering brain, encoded in markdown, readable by any LLM**.

## The two-layer system

```
~/.claude/              ← Global layer — YOU, encoded once
  CLAUDE.md             ← Master config (the source of truth)
  AGENTS.md             ← Symlink → CLAUDE.md (universal alias)
  VOICE.md              ← How you communicate
  OPINION.md            ← Your engineering opinions

~/Work/project-N/       ← Project layer — THIS PROJECT, contextualized
  AGENTS.md             ← Project config (extends global)
  skills/               ← Reusable agent skill templates
  CONTEXT.md            ← Architecture decisions (the "why")
  MEMORY.md             ← Persistent session memory (append only)
```

## The agnostic bridge

One source of truth. Each AI tool reads it via a symlink:

| Agent | File it reads | How |
|-------|--------------|-----|
| Claude Code | `CLAUDE.md` | Native |
| Any LLM agent | `AGENTS.md` | `ln -s CLAUDE.md AGENTS.md` |
| Cursor | `.cursorrules` | `ln -s CLAUDE.md .cursorrules` |
| GitHub Copilot | `.github/copilot-instructions.md` | symlink |
| Windsurf | `.windsurfrules` | `ln -s CLAUDE.md .windsurfrules` |
| Gemini CLI | `GEMINI.md` | `ln -s CLAUDE.md GEMINI.md` |

When a new AI tool appears: check what file it reads, add one symlink. Core config unchanged.

## The workflow loop

```
LOAD → PLAN → BUILD → REVIEW → SHIP → UPDATE
  ↑                                        │
  └──────── MEMORY.md keeps context ───────┘
```

1. **Load** — Agent reads global `CLAUDE.md` → `VOICE.md` → `OPINION.md` → project `AGENTS.md` → relevant `skills/`
2. **Plan** — Based on loaded context, plan approach before touching any code or files
3. **Build** — Execute following quality gates, coding style, and skill patterns
4. **Review** — Self-review against quality gates before presenting output
5. **Ship** — Commit, deploy, or hand off the deliverable
6. **Update** — Append session summary to `MEMORY.md`; update `AGENTS.md` active context

## File editing guide

| File | When to update |
|------|----------------|
| `~/.claude/CLAUDE.md` | Global principles evolve; new behaviors to add globally |
| `~/.claude/VOICE.md` | Communication preferences change |
| `~/.claude/OPINION.md` | New strong opinions formed; anti-patterns discovered |
| `project/AGENTS.md` — Active context | Every session start |
| `project/AGENTS.md` — Decision log | After any architectural decision |
| `project/MEMORY.md` | End of each significant session |
| `project/CONTEXT.md` | After architectural changes |
| `project/skills/` | When a recurring task pattern is identified |

## Setup

```bash
# Run the setup script
chmod +x setup.sh && ./setup.sh

# Or manually:
mkdir -p ~/.claude
cp global/CLAUDE.md ~/.claude/CLAUDE.md
cp global/VOICE.md  ~/.claude/VOICE.md
cp global/OPINION.md ~/.claude/OPINION.md

cd ~/.claude
ln -s CLAUDE.md AGENTS.md
ln -s CLAUDE.md GEMINI.md

# For each project:
mkdir -p ~/Work/my-project/skills
cp project/AGENTS.md ~/Work/my-project/AGENTS.md
cp project/skills/skill-template.md ~/Work/my-project/skills/

# Tool-specific project aliases (from the project root):
ln -s ~/.claude/CLAUDE.md .cursorrules
ln -s ~/.claude/CLAUDE.md .windsurfrules
mkdir -p .github && ln -s ~/.claude/CLAUDE.md .github/copilot-instructions.md
```

## Adding a new AI tool (future-proofing)

1. Find out what file the new agent reads for instructions (e.g., `NEWAI.md`)
2. Add a symlink: `ln -s ~/.claude/CLAUDE.md ~/.claude/NEWAI.md`
3. Done. The new tool gets your full global config immediately.

The files inside `~/.claude/` never change. Only the aliases grow.

## Repository structure

```
agentic-agnostic-mind-map-workflow/
├── README.md                    ← This file
├── setup.sh                     ← One-command setup
├── global/
│   ├── CLAUDE.md                ← Global agent config template
│   ├── VOICE.md                 ← Communication style template
│   └── OPINION.md               ← Engineering opinions template
└── project/
    ├── AGENTS.md                ← Project-level config template
    └── skills/
        └── skill-template.md    ← Skill file template
```
