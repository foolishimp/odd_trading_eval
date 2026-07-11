# Intent

**ID**: INT-ODD-TRADING-EVAL-001
**Status**: Active
**Date**: 2026-07-12
**Change class**: `intent_reprice`
**Profile**: product intent
**Supersedes**: the provisional forensic-attribution intent in `BOOTSTRAP.md`

## Purpose

`odd_trading_eval` exists to make trade meaning coherent across trading
businesses without flattening their local models into one warehouse schema.

Trading businesses often record the same economic reality under different
names, and reuse the same name for different roles. Dates, identifiers,
quantities, states, and classifications are meaningful only within the
function, authority, lifecycle position, and treatment that produced them.
Structural consolidation preserves values while losing that context.

The product builds a governed **trade algebra** over those local meanings. The
algebra identifies trade-domain objects in bounded contexts, preserves the
evidence and authority supporting them, and composes them through explicit
semantic treatments. Meaning resides in contextual use within the algebra, not
in source labels or centrally preferred column names.

Treatment discovery, authorship, and admission are part of the product problem.
The product does not assume that cross-context mappings already exist or that
their creation is cheap. It must make the evidence, uncertainty, authority,
review cost, reuse, and supersession of treatments observable so semantic
composition can scale beyond isolated expert mappings.

The algebra uses FINOS CDM as the contract and transaction-lifecycle spine for
qualified product and event scope. CDM does not replace source-business
meaning, strategy intent, gain semantics, valuation context, attribution, or
local operational authority. Those concepts remain explicit and reference CDM
where CDM owns the relevant legal or economic representation.

The product is descriptive and evaluative before it is generative. It first
proves that trade, lifecycle, temporal, gain, and attribution meaning can be
reconstructed and projected lawfully. Later action or optimization capabilities
must derive from that accepted semantic substrate and remain separately
governed.

This intent is independent of programming language, storage engine, GPU stack,
user interface, and deployment topology. Those choices belong to requirements,
design, and build tenants.

## Constitutional Lineage

The superseded bootstrap defined a post-trade forensic attribution engine. This
intent retains forensic attribution as a projection and proving use case inside
the broader contextual trade algebra. Bootstrap Eval, cascade, storage, and
tenant claims remain source candidates rather than inherited product law.

## Outcomes

- Preserve each trading business as a bounded semantic context with explicit
  local authority.
- Interpret every admitted value through its business function, semantic role,
  domain-time meaning, audit/knowledge-time posture, provenance, and treatment
  history.
- Publish immutable, evidence-backed trade-domain object cuts whose identity
  survives changes in labels and local representations.
- Compose local trade objects through treatments that declare preservation,
  changed meaning, loss, surplus, ambiguity, and interpret-back behavior.
- Make treatment discovery, authorship, review, admission, reuse, and
  supersession governable and measurable rather than hiding that work inside
  integration code.
- Use CDM as the canonical contract and lifecycle representation for qualified
  product and event scope within the wider trade continuum without treating CDM
  as the whole semantic universe.
- Connect strategy and other intent lineage to execution, CDM lifecycle state,
  position, valuation, expected gain, executed gain, actual gain, and
  attribution.
- Derive P&L, risk, accounting, regulatory, commentary, data-quality, and audit
  surfaces as governed projections over one semantic kernel.
- Keep incomplete, conflicting, stale, and irreconcilable data visible as
  explicit gaps rather than resolving them through silent consolidation.
- Make every material projection replayable to its source evidence, accepted
  semantic claims, object cuts, treatments, model versions, and projection
  contract.
- Support homeostatic evaluation against governed models of good without
  requiring every source of intent to originate from a computed gap.

## Directional Boundary

The current direction includes:

- contextual trade semantics across multiple trading businesses;
- trade and lifecycle identity;
- orthogonal domain-time, audit/knowledge-time, and correction semantics;
- treatment discovery, authorship, admission, and adjoint interpretation;
- CDM contract and lifecycle references;
- strategy and intent lineage;
- expected, executed, and actual gain;
- attribution and explainable projections;
- explicit semantic, data-quality, model, and control gaps;
- GTL-published domain construction with ABG-owned traversal and runtime truth.

The current direction excludes:

- replacing source systems of record;
- a universal enterprise warehouse schema;
- resolving semantic conflict by selecting one business label as canonical;
- autonomous order submission or trade execution;
- a product-local scheduler, replay engine, continuation loop, or re-entry
  controller that replaces ABG;
- GPU acceleration as a constitutional requirement;
- treating generated commentary, dashboards, catalogs, or schemas as semantic
  authority;
- mutating accepted CDM economic meaning through local extensions.

## Constraints

- Source systems remain sovereign for the operational facts they enact.
- A source field or value is evidence, not meaning in isolation.
- Every admitted claim identifies its bounded context, source authority,
  semantic role, domain temporal coordinate where applicable, audit temporal
  coordinates, and provenance.
- Different labels may denote the same role; identical labels may denote
  different roles. Neither case is resolved from spelling alone.
- Every cross-context mapping is a semantic treatment with declared fidelity,
  ambiguity, and loss.
- Every admitted treatment traces to an author or generating process, source
  evidence, candidate method, uncertainty, coverage, review decision, governing
  authority, and supersession state.
- Every treatment that supports downstream use provides an interpret-back
  contract or explicitly declares why interpretation back is partial.
- Published object cuts are immutable and superseded explicitly.
- Domain temporal coordinates express economic and lifecycle meaning. Audit
  temporal coordinates express observation, receipt, admission, and knowledge
  posture. Corrections are supersession relations with their own evidence, not
  members of either temporal-role vocabulary.
- Intent carries lineage from its actual source, including human, imported,
  contractual, strategy-generated, or gap-generated intent.
- Gain vectors declare basis, horizon, reference state, observation state,
  units, and lineage.
- Attribution includes expected, executed, and actual gain; it does not hide
  execution mismatch inside residual P&L.
- CDM mappings pin the CDM version and declare mapping fidelity.
- Deterministic or probabilistic treatment generation produces candidates only.
  Candidate correspondence and synthesis remain evidence until admitted by the
  governing authority.
- GTL graph functions are the constructive domain carrier. ABG owns graph-call
  lifecycle, traversal, event admission, replay, continuation, and re-entry.
