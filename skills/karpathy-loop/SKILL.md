---
name: karpathy-loop
description: The Karpathy looping method - mandatory goal-driven execution loop applied to every prompt. Restate the request as verifiable success criteria, plan minimal steps with verify checks, execute one step at a time, loop until criteria are met, then stop. Use on every task to stay token-efficient and avoid unrequested work.
license: MIT
---

# The Karpathy Loop

Derived from [Andrej Karpathy's observation](https://x.com/karpathy/status/2015883857489522876):

> "LLMs are exceptionally good at looping until they meet specific goals...
> Don't tell it what to do, give it success criteria and watch it go."

The loop turns every imperative instruction into a declarative goal with a
verification loop. It is mandatory on every prompt — not just coding tasks.

## The Protocol

### 1. Goal — define verifiable success criteria

Restate the request as 1-3 criteria that a machine (or a re-read) can check:

| Instead of... | Transform to... |
|---------------|-----------------|
| "Add validation" | "Tests for invalid inputs exist and pass" |
| "Fix the bug" | "A test reproducing the bug exists and passes" |
| "Research X" | "A doc exists answering questions A, B, C with cited sources" |
| "Clean up the docs" | "File is under N lines; no content lost (moved to reference)" |

If the criteria can't be made verifiable, stop and ask. Weak criteria
("make it work") force constant clarification; strong criteria let the loop
run unattended.

### 2. Plan — minimal steps, each with a verify check

```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

The smallest plan that *could* satisfy the criteria. Nothing speculative.

### 3. Execute — one step, then verify

Run the step, run its check. Never batch multiple unverified steps — a wrong
assumption in step 1 silently poisons steps 2-5 and wastes every token spent
on them.

### 4. Loop — until criteria met, then STOP

- Check fails → diagnose, adjust, re-run. That's the loop working, not a failure.
- Criteria met → **stop**. No polish passes, no drive-by improvements, no
  "while I'm here" refactors. Unrequested work is the single biggest source
  of wasted tokens and diff bloat.

### 5. Report — criteria + evidence, nothing else

State each criterion and the evidence it's met (test output, file path,
command result). Don't restate file contents, don't summarize the summary.

## Why this stays token-efficient

- **Verified steps don't get redone.** Most token waste is rework from
  unverified assumptions compounding.
- **Stopping criteria kill gold-plating.** The loop has a defined exit; open-ended
  instructions don't.
- **The report format caps output.** Evidence, not narration.

## Tradeoff

Biases caution over speed. For trivial tasks (typo fix, one-liner), collapse
the loop to: do it, verify it, report one line.
