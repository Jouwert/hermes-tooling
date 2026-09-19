# Excerpt — accounting per completed task, not per day

**Component:** task ledger (cost and outcome per completed task)
**Status:** in daily use

## The problem with the obvious metric

Token totals per day are easy to collect and nearly useless for decisions. They cannot answer
"is this workflow worth running", "did this task finish", or "how much human guidance did it
need". Those are the questions that matter when agentic work has to justify itself.

## The unit of work

One record per **completed task**, written by a deterministic recorder rather than
reconstructed from logs. Fields carried per record:

- project and workflow (normalised, so work can be compared across months);
- objective, and the success criterion in force when it started;
- status — success / partial / aborted / failed;
- token and cost deltas, measured as incremental values for the task, not session totals;
- human-message count, so guidance effort is visible next to cost;
- an explicit `unavailable` value wherever a number was not recorded.

Event history (start, verification, completion) is kept **alongside** the row and is never used
to fabricate a missing field. A task whose cost was not captured is recorded as unavailable;
it is not filled in from the session total, because that would silently redistribute cost
between tasks and corrupt exactly the comparison the ledger exists to support.

## Scale at time of writing

- **392** completed task records, with **375** linked lifecycle events and **43** parent-task
  relationships.

These are counts of records, not a quality claim, and not a productivity metric.

## Deliberate design decisions

| Decision | Reason |
|---|---|
| Deterministic row, event history as support | A single stable row keeps reporting cheap and comparable |
| Incremental deltas only | Session totals cannot be attributed to a task without guessing |
| Explicit `unavailable` | A gap is honest; an estimate corrupts the dataset |
| Parent event reused across sessions | Reopening a task must not fork its history |
| No-agent reporting | Reporting must not itself consume the budget it measures |

## Why this is the quiet backbone of the repository

Every other component here is judged with numbers taken from this ledger — including the ones
that were retired. Measurement infrastructure that refuses to estimate is what makes the
negative results in this repository credible rather than rhetorical.
