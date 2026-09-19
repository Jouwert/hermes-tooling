# Excerpt — promotion gates for an experiment that measures itself

**Component:** bounded shadow experiment attached to the gateway
**Status:** running; design published, results withheld until the observation window closes

## The problem

Attribution of cost and outcome to a *task* is easy to claim and hard to prove, because the
boundary between one task and the next is exactly what is uncertain. A short pilot had exact
task-local attribution for **1 of 35** measurements (2.86%) — i.e. almost nothing was
attributable.

## The change under test

The gateway emits two **content-free** lifecycle signals:

1. `turn:admitted` — after exact session resolution and *before* any turn preparation or
   model-assisted work;
2. `turn:persisted` — after the final transcript and usage counters are durable and *before*
   platform delivery.

The observer records usage anchors in an isolated database. The second signal also triggers the
existing deterministic finalizer, which removes a previously existing wait window in which a
fast next message could cancel otherwise-valid accounting.

## Safety boundary

- No message text, usernames, chat IDs or raw platform message IDs enter the experiment database.
- Session routing keys and inbound message IDs are hashed.
- Production task records are **read-only inputs**; no historical row is rewritten.
- Missing anchors or regressing counters remain `unavailable` — **no model may estimate them**.

## Promotion gates — all must pass before any claim is made

- the full observation window is reached;
- at least five naturally completed task iterations;
- at least 80% of completed iterations have exact ingress-to-persisted route deltas;
- at least 95% of genuine human turns have both boundaries present;
- database integrity check passes with zero constraint violations;
- **zero guessed metrics**;
- capture disables itself automatically at the end of the window.

## Why gates instead of a conclusion

The temptation with telemetry is to read an improving line and announce a result. Pre-declared
gates make that structurally harder: the experiment cannot be declared successful early, cannot
report a metric it did not record, and cannot quietly keep running past its window.

The companion rule — results are published from generated reports, not from the underlying
operational database — keeps the evidence separate from the environment.
