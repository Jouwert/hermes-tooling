# Architecture

A description of the platform's shape and the reasoning behind each layer. Deliberately
generic in the places where the specifics are private.

## The whole system in one paragraph

A single gateway process receives work from messaging surfaces, resolves it to a session, and
runs it through an agent loop with tool access. Around that loop sit four concerns that are
each their own component: **routing** (which model does this step), **delegation** (does this
task need a second agent), **memory** (continuity, projects, procedures), and **accounting**
(what did this task cost and did it finish). Experiments attach to the loop as bounded
observers rather than as changes to it.

```
                         ┌──────────────────────────────┐
   messaging surfaces ──▶│  gateway: ingress + session  │
                         │  resolution + delivery       │
                         └──────────────┬───────────────┘
                                        │
              ┌─────────────────────────▼─────────────────────────┐
              │                   agent loop                      │
              │        (tools, skills, context assembly)          │
              └───┬──────────────┬───────────────┬──────────────┬─┘
                  │              │               │              │
          ┌───────▼─────┐ ┌──────▼──────┐ ┌──────▼──────┐ ┌─────▼──────┐
          │  routing    │ │ delegation  │ │   memory    │ │ accounting │
          │ per-role    │ │ profiles,   │ │ continuity  │ │ task       │
          │ model choice│ │ review lanes│ │ + skills    │ │ ledger     │
          └─────────────┘ └─────────────┘ └─────────────┘ └────────────┘
                  ▲
          ┌───────┴───────────────────────────────────────┐
          │ bounded observers (shadow experiments, hooks) │
          │ read-only; no production record is rewritten  │
          └───────────────────────────────────────────────┘
```

## Layers, and why each exists

### 1. Gateway and session resolution

Messaging surfaces are the interface; the gateway resolves an inbound message to an exact
session before any model work happens. That ordering matters: it is what later makes exact
task attribution possible, because the session is known at ingress rather than inferred
afterwards.

### 2. The agent loop

The loop is deliberately conventional: assemble context, plan, call tools, verify, respond.
Skills are loaded on demand rather than injected wholesale, which keeps the working context
small enough to stay accurate. The interesting design work is not in the loop; it is in what
surrounds it.

### 3. Routing — which model does this step

Models are chosen per *role*, not per task size:

- one strong model for judgement, synthesis and consequential decisions;
- cheaper models for mechanical, well-specified execution;
- mechanical transforms scripted entirely, bypassing a model.

This came directly from a constraint: an early single-model setup burned roughly 29% of its
five-hour primary quota in about thirty minutes of heavy use. Routing is therefore a
correctness-of-fit decision first and a cost decision second.

### 4. Delegation — when a second agent is warranted

The default is one capable agent. A second agent is formed only when work genuinely crosses
specialist roles and the split survives the cost of briefing, reviewing and repairing the
handoff. Work is routed through an explicit board with review lanes, so a specialist's output
is verifiable rather than trusted on self-report.

Honest scale: the board exists and works, but its usage is light. Most work in practice is
one agent plus verification.

### 5. Memory — continuity and procedures

Two distinct stores, because they answer different questions:

- **Continuity** (a written session state) answers *what was happening and what is open*.
- **Procedures** (a curated skill library) answer *how this kind of work is done here*,
  including the pitfalls that were expensive to learn.

The separation is deliberate. Continuity is short-lived and factual; procedures are durable
and conditional. Mixing them produced stale instructions and was corrected.

### 6. Accounting — cost and outcome per completed task

A deterministic recorder writes one stable row per completed task: project, workflow,
objective, status, token and cost deltas, and human-message count. Event history is kept
alongside the row but never used to fabricate a missing number — absence is recorded as
"unavailable".

### 7. Bounded observers

Experiments attach to the loop through content-free lifecycle signals: one emitted before
model work, one after the transcript and usage counters are durable. The observer records
routing keys as hashes, never message text, and cannot mutate the ledger. Promotion to a
conclusion requires pre-declared gates — including a minimum number of naturally completed
tasks and a zero-guessed-metrics rule — so a small or dirty sample cannot be dressed up as a
result.

## Cross-cutting decisions

| Decision | Why |
|---|---|
| Skills on demand, not wholesale | Context accuracy degrades before context capacity does |
| Adopt-and-measure over build | Cheaper, and the measurement is the more interesting finding |
| Structural-only analysis of source by default | Keeping code on the machine is worth more than richer summaries |
| Report "unavailable" rather than estimate | A guessed metric is worse than a gap in a portfolio piece |
| Publish negatives | An unproven hypothesis, honestly reported, is the strongest credibility signal here |

## Complexity that was removed

- A wait window between a task finishing and being counted, which could cancel valid
  accounting when the next message arrived quickly.
- A single global model setting, replaced by per-role routing.
- Passive knowledge pages that duplicated what an existing validator already enforced — the
  correct outcome there was zero new pages.

Complexity that gets removed is as much a design decision as complexity that gets added.
