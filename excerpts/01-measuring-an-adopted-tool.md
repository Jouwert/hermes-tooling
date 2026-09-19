# Excerpt — measuring an adopted tool instead of assuming it

**Component:** codebase context mapping (adopted third-party CLI)
**Date of measurement:** 2026-09-19

## The question

Does mapping a repository into a dependency graph actually beat plain search-and-read cycles,
or does it just feel more rigorous? The tool was already installed and in use, so the honest
thing was to measure it on a real question rather than keep quoting its own marketing.

## Method

One real three-file question, answered twice, read-only, both arms scored against a ground
truth derived from the source itself. Same machine, same repository, same question.

## Result

| | mapping-first | search/read-first |
|---|---|---|
| Tool calls | **3** | 10 |
| Tokens consumed | **7,479** | 11,015 |
| Wall time | 3.26 s | **0.049 s** |
| Correct files found | 3 / 3 | 3 / 3 |

## What the numbers actually mean

- Roughly **3× fewer tool calls** and **~1.5× fewer tokens**.
- But roughly **66× slower**, because the CLI pays a runtime start-up cost on every
  invocation.
- The genuine win is neither speed nor a dramatic token saving: it is the **exact call-graph
  edges** (who calls this, what is the blast radius) that search cannot produce at any cost,
  plus fewer context-heavy file reads.

## Honesty rules that came out of this

1. The tool prints its own estimated "tokens saved" figure on every call. On this run it
   claimed ~39,624 tokens saved while consuming 7,479. **It is an uncalibrated self-reported
   claim and is never quoted as a measurement.**
2. A stale graph is worse than no graph — it answers confidently and wrongly. Checking that
   the graph is in sync is a required step, and drift is silent unless explicitly checked.
3. The tool is an **option, not a default**. It earns its setup cost only on a codebase under
   active work; for a one-off script the plain approach is correct and adoption is waste.

## Why this is the most useful excerpt here

It shows the difference between *using* a tool and *evaluating* one: the measurement
contradicted the vendor's headline number, kept the tool for one narrow capability, and
removed it from the default path. Adoption judgement, not tool enthusiasm.
