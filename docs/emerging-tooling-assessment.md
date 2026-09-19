# Three tools worth watching — and how I judge them

A portfolio of systems says what someone built. This document says something different: which
emerging tools I think change what is possible, who each one is actually for, and under what
conditions I would put it near an organisation's work.

All three are genuinely useful and none is well understood. Each one is worth understanding
because of the *class of problem* it removes, not because of the tool itself.

## How I assess emerging tooling

Six questions, asked before enthusiasm is allowed:

1. **What class of problem does it remove?** Not "what does it do" — which existing cost does it
   delete?
2. **Who actually has that problem, and what does it cost them today?**
3. **Is the claimed benefit measurable — and did it survive being measured?** Vendor numbers are
   claims until reproduced on your own workload.
4. **What does adoption cost?** Setup, refresh, trust, and vendor dependency all count.
5. **What must never be handed to it?** A tool with no written boundary is not production-ready,
   whatever its benchmark says.
6. **What would make me stop using it?** No exit criterion, no adoption.

This document applies that frame to three tools I have actually run against real work.

---

## 1. WikiSkill — persistent knowledge for skill evolution

**What it is.** A framework described in arXiv 2608.27454, *WikiSkill: Compiling Agent Experience
into Persistent Knowledge for Skill Evolution*. Instead of pushing everything an agent learns into
ever-longer instructions, it co-evolves the agent's skills with a persistent, evidence-linked wiki
that sits between raw experience and the executable skill. (Attribution kept to the paper itself —
the abstract page lists authors, and I have not verified any institutional affiliation, so I do not
assert one.)

**The problem class it removes.** Agents forget *why*. Instructions accumulate, context gets
re-derived from scratch, a lesson learned in one session never reaches the next, and failed
routes get retried because nothing recorded that they failed.

**Who it is for.** Teams running agents across many recurring workflows — anyone whose
instruction set has quietly become a junk drawer, and anyone losing the same hour of context
every week.

**What I did.** Implemented the architecture in a different setting from its authors: a
single-operator platform with a large procedure library. I froze evidence corpora by hash, ran
the passive knowledge layer across several domains, and attempted the comparison the framework's
central claim depends on.

**What I found.**

- The passive layer genuinely works: it accumulates cross-case interpretation, preserves
  provenance and contradictions, holds failed routes, and — the strongest result — a *fresh*
  session with no access to the originating conversation recovered the correct verified route
  from the wiki alone.
- Value is strongly **domain-dependent**. Interpretive domains with ad-hoc procedures gained a
  lot; operational domains that already had mature validators gained little.
- The best single outcome was a batch that concluded **"no durable learning"** and added nothing,
  because an existing validator already enforced the lesson. A framework that can return zero
  pages is better designed than one that always produces output.
- The claim I could **not** reproduce is the central one: that wiki-mediated skill evolution beats
  direct editing. In my setting that comparison did not run, so I published it as unproven rather
  than implying a result.

**What would have to be true for an organisation.** Recurring interpretive work across many
sessions; willingness to keep evidence inspected rather than summarised; and a human reviewer who
will reject plausible-but-unnecessary changes. Without the reviewer, the wiki quietly becomes a
second documentation site nobody reads.

**Adoption preconditions.** Define what counts as evidence before starting; freeze it by hash so
claims stay re-checkable; expect zero-output batches to be correct; cap new pages per cycle;
never edit a live procedure from wiki material without an explicit approval step.

**My verdict.** Conceptually strong and unusually honest about its own limits. **Promising for
teams with real cross-session knowledge loss; not a general upgrade, and expensive if adopted as
one.** Maturity: research framework, production-ready ideas, meaningful adoption work.

---

## 2. Jev — decisions instead of paragraphs

**What it is.** Jev, by TypeSafe AI, is a "System One" model: it returns **typed, probabilistic
decisions** rather than prose. It is trained with what the vendor calls Reinforcement Learning
for Calibrated Decisions, optimising for probabilities that match observed outcomes. Vendor
material claims large speed advantages for this class of call — treat those as claims, not
results.

**The problem class it removes.** A large share of agent steps do not need language at all. *Does
this message need escalation? Is this retrieved passage relevant? Should this run be retained?*
Today those are answered by asking a generative model to write a paragraph, then parsing a word
out of it — slow, expensive, and unreliable at exactly the boundary that matters.

**Who it is for.** Anyone with semantic gates in a pipeline: routing, ranking, escalation,
moderation pre-filters, retention scoring. Especially teams whose bill is dominated by thousands
of tiny classification calls that each look cheap.

**What I did.** I audited my own agent loop for decision-shaped call-sites, designed a
provider-independent baseline (label definition, evidence fields, success criteria, cost ceiling,
stop condition) and then stopped — deliberately — at the access boundary, with no SDK installed
and no data leaving the machine.

**What I found.** The category is the important part, and it is under-exploited. The productive
question is not *"is this better than an LLM?"* but *"which of my workflow steps are decisions
rather than text?"* — an inventory most teams have never made. That audit is useful even if the
tool is never adopted, and it is where the cost savings actually come from.

**What must never be handed to it.** Private, client, municipal or personal data before terms,
retention and data location have been checked — and never broad production logging "to see what
happens" while a vendor relationship is still being evaluated.

**What would have to be true.** A genuinely decision-shaped call-site; a defined label; agreed
success criteria and a cost ceiling; a stop condition; and one instrumented call-site, not a
platform-wide rollout or a speculative abstraction layer built in anticipation.

**My verdict.** Early, but the category is real and will be normal within a few years. **The
transferable insight is the decision audit, not the vendor.** Maturity: new product, clear
category fit, procurement and data-governance work still ahead of it in most organisations.

---

## 3. Graft — giving agents a map instead of a memory problem

**What it is.** Graft, from Nanonets: an MIT-licensed CLI that turns a repository into linked
markdown nodes plus a per-symbol code graph, so a coding agent starts from a map instead of from
search. Public benchmarks circulate claiming substantial reductions in agent tool calls — again,
vendor and community numbers are claims.

**The problem class it removes.** An agent's real cost is *finding things*. Repeated
grep-and-read cycles burn context before any work happens, and blast-radius questions — *what
breaks if I change this?* — are close to unanswerable by text search at any budget.

**Who it is for.** Teams with a codebase an agent returns to more than once, where orientation
has become a visible share of the agent's budget. Not one-off scripts.

**What I did.** Adopted it, then ran a real three-file question two ways — map-first versus
search-first — scored against a ground truth derived from the source itself, on one machine.

**What I found.**

| | map-first | search-first |
|---|---|---|
| Tool calls | **3** | 10 |
| Tokens consumed | **7,479** | 11,015 |
| Wall time | 3.26 s | **0.049 s** |
| Correct files found | 3 / 3 | 3 / 3 |

- Roughly **3× fewer tool calls** and **~1.5× fewer tokens**, in the same direction as the public
  claims — but far less dramatic than the marketing framing.
- **~66× slower**, because each CLI call pays process start-up. The win is not speed.
- The capability search cannot match at any price is **exact call-graph edges**: who calls this,
  and what is downstream. That, plus fewer context-heavy reads, is the real value.
- The tool prints its own "tokens saved" estimate on every call. On my run it claimed ~39,624
  tokens saved while consuming 7,479. **It is an advertisement, not a measurement** — an overstatement
  of roughly 5× in the run I checked.
- Two practical traps: the most efficient verbs are only available outside the MCP surface, and
  under a gateway without an edit hook **the graph does not refresh itself** — a stale graph
  answers confidently and wrongly, so a sync check is mandatory before trusting it.

**What would have to be true.** A codebase you return to; a real orientation cost; and a refresh
habit. Adopting it for a one-off script is pure waste.

**My verdict.** The most immediately usable of the three, with the clearest boundary condition.
**Adopt it for recurring codebases, and measure it on your own workload — because the vendor's
number is not your number.** Maturity: usable today.

---

## Side by side

| | WikiSkill | Jev | Graft |
|---|---|---|---|
| Category | Knowledge architecture | Decision model | Code context |
| Maturity | Research framework | New product | Usable today |
| Removes | Cross-session forgetting | Paragraphs where a decision belongs | Orientation cost |
| Biggest gain for | Recurring interpretive work | Cheap semantic gates | Recurring codebases |
| Honest blocker | Central claim unproven in my setting | Data-governance work first | Refresh discipline required |
| What I'd tell a team | Prove it on one domain before scaling | Audit your decision-shaped steps first | Adopt where you return, measure locally |

## What this says about how I work

- I evaluate emerging tooling against **real workflows**, not demos.
- I separate **vendor claims from measurements**, and say which is which.
- I look for the **category insight** — the part that stays useful even if the tool does not.
- I write the **boundary** (what must never be handed over) as part of the design, not after it.
- I publish **conditions and negatives**, because a recommendation without a "what would change
  my mind" is not advice.

That is the part of the work I would defend in a procurement conversation: not that I know these
three tools, but that I can tell you where each one earns its place — and where it does not.