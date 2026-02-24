# Claude Consensus Plugin

This is a Claude Code plugin that provides multi-model code review and plan review commands.
Do not modify the command files without understanding the full team-based execution flow.

## Configuration

Commands load model configuration from (in order of priority):
1. `~/.claude/consensus.json` — user config created by `/consensus-setup`
2. `plugins/consensus/consensus.config.json` — plugin defaults

If neither config exists, commands abort with a message to run `/consensus-setup`.

Run `/consensus-setup` to configure which models to use, provider (OpenRouter vs native CLIs), and quorum.

## How It Works

Each command dynamically spawns teammates based on the loaded config. The teammate template is generic — model-specific details (CLI command, resume flag, output file names) come from the config's `command`, `resume_flag`, and `id` fields. Native CLIs (`codex`, `gemini`) are handled as special cases in the template since they have different invocation patterns than the standard Kilo/OpenRouter path.

At runtime, commands verify CLI availability for each configured model and skip unavailable ones. If the remaining models (+ Claude) don't meet `min_quorum`, the command aborts with a clear message.

## Commands

- `/consensus-setup` — Interactive setup wizard for models, API keys, and quorum
- `/code-review` — Multi-model code review with consensus convergence
- `/plan-review` — Multi-model plan review with consensus convergence
