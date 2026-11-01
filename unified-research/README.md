# Unified Brain · Notebook · Auto-Research

One system, four repos. This doc is the map; read it on demand, not per prompt.

| Layer | Repo | Role |
|-------|------|------|
| **Brain** | [gbrain](https://github.com/dylan-productivity/gbrain) | Persistent knowledge: pages, entities, typed links, hybrid search, knowledge graph. The long-term memory every other layer reads first and writes back to. |
| **Notebook** | [open-notebook](https://github.com/dylan-productivity/open-notebook) | Working surface for active research: notebooks, source ingestion (URLs, PDFs, audio), chat over sources, podcast generation. Self-hosted Notebook LM alternative. |
| **Retrieval** | [adk-rag-agent](https://github.com/dylan-productivity/adk-rag-agent) | Vertex AI RAG corpora via Google ADK: bulk document corpora that are too large or too raw for the brain. Query, create, and manage corpora with natural language. |
| **Behavior** | [andrej-karpathy-skills](https://github.com/dylan-productivity/andrej-karpathy-skills) (this repo) | The rules of engagement: the Karpathy Loop enforced on every prompt, token-efficiency rules, and the behavioral principles. |

[gstack](https://github.com/dylan-productivity/gstack) sits alongside as the dev
workflow layer (ship/review/investigate skills) when building on the system itself.

## How the layers compose

```
                    ┌────────────────────────────┐
   every prompt ──► │  Behavior: Karpathy Loop   │  success criteria, verify, stop
                    └────────────┬───────────────┘
                                 │ governs
        ┌────────────────────────┼────────────────────────┐
        ▼                        ▼                        ▼
┌───────────────┐       ┌────────────────┐       ┌────────────────┐
│   Brain       │ ◄───► │   Notebook     │ ◄──── │   Retrieval    │
│   (gbrain)    │       │ (open-notebook)│       │ (adk-rag-agent)│
│ durable facts │       │ active projects│       │ bulk corpora   │
└───────────────┘       └────────┬───────┘       └────────────────┘
        ▲                        │
        └────── distilled ───────┘
           findings flow down into the brain when a research loop closes
```

**Read order on any research question (cheapest first):**

1. **Brain first** — `gbrain search` / graph query. If the brain already knows,
   the loop closes in one step and zero external tokens are spent. This is the
   single biggest token saver in the system: never re-research what you already know.
2. **Notebook second** — is there an active notebook on this topic with sources
   already ingested? Chat over existing sources before fetching new ones.
3. **RAG corpus third** — for questions over large document sets (data rooms,
   documentation dumps) that live in Vertex AI corpora.
4. **External research last** — web search / subagents, only for the gaps the
   first three layers couldn't fill.

**Write order when a loop closes:** findings land in the notebook (full detail,
sources attached) → durable conclusions distill into brain pages with entity
links and citations → raw bulk material goes to a RAG corpus if it's worth
keeping but too big for the brain.

## The auto-research loop

The loop that runs research unattended is specified in
[RESEARCH-LOOP.md](RESEARCH-LOOP.md). It is the Karpathy Loop applied to
research: a question becomes verifiable completion criteria, the four layers
are consulted cheapest-first, and the loop repeats until the criteria are met —
then stops.

## Token-efficiency evidence base

The design above is not vibes. [sources/token-efficiency.md](sources/token-efficiency.md)
collects 20+ published sources — vendors, practitioners, and papers — claiming
50%+ (often 75-90%+) reductions in token spend from exactly the techniques this
system uses: prompt/context caching, brain-first retrieval instead of context
stuffing, compaction, lean always-loaded instructions, and routing work to
cheaper paths.

Platform review (prompting, plugins, mods, hooks):
[reviews/claude-platform-2026-11.md](reviews/claude-platform-2026-11.md).

## Operating rules (summary)

- The Karpathy Loop ([CLAUDE.md](../CLAUDE.md)) applies to every prompt in every
  repo of this system.
- CLAUDE.md files stay lean — they're loaded on every prompt. Detail lives in
  docs like this one, read on demand. (This principle was applied to gbrain and
  gstack: their per-file inventories moved to `docs/CLAUDE-REFERENCE.md` in each repo.)
- Brain-first lookup before any external research. Re-research is the most
  expensive failure mode.
- Research runs report criteria + evidence, and stop when criteria are met.
