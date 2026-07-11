# Goals

**Status**: Active
**Date**: 2026-07-12
**Work wave**: semantic foundation and first requirement admission

## Governing Constraint: Treatment Authorship At Scale

Treatment authorship is the dominant viability risk for the trade algebra.
Formal treatment application is not sufficient if semantic correspondence,
entity resolution, evidence collection, expert review, and treatment
maintenance remain unbounded manual work.

This wave must expose treatment-production cost and quality. Assisted or
probabilistic methods may generate candidates, but governing authority admits
treatments. The product must measure candidate coverage, uncertainty, review
latency, conflict, reuse, repair, and supersession rather than assuming a
mapping already exists.

## G-001 - Ratify Orthogonal Temporal Semantics

Define domain temporal coordinates, audit and knowledge temporal coordinates,
and correction relations as separate product structures before deriving
temporal requirements.

Closure signals:

- economic and lifecycle roles cannot alias observation or admission time;
- correction is represented as supersession, not as a temporal-role enum value;
- bitemporal replay questions are expressible without overloading domain dates;
- the seven-date commodity fixture is explicitly one test instance rather than
  the product taxonomy.

## G-002 - Ratify Treatment Authorship And Admission

Define the lifecycle by which correspondence evidence becomes a treatment
candidate, receives review, becomes admitted for a bounded scope, is reused,
and is repaired or superseded.

Closure signals:

- every treatment has recoverable author or generator lineage;
- evidence, uncertainty, coverage, authority, decision, and intended use are
  explicit;
- candidate generation cannot mutate accepted semantic truth;
- treatment effort and reuse can be measured across business contexts;
- unresolved entity and semantic correspondence remains visible as gaps.

## G-003 - Derive The First Requirement Families

Derive requirements only after G-001 and G-002 close their product-language
ambiguities.

The first requirement families cover:

- contextual claims, identity, and local authority;
- orthogonal temporal semantics and correction;
- treatment authorship, admission, adjoints, fidelity, and scale evidence;
- immutable object cuts and composition;
- CDM references and bounded-fidelity lifts;
- intent lineage, gain bases, expected/executed/actual gain, and attribution;
- projection, gaps, replay, and forbidden runtime authority.

## G-004 - Prove The Semantic-Plumbing Slice

Specify and then realize the two-business commodity fixture as a proof of
contextual composition, not as proof of broad CDM lifecycle coverage.

The slice must prove label independence, context collision, local authority,
orthogonal time, treatment authorship, incomplete and conflicting data,
correction, adjoint interpretation, gain attribution, and replay.

## G-005 - Qualify The CDM Lifecycle Boundary Separately

Define a separate fixture pack over well-covered CDM products and lifecycle
events. It must exercise partial fills, allocations, amendments, novations,
multi-trade netting or compression, and a relevant corporate-action event.

CDM lifecycle-spine claims remain bounded to the event and product families
that pass this qualification.

## Sequence

```text
G-001 temporal model
  + G-002 treatment lifecycle
    -> G-003 requirement families
         +-> G-004 semantic-plumbing proof
         +-> G-005 CDM lifecycle qualification
```

G-004 and G-005 share the admitted semantic kernel but close different claims.
Neither may be cited as evidence for the other.

## Excluded From This Wave

- live trading-feed integration;
- autonomous trading or lifecycle action;
- GPU performance work;
- production deployment topology;
- broad asset-class rollout;
- user-interface implementation;
- activation of the candidate TypeScript tenant before G-001 through G-003
  produce ratified requirement and design boundaries.
