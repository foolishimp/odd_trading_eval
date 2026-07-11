# odd_trading_eval TypeScript Tenant

**Status**: Candidate
**Kind**: project-owned realization tenant
**Authority**: downstream of the shared `specification/` product line

## Candidate Position

This tenant is a potential TypeScript realization of the contextual trade
algebra defined by `specification/INTENT.md` and `specification/PRODUCT.md`.

It is not active implementation authority. It records a plausible realization
boundary so requirements and design can test whether the first product slice is
buildable without committing the product to a language or stack.

TypeScript is a suitable first semantic/control tenant because the initial
proof is dominated by typed semantic objects, discriminated contexts, schema
validation, graph-function publication, event envelopes, projection contracts,
and deterministic replay assertions. Numerical acceleration is not required to
prove those boundaries.

## Proposed Responsibility

The tenant would own:

- TypeScript representations of qualified claims, domain temporal coordinates,
  audit and knowledge temporal coordinates, correction relations, object cuts,
  treatment candidates, admissions, treatments, adjoints, continuum cuts,
  intent lineage, gain bases, attribution, projection contracts, and gaps;
- deterministic schema validation and semantic admission adapters;
- product-owned graph functions and function-catalog publication;
- the `odd_trading_eval` GTL module and outer callable carrier;
- adapters that reference a pinned CDM distribution without duplicating CDM
  types as local truth;
- deterministic CPU reference implementations for treatment, gain,
  attribution, and projection operations;
- fixture readers and proof projections;
- ABG-facing payload and evidence adapters;
- tests proving semantic laws and forbidden runtime authority.

The tenant would not own:

- source-system operational truth;
- CDM governance or mutation of CDM economic meaning;
- ABG graph-call lifecycle, traversal, event admission, replay, continuation,
  correction, closure, or re-entry;
- autonomous order submission, booking, or lifecycle action;
- a product-local scheduler or event store;
- GPU kernels as a prerequisite for semantic closure;
- user-interface product truth.

## Candidate Stack

The first design pass should evaluate:

| Surface | Candidate |
| --- | --- |
| Semantic language | TypeScript with strict branded and discriminated types |
| Runtime | Active Node.js LTS pinned at tenant activation |
| Contract validation | Generated JSON Schema plus deterministic runtime validation |
| Columnar interchange | Apache Arrow and Parquet |
| Reference analytical query | Embedded DuckDB |
| Decimal values | Exact decimal representation; no unqualified JavaScript number for money or quantity |
| CDM boundary | Version-pinned Rune/CDM JSON or generated adapter behind product-owned references |
| Constructive carrier | Published GTL graph functions and one public module |
| Runtime authority | Installed ABIogenesis/ABG public carrier |
| Proof | Node test runner plus replay fixtures and schema/property tests |

Specific libraries and versions require ADRs. The tenant must not encode a
library choice as product law.

## Proposed Public Carrier

The first outer carrier should represent one bounded semantic construction:

```text
odd_trading_eval.construct_trade_continuum
```

Candidate graph functions:

```text
qualify_source_observations
publish_trade_object_cuts
author_treatment_candidates
record_treatment_admissions
apply_semantic_treatments
compose_trade_continuum
lift_cdm_references
observe_gain_vectors
attribute_gain_delta
project_trade_evaluation
evaluate_trade_gaps
```

The names are design candidates. Ratified design must declare exact typed
inputs, outputs, closure obligations, evaluator regimes, lineage, and public or
helper role before implementation.

ABG invokes and traverses the published carrier. Each graph function performs
one admitted bounded step, publishes typed evidence, and yields control.

## First Realization Slice: Semantic Plumbing

The tenant should implement the two-business synthetic commodity-basis fixture
defined by `PRODUCT.md` before integrating a live trading feed. This fixture
does not qualify broad CDM lifecycle support.

Candidate flow:

```text
two source-business fixture packets
  -> qualified claims with contextual and domain-time roles
  -> audit / knowledge coordinates and correction relations
  -> attribute-ledger evidence
  -> immutable trade-object cuts
  -> authored treatment candidates with evidence and uncertainty
  -> recorded authority decisions
  -> admitted cross-context treatments and adjoints
  -> one composed trade-continuum cut
  -> pinned CDM references with mapping fidelity
  -> expected / executed / actual gain vectors
  -> attribution and P&L explain
  -> semantic and data-quality gaps
  -> ABG replay and lineage proof
```

The CPU reference path is the acceptance oracle. Python, CUDA, RAPIDS, or other
numerical adapters may be evaluated only after they can reproduce the reference
semantics within a declared tolerance and return results through the same
admission boundary.

## Required Proof

Tenant activation should require tests for:

- different labels carrying the same semantic role;
- the same label carrying different roles in different contexts;
- seven fixture-specific domain dates with no role collision;
- separation of domain time from observation, receipt, admission, and knowledge
  time;
- correction represented as evidence-backed supersession;
- local authority preservation under composition;
- treatment authorship lineage, evidence coverage, uncertainty, review latency,
  reuse scope, and unresolved gaps;
- treatment fidelity, declared loss, and adjoint interpretation;
- immutable object-cut supersession;
- missing and conflicting source claims remaining explicit;
- out-of-order domain events, observations, admissions, knowledge intervals,
  and corrections without temporal-axis collapse;
- many-to-many intent, execution, trade, and lifecycle lineage;
- expected, executed, and actual gain basis compatibility;
- attribution residual visibility;
- CDM version and mapping-fidelity enforcement;
- projection reproducibility from pinned evidence;
- rejection of unknown semantic roles and unqualified dates;
- negative proof that tenant code cannot append ABG events or choose
  continuation, closure, or re-entry.

Broad CDM lifecycle qualification belongs to the separate product slice in
`PRODUCT.md`. It requires its own fixtures and cannot be inferred from this
commodity proof.

## Proposed Layout

```text
build_tenants/odd_trading_eval/typescript/
  README.md
  design/
    README.md
    adrs/
  package.json
  src/
    domain/
    algebra/
    cdm/
    gtl/
    adapters/
    projections/
  tests/
    fixtures/
    semantic/
    replay/
    authority/
```

Only this README is currently materialized. The remaining structure is a
candidate design and must not be scaffolded until tenant activation.
