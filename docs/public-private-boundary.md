# Public / private boundary

What this repository documents, and what it deliberately withholds — with the reasoning, so a
reader can judge the boundary rather than take it on trust.

## Principle

The valuable public material is **design reasoning and measured outcomes**. The material that
must stay private is anything that identifies an environment, a person, a client, or a
credential — and anything whose publication would breach a data or licensing obligation.

Where those two overlap, the design reasoning is rewritten generically rather than published
with identifying detail removed but structure intact.

## Published

- Architecture and component rationale.
- Measured comparisons, with method stated and any vendor-supplied claim labelled as such.
- Promotion gates, success criteria and stop conditions.
- Negative results and unproven hypotheses.
- Aggregate counts of activity (e.g. recorded tasks, procedures) — **counts of records, not
  quality claims**.

## Withheld

| Withheld | Reason |
|---|---|
| Credentials, tokens, keys, cookies | Secrets and attack surface |
| Host names, tunnel addresses, ports, service topology | Enables targeting; irrelevant to the design argument |
| Session identifiers and routing keys | Even hashed, these are operational data |
| Real task rows, per-task costs, project names in the ledger | Mixes client, organisational and personal work |
| Experiment evidence databases | Contains operational metadata by design; results are published from reports instead |
| Subscriber-press capture pipeline internals | Access terms and licensing; it is described as a boundary, never as a procedure to copy |
| Personal profile identities and role files | Publishing another person's or profile's identity file is not mine to do |
| Live drafts, article text, or named subjects from editorial work | Defamation and privacy exposure for living people |

## Rules applied while writing

1. **No reconstruction.** If a number is not recorded, the text says "unavailable". Nothing is
   inferred, estimated or back-filled to make a table look complete.
2. **No de-anonymised examples.** Synthetic or fixture values are used where an example is
   genuinely needed; real values are never partially masked.
3. **No procedure disclosure.** For pipelines governed by access terms, the writeup describes
   the obligation ("full-fidelity preservation with provenance") rather than a recipe.
4. **Human review before publication.** Anything describing editorial or client work is
   reviewed by the owner before it goes public.
5. **Private repositories first.** New case-study repositories are drafted private and only
   made public once content review is complete.

## Why this section exists at all

A portfolio that only lists capabilities asks the reader to trust the author. A boundary
section lets the reader verify that sensitive material was *considered and excluded on
purpose*. In work adjacent to public-sector and editorial data, that is itself the credential.
