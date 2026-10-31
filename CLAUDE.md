# CLAUDE.md

## The Loop (mandatory — run this on every prompt)

From Andrej Karpathy: "LLMs are exceptionally good at looping until they meet
specific goals... Don't tell it what to do, give it success criteria and watch it go."

Before doing anything else, on every prompt:

1. **Goal** — restate the request as 1-3 *verifiable* success criteria
   (a test that passes, a command whose output matches, a file that exists with X).
2. **Plan** — the smallest set of steps that could satisfy the criteria;
   each step names its verify check.
3. **Execute** — do one step, then verify it. Never batch unverified steps.
4. **Loop** — criteria unmet → adjust and repeat. Criteria met → stop.
   No extra polish passes, no unrequested work.
5. **Report** — state the criteria and the evidence they're met. Nothing else.

If the success criteria can't be made verifiable, say so and ask — don't guess.

## Token efficiency (every prompt)

- Read only what the current step needs. Read sections, not whole files, when large.
- Never re-read files already in context; never re-run commands whose output hasn't changed.
- One precise search beats browsing. Grep before opening files.
- Keep responses to the report format above — don't restate file contents back.
- Delegate large scans/research to subagents so raw output stays out of the main context.
- Keep CLAUDE.md files lean: they are loaded on *every* prompt. Detail belongs in
  docs/ files read on demand.

## Principles (condensed)

1. **Think before coding** — surface assumptions, ambiguities, and simpler
   alternatives before implementing. If unclear, stop and ask.
2. **Simplicity first** — minimum code that meets the criteria. No speculative
   features, abstractions, or error handling. If 200 lines could be 50, rewrite.
3. **Surgical changes** — touch only what the request requires; match existing
   style; clean up only orphans your own change created.

Full versions with examples: [skills/karpathy-guidelines/SKILL.md](skills/karpathy-guidelines/SKILL.md).
Goal-driven execution is The Loop above.

## Unified Brain · Notebook · Auto-Research

This repo is the behavioral layer of a four-part system. Architecture, the
auto-research loop, and the token-efficiency evidence base live in
[unified-research/](unified-research/README.md) — read on demand, not per prompt.

**Tradeoff:** these rules bias toward caution over speed. For trivial tasks, use judgment.
