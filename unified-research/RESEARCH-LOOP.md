# The Auto-Research Loop

The Karpathy Loop applied to research. Input: a research question. Output: a
notebook entry with cited findings + distilled brain pages. The loop is
autonomous once criteria are set — "don't tell it what to do, give it success
criteria and watch it go."

## Phase 0 — Criteria (never skip)

Turn the question into 2-4 verifiable completion criteria before any retrieval:

- "Research competitor pricing" → "A page exists listing price, tier structure,
  and source URL for each of the N named competitors; every fact has a citation."
- "What's the state of X?" → "Doc answers sub-questions A, B, C; each answer
  cites ≥1 source published in the last 12 months."

If the question can't be made verifiable, stop and ask the user. Unbounded
research is unbounded token spend.

## Phase 1 — Brain first (cheapest)

```
gbrain search "<question terms>"        # hybrid search
gbrain graph-query <entity-slug>        # if the question centers on an entity
```

- Criteria fully met from brain content → **done**, report with page links. Zero
  external tokens spent.
- Partially met → record which criteria remain open. Only the gaps proceed.

## Phase 2 — Notebook second

Check open-notebook for an existing notebook on the topic. If sources are
already ingested, ask over them before fetching anything new. Add the open
criteria as notes to the notebook so the run is resumable.

## Phase 3 — RAG corpus third

For gaps answerable from bulk document sets, query the relevant Vertex AI
corpus via adk-rag-agent. Create a corpus only if the source set will be
queried again; one-shot questions don't justify corpus setup cost.

## Phase 4 — External research (most expensive, last)

For remaining gaps only:

1. Fan out **subagents** per gap (search + fetch happen in the subagent so raw
   page content never enters the main context — only structured findings return).
2. Every finding carries: claim, source URL, author, date. No citation → not a finding.
3. Ingest the sources worth keeping into the notebook (full text) so the next
   loop on this topic starts at Phase 2, not Phase 4.

## Phase 5 — Verify against criteria

Walk the criteria one by one. Each is met with evidence, or the loop returns to
the cheapest phase that could close it. Three consecutive passes with no
progress on a criterion → stop and report the blocker instead of burning tokens.

## Phase 6 — Distill and write back

- **Notebook**: full findings, all sources attached.
- **Brain**: durable conclusions only — entity pages, typed links, timeline
  entries, citations. Follow gbrain's filing rules.
- **Report**: criteria + evidence + links. Nothing else. Stop.

## Scheduling (optional, recurring topics)

For standing research topics, register a recurring job that re-runs the loop
with the same criteria plus "…published since <last run>". Use gbrain's
cron/minions layer with quiet hours and stagger; the brain-first phase makes
re-runs cheap because prior findings short-circuit at Phase 1.

## Token budget rules

- Phases are ordered by cost; never jump to Phase 4 while a cheaper phase is unexhausted.
- Subagents return structured findings, not page dumps.
- A finding ingested once (Phase 4.3) is never re-fetched — that's what the
  notebook and brain are for.
- Report format is capped: criteria, evidence, links.
