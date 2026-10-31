# Token-Efficiency Evidence Base

Published sources claiming **50%+ reductions in token spend / LLM cost**, compiled
2026-10-31 via web research with URL verification. Grouped by source type. Read the
honesty notes: these are the authors' *published claims*, not independent replications —
some are measured evals, some are pricing discounts on cached/batched tokens (which cut
spend ≥50% only on the portion they apply to), and a couple are headline claims with
thin public methodology (flagged). Sources marked *(snippet-verified)* were confirmed via
search-result extracts because this environment's network allowlist blocked the host;
all others were fetched directly.

## Platform / vendor sources (≥50% claims)

| # | Source | Claim | Link |
|---|--------|-------|------|
| 1 | **Anthropic — Prompt caching with Claude** (Aug 2024) | "Reducing costs by up to **90%** and latency by up to 85% for long prompts"; benchmark table: chat-with-a-book −90% cost, many-shot −86%, multi-turn −53% | [claude.com/blog/prompt-caching](https://claude.com/blog/prompt-caching) |
| 2 | **Anthropic — Message Batches API** (Oct 2024) | Batches "cost **50% less** than standard API calls" | [claude.com/blog/message-batches-api](https://claude.com/blog/message-batches-api) |
| 3 | **Anthropic — Managing context (context editing + memory tool)** (Sep 2025) | 100-turn agentic eval: context editing completed workflows "while reducing token consumption by **84%**"; +39% performance with memory tool | [claude.com/blog/context-management](https://claude.com/blog/context-management) |
| 4 | **Anthropic — Code execution with MCP** (Nov 2025) | Worked example: loading tool definitions on demand and filtering data in the execution environment cut a workflow "from 150,000 tokens to 2,000 tokens — a time and cost saving of **98.7%**" (single example, not a benchmark) | [anthropic.com/engineering/code-execution-with-mcp](https://www.anthropic.com/engineering/code-execution-with-mcp) |
| 5 | **Anthropic — Advanced tool use (Tool Search Tool)** (Nov 2025) | On-demand tool discovery: "an **85% reduction in token usage** while maintaining access to your full tool library" (~8.7K vs ~77K context tokens) | [anthropic.com/engineering/advanced-tool-use](https://www.anthropic.com/engineering/advanced-tool-use) |
| 6 | **OpenAI — Prompt Caching in the API** (Oct 2024) *(snippet-verified)* | "**50% discount** and faster prompt processing times" on reused input tokens (GPT-4o $1.25 cached vs $2.50 uncached/MTok) | [openai.com/index/api-prompt-caching](https://openai.com/index/api-prompt-caching) |
| 7 | **OpenAI — Batch API** (Apr 2024) *(snippet-verified)* | Batches "cost **50% less** than synchronous calls" | [developers.openai.com/api/docs/guides/batch](https://developers.openai.com/api/docs/guides/batch) |
| 8 | **DeepSeek — Context Caching on Disk** (Aug 2024) *(snippet-verified; corroborated by Simon Willison)* | Cache hits priced $0.014/MTok; "save **up to 90%** on costs"; on by default | [api-docs.deepseek.com/news/news0802](https://api-docs.deepseek.com/news/news0802) |
| 9 | **Google — Gemini 2.5 implicit caching** (May 2025) *(snippet-verified)* | "The same **75% token discount**" passed back automatically on cached prefixes; current Vertex AI docs state **90%** for Gemini 2.5 implicit caching | [developers.googleblog.com — implicit caching](https://developers.googleblog.com/en/gemini-2-5-models-now-support-implicit-caching/) |

## Practitioner / thought-leader sources (≥50% claims)

| # | Source | Claim | Link |
|---|--------|-------|------|
| 10 | **Yichao "Peak" Ji, Manus — Context Engineering for AI Agents** (Jul 2025) *(snippet-verified)* | KV-cache hit rate is "the single most important metric for a production-stage AI agent"; cached input is **10x cheaper** than uncached ($0.30 vs $3.00/MTok on Claude Sonnet) at ~100:1 input:output ratios | [manus.im/blog](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus) |
| 11 | **Simon Willison — Gemini implicit caching** (May 2025) *(snippet-verified)* | Documents the automatic "**75% token discount**" on shared prefixes; earlier post showed explicit Gemini caching halving per-prompt cost | [simonwillison.net/2025/May/9](https://simonwillison.net/2025/May/9/gemini-implicit-caching/) |
| 12 | **LangChain — how_to_fix_your_context** (2025) | Context pruning: "reduced token usage from 25k to 11k tokens" (~**56%**) vs basic RAG; summarization guidelines target **50-70%** reduction | [github.com/langchain-ai/how_to_fix_your_context](https://github.com/langchain-ai/how_to_fix_your_context) |
| 13 | **Thomson Reuters Labs — Prompt Caching: The Secret to 60% Cost Reduction** (~2024-25) *(title-level verification only — weakest source here)* | "**60% cost reduction** in LLM applications" via prompt caching | [medium.com/tr-labs-ml-engineering-blog](https://medium.com/tr-labs-ml-engineering-blog/prompt-caching-the-secret-to-60-cost-reduction-in-llm-applications-6c792a0ac29b) |

## Research / open-source tooling (≥50% claims)

| # | Source | Claim | Link |
|---|--------|-------|------|
| 14 | **LLMLingua — Microsoft Research** (EMNLP 2023) | "Up to **20x compression** with minimal performance loss" (≈95% prompt-token reduction at the upper bound; 3-9x typical) | [arxiv.org/abs/2310.05736](https://arxiv.org/abs/2310.05736) · [github.com/microsoft/LLMLingua](https://github.com/microsoft/LLMLingua) |
| 15 | **LongLLMLingua — Microsoft Research** (ICLR 2024) | "+21.4% RAG performance using only **1/4 of the tokens**"; "**94.0% cost reduction** in the LooGLE benchmark" | [arxiv.org/abs/2310.06839](https://arxiv.org/abs/2310.06839) |
| 16 | **LLMLingua-2 — Microsoft** (ACL Findings 2024) | Task-agnostic compression at **2-5x** (50-80% token reduction), 3-6x faster compressor, 1.6-2.9x lower end-to-end latency | [arxiv.org/abs/2403.12968](https://arxiv.org/abs/2403.12968) |
| 17 | **FrugalGPT — Chen, Zaharia, Zou (Stanford)** (May 2023) | LLM cascade "can match the performance of the best individual LLM (e.g. GPT-4) with **up to 98% cost reduction**" (task-specific upper bound) | [arxiv.org/abs/2305.05176](https://arxiv.org/abs/2305.05176) |
| 18 | **RouteLLM — LMSYS / UC Berkeley** (Jul 2024) | Routers "reduce costs by **up to 85%**" while maintaining 95% GPT-4 performance on MT Bench (45% MMLU / 35% GSM8K — benchmark-dependent) | [arxiv.org/abs/2406.18665](https://arxiv.org/abs/2406.18665) · [github.com/lm-sys/RouteLLM](https://github.com/lm-sys/RouteLLM) |
| 19 | **ICAE (In-context Autoencoder) — Microsoft** (ICLR 2024) | "**4x context compression** based on Llama" (75% context-token reduction) with <1% added parameters; needs model access, not black-box APIs | [arxiv.org/abs/2307.06945](https://arxiv.org/abs/2307.06945) |
| 20 | **GPTCache — Zilliz** (2023) | Semantic caching: "Slash Your LLM API Costs by **10x**" (unbenchmarked vendor headline; savings depend on cache hit rate) | [github.com/zilliztech/GPTCache](https://github.com/zilliztech/GPTCache) |
| 21 | **TOON (Token-Oriented Object Notation) — Johann Schopplich** (Oct 2025) | vs JSON: **58.7% fewer tokens** on flat/uniform tabular data (up to 66.5% per dataset); ~33% on mixed structures — ≥50% only for tabular payloads | [github.com/toon-format/toon](https://github.com/toon-format/toon) |

**Corroborating survey:** *Prompt Compression for LLMs: A Survey* (Cambridge, Oct 2024) independently
reports the up-to-20x LLMLingua figures — [arxiv.org/abs/2410.12388](https://arxiv.org/abs/2410.12388).

## Foundational sources (no headline %, but shaped the techniques above)

- **Andrej Karpathy** — endorsed "context engineering" over prompt engineering: "the delicate art and
  science of filling the context window with just the right information for the next step"
  ([x.com/karpathy, Jun 2025](https://x.com/karpathy/status/1937902205765607626)); and the looping
  method this project enforces: "LLMs are exceptionally good at looping until they meet specific
  goals... give it success criteria and watch it go."
- **Drew Breunig** — the context-failure taxonomy (poisoning, distraction, confusion, clash) and six
  fixes that LangChain's measured numbers above implement
  ([dbreunig.com, Jun 2025](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html)).
- **Chroma — Context Rot** (Jul 2025) — across 18 frontier models, performance degrades as input grows;
  the empirical case that shorter engineered context is better *and* cheaper
  ([research.trychroma.com/context-rot](https://research.trychroma.com/context-rot)).
- **Walden Yan, Cognition — Don't Build Multi-Agents** (Jun 2025) — context engineering as the core
  agent-design discipline ([cognition.ai/blog/dont-build-multi-agents](https://cognition.ai/blog/dont-build-multi-agents)).
- **Anthropic — Effective context engineering for AI agents** (Sep 2025) — compaction, structured
  note-taking, subagents returning 1-2K-token summaries
  ([anthropic.com/engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)).
- **Anthropic — Claude Code best practices** — keep CLAUDE.md short ("every line takes a little of
  Claude's attention on every task"), `/clear` between tasks, focused `/compact`
  ([code.claude.com/docs/en/best-practices](https://code.claude.com/docs/en/best-practices)).

## How this project applies them

| Technique (sources) | Where it lives here |
|---|---|
| Stable prefix / cache-friendly, lean always-loaded instructions (1, 6, 8, 9, 10, 11) | Slim CLAUDE.md in all five repos; detail moved to on-demand `docs/CLAUDE-REFERENCE.md` |
| Retrieve instead of stuffing context; prune to what's needed (12, 14-16, foundational) | Brain-first → notebook → RAG corpus → web, cheapest-first ([RESEARCH-LOOP.md](../RESEARCH-LOOP.md)) |
| Subagents return distilled findings, not raw dumps (4, 5, Anthropic context engineering) | Research loop Phase 4; token-efficiency rules in CLAUDE.md |
| Route work to the cheapest path that can do it (17, 18) | Phase ordering + "never jump to Phase 4 while a cheaper phase is unexhausted" |
| Stop when success criteria are met — no unbounded loops (Karpathy) | The Loop, enforced every prompt |
