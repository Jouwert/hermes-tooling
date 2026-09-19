# hermes-tooling

Case studies in extending my own agent platform — what I built, what I measured, and what I
decided not to keep.

This repository documents practical work on the agent infrastructure I use for real
organisational AI projects. It is deliberately documentation-first: the point is the
engineering judgement behind each decision, including the ones that ended in "parked" or
"proven not worth it".

## The real problem

I run a single-operator AI agent platform as my working environment for public-sector and
organisational AI deployment. It does the same things a team's internal tooling would do:
route work between models, hand tasks to specialist agents, keep durable project memory, and
account for what each completed task cost.

That creates three recurring problems which no off-the-shelf product solved for me:

1. **Attribution.** Standard usage dashboards count tokens per day. I needed cost and outcome
   per *completed task*, because that is the unit of work a client actually pays for.
2. **Memory that survives a session.** Agent sessions are stateless. Long projects were being
   restarted from a blank context, which is both wasteful and a quality risk.
3. **Knowing when to stop.** Agentic tooling invites unbounded cleverness. Several of the
   components here exist specifically to make the platform *smaller* or more decisive.

## Who used it

One operator — me — against real project work, over months. This is not a product with users;
it is a working environment that had to hold up under deadlines. That is the honest scale of
it, and it is also why every entry in this repo has a concrete operational reason.

## Deployment constraints

These shaped nearly every decision:

- **One Linux host, one operator.** No cluster, no dedicated ML infrastructure, no on-call.
  Anything that needed a second machine was rejected on those grounds alone.
- **Subscription and quota limits.** The platform runs against quota-limited model access.
  An early single-model setup consumed roughly 29% of its five-hour primary quota in about
  thirty minutes of heavy use — which is what forced a routing and delegation design rather
  than "use the biggest model for everything".
- **Privacy boundaries are non-negotiable.** The platform is used alongside public-sector,
  client and personal work. Experimental tooling must be able to run without any of that data
  reaching it. Several components were designed around that constraint rather than in spite
  of it.
- **No production rewrites.** Telemetry work had to observe using existing records without
  rewriting history, and had to be allowed to report "unavailable" rather than estimate.

## Trade-offs I would defend

**Adopt before building.** Where a third-party tool did the job, I adopted and *measured* it
instead of writing my own (see Graft below). My contribution is the adoption judgement and the
evaluation, not the code — and the writeup says so.

**Cheap models for mechanical work, one strong model for judgement.** Routing by role rather
than by size. The interesting finding was not the saving; it was that the cheap tier is
adequate for maintenance work and not adequate for editorial judgement, and being honest about
which is which matters more than the invoice.

**Measure with gates, or do not claim.** Every component that produces a number here declares
its success criteria *before* it runs, and is allowed to return "no durable learning" or
"unproven". Two of the entries below are published as negatives on purpose.

**Documentation as part of the engineering.** The discipline of writing the boundary —
what this tool does *not* do, what it must not receive — is what made several of these safe to
run at all.

## What is in here

| Component | What it does | Honest status |
|---|---|---|
| **WikiSkill** | Evidence-linked knowledge wiki sitting between raw experience and executable procedures | Passive layer proven; central architectural claim **not** proven — published as a negative |
| **Jev** | Evaluating a small typed decision component beside the agent loop (scoring, routing, semantic gates) | **Parked** by choice; no integration in the production path |
| **Graft** | Third-party codebase mapping — dependency graph before grep/read cycles | Adopted and **measured**; kept for one narrow capability |
| **Orchestration & delegation** | One default agent; specialist profiles and review lanes when a task genuinely crosses roles | Real but **lightly used** — do not inflate the scale |
| **Task ledger** | Cost and outcome per completed task, from a deterministic recorder | In daily use; 392 tasks recorded at time of writing |
| **Shadow experiment lifecycle** | Bounded experiments alongside the gateway, with pre-declared promotion gates | Running; design published, results withheld until the window closes |
| **Session continuity & memory** | Continuity file plus procedures-as-memory so lapsed sessions resume | In daily use |
| **Model routing & quota discipline** | Per-role model choice with fallbacks and quota awareness | In daily use; written as method, not as a static price table |

Each component is described in `docs/architecture.md`, with the boundary and the specific
honesty caveats in the same document.

## What is deliberately omitted

No credentials, host names, tunnel addresses, real task rows, per-task costs, experiment
databases, subscriber-press pipeline internals, or personal profile identities. See
`docs/public-private-boundary.md` for the full list and the reasoning.

## The test I apply to every entry

Does this show **deployment judgement** — why the thing was built this way, what was
intentionally kept simple, what was kept private — rather than technical complexity for its
own sake? Where the answer was no, the entry got cut or reduced to one line.

## Licence and scope

Documentation in this repository describes systems operated privately. Nothing here is
deployable as a product, and no client, organisational or personal data is included.
