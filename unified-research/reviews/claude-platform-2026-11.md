# Claude platform review: prompting, plugins, mods, hooks (Nov 2026)

Reviewed 2026-10-31 from official docs. Items I only saw as an index or heading, not a full page, are marked *(index only)*. Nothing here is a measured result on this project; adoption decisions are at the end.

## Prompting guidance for the Claude 5 family
- **Per-model pages exist for Fable 5.1, Mythos 5.1, Fable 5, Opus 5.5, Sonnet 5.5 and Haiku 5.5.** Tune from the model's own page, not generic advice. [Main guide](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
- **Effort is the main thinking and cost control.** Thinking is adaptive and always on for Fable 5.x and Opus 5.5; `budget_tokens` returns a 400 on 4.7+. Level names do not mean the same depth across models, so sweep levels on your own evals. Lowering effort cuts thinking more reliably than "think less" prose. Changing top-level effort between requests invalidates the prompt cache. [Opus 5.5 page](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5-5)
- **Stop and verify paragraphs.** Sonnet 5.5 and Haiku 5.5 pages give short system-prompt text for "keep working until done, then stop and report, no extras" and "run a real check before saying done" (models sometimes report done without testing at low effort). The Sonnet 5.5 page says dropping extra review rounds cut session cost about a third at `max` effort; that is Anthropic's figure, below the 50% bar of our sources list. [Sonnet 5.5 page](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-sonnet-5-5)
- **Cache hygiene.** Keep history append-only; do not change `system` or `tools` mid-session; never put user text inside `tool_result`; avoid per-step countdown text after tool results.
- **Unchanged:** XML tags for mixed content, 3-5 examples, long documents at the top with the query last.
- **Not verified:** the Fable 5.1 and Opus 5 page bodies (section lists only). The older "overthinking" mitigations are written for Opus 4.6; do not assume they transfer.

## Plugins
- Layout: `.claude-plugin/plugin.json` (only `name` required), plus `skills/`, `agents/`, `commands/`, `hooks/hooks.json`, `.mcp.json`, `monitors/`, `output-styles/`. [Manifest reference](https://code.claude.com/docs/en/plugins/manifest-reference.md)
- **Descriptions cost tokens on every turn**, so keep skill and command descriptions short. [Overview](https://code.claude.com/docs/en/plugins/overview)
- Publish with `.claude-plugin/marketplace.json`; check with `claude plugin validate`. [Create a marketplace](https://code.claude.com/docs/en/plugins/create-marketplace.md)
- `claude plugin eval` scores a plugin against a no-plugin baseline *(index only)*. [What's new](https://code.claude.com/docs/en/whats-new/index.md)
- `ralph-loop` in the official marketplace is related prior art for loops; not inspected. [Marketplaces](https://code.claude.com/docs/en/plugins/anthropic-marketplaces.md)

## Mods
A mod is a plugin whose `hooks/hooks.json` points at a JS/TS module registering handlers (`on(event, hook)`). Mods can draw panes, restyle the spinner, add slash commands and rewrite prompts or tool calls. They need CLI 2.1.287+, run but do not draw in `-p` and cloud sessions, and are **not sandboxed**. [Mods overview](https://code.claude.com/docs/en/plugins/mods/overview.md)

## Hooks, `/goal`, costs
- About 30 hook events. `UserPromptSubmit` and `SessionStart` can add up to 10,000 characters of context but **cannot rewrite the prompt**; `Stop` with `decision: "block"` keeps Claude working. [Hooks](https://code.claude.com/docs/en/hooks.md) (read about a third)
- `/goal` sets a completion condition judged after each turn by a small model. It is the closest native version of our Loop. [Goal](https://code.claude.com/docs/en/goal.md)
- Cost advice: CLAUDE.md under ~200 lines, workflows in skills, noisy work in subagents, `/clear` between tasks, `/usage` to audit. [Costs](https://code.claude.com/docs/en/costs.md)

## Adopt / skip
| Item | Decision | Why |
|---|---|---|
| Effort guidance in CLAUDE.md | **Adopt** (one bullet) | Cheapest lever; nothing to maintain |
| Register `karpathy-loop` in `plugin.json` | **Adopt (bug fix)** | The skill was shipped but never listed, so plugin installs missed it |
| Hook that re-injects the Loop each prompt | **Skip** | CLAUDE.md already loads every prompt; injecting again doubles the tokens |
| `/goal` as native enforcement | **Evaluate next** | Zero code; compare against the custom Loop before building a `Stop` hook |
| `Stop` hook blocking "done" without evidence | **Defer** | Only if `/goal` falls short |
| `claude plugin eval` token comparison | **Defer** | Unverified; try once the plugin is installed |
| A mod | **Skip** | Unsandboxed, no clear gain over hooks |
