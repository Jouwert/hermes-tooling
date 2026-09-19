# How I decide what earns adoption

A short, reusable method. It is the part of this repository most likely to be useful to someone
else, because it applies to any emerging AI tool, not only the three assessed here.

## Why a method at all

The failure mode with new AI tooling is not picking something bad. It is adopting something
plausible, never measuring it, and then being unable to say whether it helped — while the
dependency quietly grows. A written method makes that harder.

## Step 1 — Name the cost being removed

Write one sentence: *this tool removes the cost of ___*. If the sentence needs "and", it is two
evaluations, not one. If the cost cannot be named, stop here.

Worked examples:
- knowledge layer → *re-deriving context that a previous session already established*
- decision model → *asking a generative model to write a paragraph in order to extract one word*
- code map → *repeatedly searching for things in a codebase an agent already works in*

## Step 2 — Separate the four kinds of statement

Almost all tool confusion comes from mixing these:

| Kind | Example | How it is treated |
|---|---|---|
| **Vendor claim** | "cuts tool calls by 46%" | A hypothesis. Never repeated as a result. |
| **Reproducible measurement** | my two-arm canary, method stated | Evidence. Must be re-runnable by a reader. |
| **Private record** | a count from a working ledger | Reported with basis and date, labelled as a record. |
| **Design reasoning** | "this category will matter" | Clearly argued opinion, not evidence. |

A tool writeup that does not distinguish these is marketing with extra steps.

## Step 3 — Measure on your own workload, on one basis

One real question, two arms, one machine, ground truth derived from the source rather than from
the tool. Compare tool calls, tokens, wall time and *correctness* — the four numbers that
actually decide adoption. Then, and only then, look at the vendor's number and see whether you
recognise it.

The most informative outcome is a measurement that contradicts the marketing while still
supporting adoption for a narrower reason.

## Step 4 — Price the adoption, not just the licence

Adoption cost is nearly always the thing that decides it:

- **Setup and refresh** — who keeps it current, and what happens when it goes stale?
- **Trust** — does it answer confidently when wrong? Can you tell?
- **Surface** — is the useful part actually reachable in your stack, or is it in a different
  interface than the one you use?
- **Dependency** — if this disappears next quarter, what breaks?
- **Attention** — does it remove work, or move it to a place you will still be looking at?

## Step 5 — Write the boundary before the integration

State what the tool must never receive, before it receives anything. No client, personal or
municipal data without terms, retention and data-location checked. No broad production logging
during evaluation. No generalised abstraction layer built in anticipation of a tool that has not
earned it — one instrumented call-site first.

## Step 6 — Define the exit

Write down what would make you stop. A tool you cannot switch off is a commitment, not a
selection.

## Step 7 — Decide in one of four outcomes

1. **Adopt** — measured benefit, acceptable cost, boundary written.
2. **Adopt narrowly** — real value in one capability, not the default path. *(Graft.)*
3. **Park** — right category, wrong moment: access, governance or maturity not yet there, with
   restart conditions written down. *(Jev.)*
4. **Publish as unproven** — worked partly, central claim untested. Keeping it running would look
   like progress. *(The knowledge layer.)*

Two of these four are negative outcomes, and both are useful results. A method whose only
possible output is "adopt" is not a method.

## The one-line version

**Name the cost, separate claims from measurements, measure on your own workload, price the
adoption, write the boundary, define the exit — and be willing to publish the negative.**