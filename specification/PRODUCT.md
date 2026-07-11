# Product

**ID**: PROD-ODD-TRADING-EVAL-001
**Status**: Active
**Date**: 2026-07-12
**Supersedes**: the provisional product definition in `BOOTSTRAP.md`

This document defines the current product surface of the mutable
`odd_trading_eval` source project. It defines the next released product; it is
not a release artifact or installed product instance.

The product is governed by `SPEC_METHOD.md`, refined by
`WORLD_MODEL_METHOD.md` for semantic publication and `ODD_METHOD.md` for
GTL/ABG realization.

## Constitutional Lineage

`BOOTSTRAP.md` defined a post-trade forensic attribution engine. The current
product subsumes that engine as one evaluation projection over a contextual
trade algebra. Bootstrap-specific Eval catalogs, cascade mechanics, storage,
and tenant choices are not inherited unless ratified through requirements and
design.

## Product Position

`odd_trading_eval` is a governed trade-algebra construction and evaluation
product.

It observes evidence from trading businesses, reconstructs the contextual
meaning those businesses enact, and publishes an immutable semantic trade
continuum. The continuum connects source-business objects, strategy and other
intent, execution evidence, CDM contract and lifecycle state, position,
valuation, gain, attribution, and projections without erasing local authority.

The product serves organizations in which many trading businesses represent
related economic reality through incomplete, divergent, and locally meaningful
data models. It replaces label-led consolidation with context-led composition.

The current product evaluates and explains admitted trade reality. It does not
submit orders, book trades, mutate source systems, or autonomously change
models and requirements.

## Primary Product Outcome

Given a downstream trade, position, valuation, P&L, risk, or control figure,
the product answers:

```text
what object this is evidence of
which business context and authority gave it meaning
what each date, identifier, state, and amount means in that context
which treatments preserved or changed that meaning
which CDM contract and lifecycle state it references
which intent and gain basis apply
what is known, missing, conflicting, stale, or ambiguous
which projection produced the figure
whether the result can be interpreted back to source reality
```

## Single Semantic Truth Surface

The product's semantic truth surface is the published trade-algebra layer.

It contains:

- qualified semantic claims;
- governed attribute-ledger entries;
- immutable trade-domain Markov-object cuts;
- domain temporal coordinates;
- audit and knowledge temporal coordinates;
- correction and supersession relations;
- treatment candidates, admission decisions, treatment surfaces, and adjoint
  mappings;
- covariance and causal edges;
- composed trade-continuum cuts;
- CDM references and declared-fidelity lifts;
- intent lineage;
- gain-vector bases and gain observations;
- attribution results;
- projection contracts and results;
- explicit gaps and ambiguity.

Raw source records remain operational evidence. CDM objects remain canonical
for the contract and lifecycle semantics they represent. P&L, risk, schemas,
reports, commentary, catalogs, and user interfaces remain projections. None of
those surfaces replaces the wider trade-algebra truth surface.

## Semantic Chain

The product makes this chain explicit:

```text
source-business function
  -> source observation
  -> qualified semantic claim
  -> attribute ledger
  -> immutable object cut
  -> treatment candidate and evidence
  -> governed treatment admission
  -> admitted semantic treatment
  -> composed trade-continuum cut
  -> CDM contract/lifecycle reference
  -> position and valuation observation
  -> expected / executed / actual gain
  -> attribution
  -> governed projection
  -> gap and re-entry evidence
```

Each transition preserves lineage and declares partiality. No transition may
invent missing meaning to keep the chain moving.

## Product Terms

- **Trading Business Context**: a bounded business function with its own object
  meanings, lifecycle, policy, source authority, and local vocabulary.
- **Qualified Semantic Claim**: a source-backed assertion whose meaning is
  qualified by context, authority, semantic role, domain temporal coordinate
  where applicable, audit temporal coordinates, and evidence.
- **Semantic Role**: the function a value performs in an expression or
  lifecycle, independent of its source label.
- **Domain Temporal Coordinate**: a date or time qualified by its economic or
  lifecycle role inside a trading context, such as trade, execution, effective,
  fixing, delivery, settlement, valuation, or reporting time.
- **Audit Temporal Coordinate**: a system-time fact describing when evidence was
  observed, received, admitted, or knowable. Audit time does not redefine the
  economic role of a domain date.
- **Knowledge Interval**: the audit-time interval during which an admitted claim
  or cut was the product's current accepted knowledge.
- **Correction Relation**: an evidence-backed supersession edge from a prior
  claim, event, or cut to its correction. `correctedAt` is audit metadata on the
  relation; correction is not a domain temporal role.
- **Attribute Ledger**: the append-only accepted record of qualified claims over
  stable object identity.
- **Trade Object Cut**: an immutable, evidence-backed Markov-object projection
  representing a coherent trade-domain object in a declared context.
- **Treatment**: a governed reinterpretation from one context or object basis
  into another. It declares preserved structure, changed meaning, loss,
  surplus, ambiguity, and authority.
- **Treatment Candidate**: an authored or generated proposed treatment carrying
  source evidence, method, uncertainty, coverage, author identity, and intended
  target use. A candidate cannot transform accepted product truth.
- **Treatment Admission**: an authority decision that accepts, rejects, or
  returns a treatment candidate for repair and records reviewer, rationale,
  evidence basis, scope, knowledge interval, and supersession.
- **Adjoint Mapping**: the interpret-back contract paired with a treatment.
- **Trade Continuum Cut**: a versioned composition of trade-domain object cuts
  and their semantic, temporal, causal, covariance, and treatment edges.
- **CDM Reference**: a version-pinned reference to a CDM object that owns
  contract or transaction-lifecycle meaning.
- **CDM Lift**: a governed treatment from admitted execution or lifecycle
  evidence into CDM, with explicit fidelity and lineage.
- **Intent Lineage**: the admitted origin and history of intent. Sources include
  human, imported, contractual, strategy-generated, and gap-generated intent.
- **Strategy Run**: one application of a versioned strategy definition to
  declared marketplace, portfolio, parameter, constraint, and model state.
- **Gain Vector Basis**: a versioned coordinate system defining gain dimensions,
  identities, units, aggregation, and compatible projections.
- **Gain Vector**: an observation in a declared basis and horizon between an
  explicit reference state and observation state.
- **Attribution**: an explanation of differences among expected, executed, and
  actual gain, including a declared residual.
- **Projection Contract**: a versioned declaration of inputs, output schema,
  basis, transformation, fidelity, validation, and consumer purpose.
- **Gap Assessment**: typed evidence of missing, conflicting, stale,
  semantically incompatible, economically divergent, or policy-breaching
  state. A gap may support re-entry or intent but is not the only source of
  intent.

## Trade Algebra

The trade algebra is a family of typed partial operations over semantic
objects. Each operation returns a result or an explicit gap.

```text
Qualify
  SourceObservation x TradingBusinessContext
  -> QualifiedSemanticClaim | Gap

Cut
  AttributeLedger x IdentityDirection x Context
  -> TradeObjectCut | CandidateCut

AuthorTreatment
  SourceCut x TargetContext x CorrespondenceEvidence x AuthorContext
  -> TreatmentCandidate | Gap

AdmitTreatment
  TreatmentCandidate x AuthorityDecision
  -> AdmittedTreatment | RejectedTreatment | RepairGap

Treat
  TradeObjectCut<A> x AdmittedTreatment<A, B>
  -> TreatedCut<B> + TreatmentEvidence | Gap

Compose
  TradeObjectCut[] x DeclaredEdges
  -> TradeContinuumCut | Gap

LiftCDM
  TradeContinuumCut x CdmMappingContract
  -> CdmReference[] + MappingEvidence | Gap

ObserveGain
  ReferenceState x ObservationState x GainVectorBasis x Horizon
  -> GainVector | Gap

Attribute
  ExpectedGain x ExecutedGain x ActualGain x AttributionContract
  -> Attribution | Gap

Project
  TradeContinuumCut x ProjectionContract
  -> ProjectionResult | Gap

Evaluate
  TradeContinuumCut x GovernedEvaluationModel
  -> GapAssessment[]
```

The operations obey these product laws:

- **Context law**: values are interpreted through qualified role and context,
  never through labels alone.
- **Identity law**: composition preserves object identity and cut version.
- **Authority law**: local operational authority remains visible after
  composition.
- **Temporal orthogonality law**: domain time, audit/knowledge time, and
  correction relations remain separate and jointly queryable; ordering does
  not substitute for role.
- **Treatment authorship law**: no treatment is admitted without recoverable
  authorship or generation lineage, evidence, uncertainty, scope, review, and
  authority. Treatment-production cost and coverage remain observable.
- **Treatment law**: transformation declares preservation, change, loss,
  surplus, ambiguity, and adjoint behavior.
- **Partiality law**: missing or incompatible meaning yields a gap rather than a
  guessed value.
- **Basis law**: gain and attribution operations require compatible versioned
  bases.
- **Projection law**: projection fidelity and loss are declared and testable.
- **Replay law**: published results reconstruct from admitted evidence and
  pinned semantic, mapping, model, and projection versions.

## CDM Boundary

CDM owns the normalized representation of in-scope financial product economics,
trade state, business events, legal agreements, settlement terms, and
transaction lifecycle.

`odd_trading_eval` owns:

- source-business contextual meaning;
- semantic qualification and treatments;
- object-cut identity and composition;
- intent and strategy lineage outside CDM;
- gain-vector and attribution semantics;
- projection and evaluation contracts;
- domain interpretation of gaps.

The product references CDM objects rather than duplicating them. Local
extensions use a product-owned namespace unless accepted into the governed CDM
distribution. A partial or lossy CDM lift remains visibly partial or lossy and
cannot become canonical by admission alone.

## Homeostatic Evaluation Boundary

A governed model of good may evaluate the trade continuum and emit gap
assessments. Those assessments may support proposed hedge, rebalance, reprice,
investigate, repair, block, or escalation intent.

Homeostatic evaluation is a projection over admitted semantic truth. It does
not own source truth, object identity, CDM state, or action authority. Human,
imported, contractual, and strategy intent remain lawful independent sources.

The current product publishes gap and re-entry evidence. ABG and the governing
product boundary own traversal and admission; an execution product would own
any future material action.

## Product Capabilities

The current product definition provides these capability families:

1. source-context registration and authority declaration;
2. semantic observation, qualification, and assurance;
3. attribute-ledger and immutable object-cut publication;
4. treatment discovery, authorship, evidence, review, admission, reuse, and
   supersession;
5. treatment, adjoint, covariance, and causal-edge publication;
6. trade-continuum composition and query;
7. CDM reference, lift, lifecycle, and fidelity interpretation;
8. intent and strategy lineage;
9. gain-basis registration and expected/executed/actual gain observation;
10. attribution and residual evaluation;
11. P&L, risk, data-quality, commentary, and audit projections;
12. gap publication and lawful re-entry evidence;
13. replay, lineage, and projection proof through GTL/ABG runtime truth.

## First Product Slice: Semantic Plumbing

The first slice uses one synthetic commodity basis trade represented by two
trading businesses. It proves contextual semantic plumbing. It does not qualify
the broader claim that CDM is the product's lifecycle spine.

The source fixtures deliberately contain:

- different labels for equivalent semantic roles;
- identical labels with different contextual meanings;
- seven fixture-specific domain dates with distinct economic or lifecycle
  roles;
- separate observed, received, admitted, and knowledge-time coordinates;
- one missing value;
- one conflicting value with different local authorities;
- one correction relation that supersedes an earlier claim after admission;
- correspondence evidence from which at least one treatment candidate must be
  authored and admitted rather than supplied as accepted fixture truth;
- strategy intent, execution evidence, and a CDM lifecycle reference;
- expected, executed, and actual gain in one versioned commodity basis;
- a P&L explain projection with a visible residual.

The slice succeeds when the product:

- preserves both local contexts and authorities;
- identifies equivalent meaning without depending on label equality;
- keeps semantically different values separate despite label equality;
- keeps domain time, audit/knowledge time, and correction relations separate;
- records treatment authorship effort, evidence coverage, uncertainty, review
  decision, reuse scope, and unresolved correspondence gaps;
- publishes immutable object cuts and explicit treatments;
- interprets forward and back through the treatment chain;
- exposes missing, conflicting, stale, and lossy state as gaps;
- exercises a pinned CDM reference and lift boundary without claiming broad
  lifecycle qualification;
- attributes expected versus executed versus actual gain;
- reconstructs the final projection from admitted evidence and pinned versions;
- proves that product code cannot take ABG continuation, replay, or re-entry
  authority.

The number seven is a fixture property, not a product temporal taxonomy. Future
contexts may publish different domain temporal roles without extending the
audit-time model.

## CDM Lifecycle Qualification Slice

CDM lifecycle qualification is a separate product proof over products and
events with strong CDM coverage. The semantic-plumbing slice cannot close this
claim.

The qualification fixture pack must cover:

- execution assembled from partial fills and allocation;
- amendment and novation with before/after trade-state lineage;
- a netting or compression event affecting multiple trade states;
- a corporate-action-driven lifecycle event where the selected product makes
  that event relevant;
- separation of CDM event/effective dates from workflow and audit timestamps;
- late, rejected, partial, and lossy source-to-CDM mappings;
- reconstruction of each resulting state from pinned CDM version, mapping
  contract, source evidence, and admitted event lineage.

The product may call CDM its lifecycle spine only for the qualified scope. Any
unqualified asset class or event family remains explicitly unsupported or
partial.

## Product Lifecycle

The source project currently contains active goals, intent, and
product-definition surfaces plus candidate design source material. It has no
released product and no active realization tenant.

A candidate tenant may become active only after requirements and ratified
design define:

- the semantic carrier schemas;
- the treatment-candidate, authorship, admission, reuse, and supersession
  contract;
- the orthogonal domain-time, audit/knowledge-time, and correction contract;
- the public graph-function catalog and GTL module;
- the CDM version and adapter boundary;
- ABG invocation and event-admission boundaries;
- the semantic-plumbing fixture and proof contract;
- the separate CDM lifecycle qualification boundary;
- release, install, compatibility, and supersession behavior.

Prior bootstrap and architecture documents remain source material. They do not
expand the current product boundary unless their claims are ratified through
this product surface, requirements, and design.
