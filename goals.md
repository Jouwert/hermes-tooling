# Goals — and non-goals

## Why this repository exists

To show, through one coherent case study, how I extend the agent platform I depend on rather
than only operating it: what I chose to build, what I chose to adopt, how I measured the
result, and where the honest answer is "this did not prove what I hoped".

The signal this is meant to carry is **deployment judgement under real constraints** — single
operator, quota-limited models, strict data boundaries — not a feature list.

## Goals

1. **Make agent work accountable.** Every completed task should carry a cost and an outcome,
   measured deterministically, without reconstructing history from raw logs.
2. **Make agent work survivable.** A lapsed session should resume from a written state instead
   of asking a human to reconstruct context.
3. **Make agent work smaller where possible.** Unbounded cleverness is a cost. Several
   components here exist to decide *not* to do something: not to form a team, not to add a
   knowledge page, not to route to the expensive model.
4. **Keep experiment data separate from production.** Any experimental tooling must run
   without receiving client, organisational or personal data, and must be switchable off.
5. **Publish the negative results.** A component that did not prove its hypothesis is more
   informative than one that merely shipped.

## Non-goals

- **Not a product.** There is no multi-tenant story, no SLA, no support surface.
- **Not a benchmark of model quality.** Where models are compared, the comparison is about
  role suitability under quota, not about which model is "best".
- **Not an argument for multi-agent by default.** The opposite: the default is one capable
  agent, and multi-agent structure has to earn its coordination cost.
- **Not a framework release.** Nothing here is intended to be installed by someone else.
- **Not a claim of authorship over third-party tools.** Adopted tools are named as adopted.

## Success criteria — how to tell whether this repository worked

A reader should be able to answer, from the documents alone:

1. What was the operational problem, in one sentence, for each component?
2. What was the constraint that made the design interesting?
3. What was measured, and what was merely asserted?
4. What is deliberately excluded, and why?
5. Which parts did **not** work out, and how was that handled?

If a reader can only answer "he wired up some agent tools", the writeup has failed.
