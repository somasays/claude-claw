# ClaudeClaw

A Claude Code plugin that runs an interactive setup wizard to create layered context files for persistent AI identity, domain knowledge, permissions, delegation, and memory. Adapts [OpenClaw](https://github.com/tcsenpai/openclaw)'s layered prompt approach for Claude Code's native architecture.

Instead of a single monolithic system prompt, ClaudeClaw creates focused, purpose-built files that give Claude persistent personality, project understanding, and operational guardrails across sessions.

## What It Creates

| File | Purpose |
|------|---------|
| `.claude/context/SOUL.md` | Identity, role, worldview, beliefs |
| `.claude/context/STYLE.md` | Tone, emoji policy, verbosity |
| `.claude/context/IDENTITY.md` | Auto-generated self-description card |
| `.claude/context/USER.md` | Human profile and preferences |
| `.claude/context/AGENTS.md` | Operating rules, domain ops, delegation framework |
| `.claude/context/TOOLS.md` | Environment, project layout, delegation tooling |
| `.claude/context/HEARTBEAT.md` | Session-start checks and domain health |
| `CLAUDE.md` | Orchestrator with domain knowledge and context file references |
| `MEMORY.md` | Long-term curated memory (native Claude Code path) |
| `.claude/settings.local.json` | Permission scoping (restrictive/balanced/autonomous/custom) |
| `./claw` | Claude delegation script for parallel worktree sessions (optional) |

## Installation

```bash
# Add the marketplace
/plugin marketplace add somasays/claude-claw

# Install the plugin
/plugin install claudeclaw@claude-claw
```

## Usage

```bash
# Full mode (all features)
/claudeclaw

# Minimal mode (identity + permissions only)
/claudeclaw --minimal

# Skip specific sections
/claudeclaw --skip-domain
/claudeclaw --skip-permissions
/claudeclaw --skip-delegation

# With project context hint
/claudeclaw Python API project
```

The wizard asks **one question at a time** with sensible defaults. Press enter to accept a default, type your answer to override, or type `skip` to skip any question.

## Features

### Context Source Scanning
ClaudeClaw scans your project for `README.md`, `package.json`, `pyproject.toml`, test configs, `.env.example`, and directory structure. Discovered info becomes defaults in the interview, so you confirm rather than type.

### Domain Knowledge
The wizard captures your project's architecture, tech stack, key files, critical rules, development workflow, and operational commands. All of this lands in `CLAUDE.md` as structured context.

### Permission Presets
Choose from four presets for `.claude/settings.local.json`:
- **Restrictive** -- Read-only, ask for everything else
- **Balanced** -- Read + edit + test + basic git (default)
- **Autonomous** -- Full access except destructive operations
- **Custom** -- Walk through each permission category

### Claude Delegation
Set up a `./claw` script that launches child Claude instances with scoped permissions:

```bash
./claw run analyze "How does the auth system work?"
./claw run fix-commit -f docs/bugs/BUG1.md
./claw presets
```

Presets: `analyze` (read-only), `fix` (edit+test), `fix-commit` (edit+test+git), `full` (everything except destructive ops).

**Git auto-detection**: If the target directory is a git repo, children run in isolated worktrees. If not, they run in-place. Use `--worktree` or `--no-worktree` to override.

**Cross-repo support**: The claw script can live in a separate "hub" repo while targeting a different directory. Spec file paths (`-f`) are resolved to absolute, so specs can live anywhere.

**Auto-delegation**: Agents can auto-invoke `analyze` (read-only) without confirmation. Modifying presets require asking first.

### Two Modes

**Full mode** (default): 14-step wizard covering identity, style, domain knowledge, user profile, agent rules, permissions, delegation, tools, and heartbeat checks.

**Minimal mode** (`--minimal`): Just identity, user profile, and permissions. Creates `SOUL.md`, `USER.md`, a minimal `CLAUDE.md`, and `settings.local.json`.

## Example Output

```
ClaudeClaw setup complete! Here's what I created:

  .claude/context/SOUL.md          -- Identity, role, and worldview
  .claude/context/STYLE.md         -- Communication patterns
  .claude/context/IDENTITY.md      -- Self-description card
  .claude/context/USER.md          -- Your profile
  .claude/context/AGENTS.md        -- Operating rules + delegation framework
  .claude/context/TOOLS.md         -- Environment + project layout
  .claude/context/HEARTBEAT.md     -- Session-start checks
  CLAUDE.md                        -- Rich orchestrator with domain knowledge
  .claude/settings.local.json      -- Permission scoping (balanced preset)
  ./claw                           -- Claude delegation script

Permissions: balanced (edit + test + git commit, ask for pushes)
Delegation: 4 presets (analyze, fix, fix-commit, full)

To customize: edit any file in .claude/context/
To see it in action: start a new Claude Code session
```

## License

MIT
