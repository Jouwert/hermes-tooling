# Evidence index — where every number in this repository comes from

Purpose: make each quantitative claim traceable, and make clear which claims are *reproducible
by a reader* and which are *verified records held privately*.

Two honest categories:

- **Reproducible** — the method is described in this repository and the result can be re-run.
- **Privately verified** — the underlying record is private (it includes operational data), so
  the figure is reported with its verification basis and date, and the reader is asked to treat
  it as a record rather than an independent measurement.

No figure in this repository is estimated, inferred, or extrapolated. Where a value was not
recorded, the text says "unavailable" — that is a rule in the accompanying system, not a
convention of the writing.

## Index

| Claim | Value | Category | Verification basis | Date verified |
|---|---|---|---|---|
| Adopted-tool comparison: tool calls | 3 vs 10 | Reproducible | Two-arm canary described in `excerpts/01-measuring-an-adopted-tool.md`; method stated, ground truth derived from source | 2026-09-19 |
| Adopted-tool comparison: tokens | 7,479 vs 11,015 | Reproducible | Same canary, both arms measured on one consistent basis (not via the vendor's own estimate) | 2026-09-19 |
| Adopted-tool comparison: wall time | 3.26 s vs 0.049 s | Reproducible | Same canary | 2026-09-19 |
| Vendor's self-reported saving | ~39,624 tokens | Reported as a claim | Quoted **only** to show it disagrees with the measurement; never presented as a result | 2026-09-19 |
| Derived ratios from the canary | ~3× fewer tool calls, ~1.5× fewer tokens, ~66× slower | Reproducible | Arithmetic on the canary rows above, same run | 2026-09-19 |
| Promotion-gate thresholds | 80% / 95% | Not a measurement | Pre-declared pass conditions in the private experiment plan; they are criteria the run must meet, not results | 2026-09-19 |
| Knowledge-layer closure state | 6 corpora / 58 units / 81 refs, 80 matching | Privately verified | Frozen-hash verifier run on the private experiment record; result `PASS_WITH_DECLARED_DRIFT` | 2026-09-19 |
| Task ledger: completed records | 392 | Privately verified | Direct count against the private ledger database | 2026-09-19 |
| Task ledger: linked lifecycle events | 375 | Privately verified | Direct count against the private ledger database | 2026-09-19 |
| Task ledger: parent relationships | 43 | Privately verified | Direct count against the private ledger database | 2026-09-19 |
| Attribution baseline before the change | 1 of 35 (2.86%) | Privately verified | Prior short pilot, recorded in the private experiment plan | 2026-09-19 |
| Single-model quota burn | ~29% of a 5-hour primary window in ~30 min | Privately verified | Quota monitor reading recorded in the private project record at the time | 2026-09-19 |

## Notes for a reviewer

1. **The canary is the strongest artifact here** because it is reproducible and because its
   result contradicted the tool vendor's own headline figure. Most of the rest of this
   repository is design reasoning with privately verified counts attached.
2. **Counts are counts.** 392 completed records, 375 events and 43 parent links measure
   activity in a working environment. They are not a quality or productivity claim, and are
   not used as one anywhere in this repository.
3. **The parked and unproven components carry no numbers at all** — deliberately. A negative
   result reported without invented precision is more defensible than one dressed up with
   figures it never had.
4. **No figure has been updated since verification** on the date shown. If a value here ever
   disagrees with the private record, the private record is authoritative and this index is
   wrong.

## External claims and citations

Anything sourced from outside my own measurements is attributed, not adopted as a result.

| External statement | Source | How it is treated here |
|---|---|---|
| The knowledge-layer framework's architecture and purpose | arXiv 2608.27454, *WikiSkill: Compiling Agent Experience into Persistent Knowledge for Skill Evolution* | Named as the framework's own description; my implementation and findings are separate |
| The decision model's category and training objective (typed probabilistic decisions, calibration-based training) | Vendor material for Jev (TypeSafe AI) | Described as the vendor's framing; no vendor performance figure is repeated as a result |
| The code-map tool's capabilities and licensing | Vendor material for Graft (Nanonets) | Capabilities observed directly; public benchmark figures **not** repeated as results |
| Publicly circulated benchmark figures for the code-map tool | Vendor/community posts | Deliberately excluded — my own canary is the only figure reported, because it is the only one measured on this workload |

## Derived and non-measurement values

| Value | Type | Basis |
|---|---|---|
| ~5× overstatement in the tool's own saved-token readout | Derived | Ratio of the tool's claim to its measured consumption, same run |
| "Narrower than the headline suggests" (domain fit) | Qualitative finding | Batch outcomes across domains in the private experiment record |
| Adoption verdicts (adopt / adopt narrowly / park / publish unproven) | Judgement | Stated as reasoning in `docs/evaluation-method.md`, with conditions attached |

## Companion rule

The system that produces the ledger figures refuses to estimate a missing value and records it
as "unavailable". This index follows the same rule at the writing layer: a gap is stated as a
gap, and a vendor's estimate is labelled as an estimate.
