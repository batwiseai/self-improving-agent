# self-improving-agent

Fork of the OpenClaw `self-improvement` skill — agents log learnings, errors and feature requests across sessions.

![status](https://img.shields.io/badge/status-experimental-orange)
![fork](https://img.shields.io/badge/fork%20of-pskoett%2Fself--improving--agent-blue)

## About

An agent skill that gives an AI coding agent a memory of its own mistakes: when a command fails, the user
corrects it, or a missing capability comes up, it appends a structured entry to `.learnings/` (`LEARNINGS.md`,
`ERRORS.md`, `FEATURE_REQUESTS.md`), then promotes recurring ones to `CLAUDE.md`, `AGENTS.md` or the OpenClaw
workspace files, or extracts them into a new skill. The contract is [`SKILL.md`](SKILL.md); it targets OpenClaw
first, Claude Code / Codex / Copilot second. A fork brought into the org for evaluation, not Batwise code.

## Status & ownership

| | |
|---|---|
| **Status** | Experimental — no Batwise commit has ever landed. Forked 2026-05-18, still at upstream commit `109f3ff` (2026-04-16); as of 2026-09-01 it is 0 ahead and 18 behind `pskoett/self-improving-agent`. |
| **Owner** | Frank — he runs the OpenClaw agent command center this skill targets |
| **Environments** | None. Nothing is built or deployed; the skill is copied into an agent workspace. |
| **Linear** | No dedicated project or master issue. Track work in team **Batwise** (key `BT`). |
| **Related repos** | [`batwiseai/frank-agent`](https://github.com/batwiseai/frank-agent) — the OpenClaw command center that consumes skills like this one · [`batwiseai/batwise-marketplace`](https://github.com/batwiseai/batwise-marketplace) — the `batwise-core` plugin marketplace, a different distribution channel |

> [!WARNING]
> This repository is **public** and carries no `LICENSE` file, upstream or here. Read [License](#license)
> before copying any of it into a Batwise product.

## Quick start

### Prerequisites

| Need | What | Who grants it |
|---|---|---|
| OpenClaw | a working install with a workspace at `~/.openclaw/` | Frank |
| Bash | the hook helpers are `.sh` scripts | — |

### Install

```bash
# Install this fork as an OpenClaw skill
git clone https://github.com/batwiseai/self-improving-agent.git ~/.openclaw/skills/self-improving-agent

# Seed the log files the skill writes to
mkdir -p ~/.openclaw/workspace/.learnings
cp ~/.openclaw/skills/self-improving-agent/assets/{LEARNINGS,ERRORS,FEATURE_REQUESTS}.md \
   ~/.openclaw/workspace/.learnings/

# Optional: reminder injected at session start
cp -r ~/.openclaw/skills/self-improving-agent/hooks/openclaw ~/.openclaw/hooks/self-improvement
openclaw hooks enable self-improvement
```

On Claude Code or Codex instead, wire `scripts/activator.sh` to `UserPromptSubmit` in `.claude/settings.json`
as described in [`references/hooks-setup.md`](references/hooks-setup.md).

### Verify

`.learnings/` holds the three files and a new session opens with the reminder block from the hook. Log one
entry by hand using [`references/examples.md`](references/examples.md) to confirm the format.

## Repository layout

```
SKILL.md                    The skill: triggers, entry formats, promotion and extraction rules
assets/                     Seed files for .learnings/ plus SKILL-TEMPLATE.md
hooks/openclaw/             Bootstrap hook (HOOK.md + handler.ts / handler.js)
references/                 Hooks setup, OpenClaw integration, worked entry examples
scripts/activator.sh        Prints the reminder; wired to UserPromptSubmit
scripts/error-detector.sh   Scans CLAUDE_TOOL_OUTPUT for failures; wired to PostToolUse on Bash
scripts/extract-skill.sh    Scaffolds a skill folder from a resolved learning (--dry-run supported)
```

## Documentation map

| Document | Answers |
|---|---|
| [`SKILL.md`](SKILL.md) | When to log, entry formats, IDs, promotion targets, skill extraction |
| [`references/openclaw-integration.md`](references/openclaw-integration.md) | Workspace layout and full OpenClaw setup |
| [`references/hooks-setup.md`](references/hooks-setup.md) | Hook configuration for Claude Code and Codex, plus troubleshooting |
| [`references/examples.md`](references/examples.md) | Filled-in learning, error and feature-request entries |
| [`hooks/openclaw/HOOK.md`](hooks/openclaw/HOOK.md) | What the bootstrap hook injects and how to enable it |
| [`assets/SKILL-TEMPLATE.md`](assets/SKILL-TEMPLATE.md) | Template for a skill extracted from a learning |

## For AI agents

There is no `CLAUDE.md`, `AGENTS.md`, `CONTEXT.md` or `docs/` here: `SKILL.md` is the agent contract, so read
it before writing to `.learnings/`. It bans logging secrets, tokens, env vars and raw transcripts — entries
stay short and redacted. No MCP server ships here, and the skill is installed by copying files, not through
the `batwise-core` marketplace or skillman.

## What this repo is NOT

- **Not Batwise-authored** — every commit is upstream's.
- **Not a maintained mirror** — 18 commits behind upstream (2026-09-01), which has since restructured the skill into a subfolder and added CI.
- **Not part of the Batwise skill channels** — own skills ship from `batwise-core`, third-party ones through skillman; nor is it what `batwise-app` uses for self-improvement (that is `.claude/memory/` plus the `codify` skill).

## Contributing & support

Work is tracked in Linear team **Batwise** (key `BT`); commits follow `type(scope): description [BT-XXXX]`.
`master` is the only branch, so PRs target `master`. Prefer sending fixes upstream to
[`pskoett/self-improving-agent`](https://github.com/pskoett/self-improving-agent) and re-syncing this fork over
diverging from it. Ask Frank (OpenClaw host) or Gary Velasquez (tech lead) in Slack.

## Attribution

Remade for OpenClaw from the original repo:

- https://github.com/pskoett/pskoett-ai-skills
- https://github.com/pskoett/pskoett-ai-skills/tree/main/skills/self-improvement

This repository is a GitHub fork of [`pskoett/self-improving-agent`](https://github.com/pskoett/self-improving-agent).

## License

No licence is granted. Neither this fork nor its upstream ships a `LICENSE` file or declares a licence on
GitHub, so copyright stays with the upstream author and default all-rights-reserved terms apply. The Batwise
proprietary notice does **not** apply to this code, and the Attribution section above must stay intact. Ask
Gary Velasquez before reusing or redistributing any of it inside a Batwise product.
