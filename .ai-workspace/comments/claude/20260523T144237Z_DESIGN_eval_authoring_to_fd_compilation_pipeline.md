# Design Proposal: Two-Part Eval Pipeline — Authoring → F_D Compilation

**Date**: 2026-05-23
**Author**: claude
**Status**: Design commentary / proposed `design_reframe`. Not ratified. Resolves three open decisions raised in the session that produced [[20260519T230000Z_RESEARCH_pnl_explain_forensic_attribution]] and the cascade architecture in `specification/BOOTSTRAP.md` §10.
**Scope**: Proposes the lawful re-entry shape for capturing business rules over the trade model as authored `Eval` artifacts, then compiling them through a customized odd_sdlc traversal into deterministic Python big-data F_D runtime code.

---

## 1. Position

The Eval pipeline is two-part:

| Part | Surface | Output |
|---|---|---|
| **1. Authoring** | `specification/evals/` — typed, governed Eval specs written by domain quants | Versioned Eval declarations under TICKET_METHOD change admission |
| **2. Compilation** | Customized odd_sdlc traversal acting as a deterministic compiler | Executable Python F_D modules that emit `BreachCandidate` / `ResolvedBreach` against admitted `AttributionRow` data |

The Eval is the *rule definition*. It is not a runtime Markov object. The runtime Markov objects already declared in `BOOTSTRAP.md` §6.2 — `AttributionRow`, `BreachCandidate`, `ResolvedBreach`, `HypothesisRanking`, `RatifiedExplanation` — stay intact. The Eval sits one level up: it is the specification artifact that, when compiled, becomes the F_D code at S1 and S2 that produces those runtime objects.

This resolves the vocabulary tension raised in prior discussion: Eval and BreachCandidate are not competing names for the same thing. Eval is constitutional (`WHAT must hold`), BreachCandidate is realization-emitted (`an instance where an Eval did not hold`).

## 2. Three Pre-Decided Items

This proposal locks the three decisions flagged in the prior turn:

| Decision | Resolution | Rationale |
|---|---|---|
| **GainSurface placement** | S3 hypothesis-space extension | Inferred from observed behavior, not source-feed data; sits on the F_P side, not in §9 typed domain layer or §10 RiskFactor union. Adds hypotheses `gain_surface_violation`, `dimension_neglect`, `surface_drift` to the S3 vocabulary per `coal_trading` Module |
| **"Eval" as runtime Markov object** | Rejected. Eval is a spec-layer artifact compiled to F_D code. Runtime types stay `BreachCandidate` etc. | Avoids duplicating cascade vocabulary; matches `ODD_METHOD.md` separation of WHAT (spec) from HOW (realization) |
| **Single-node-first bound (§8.4)** | Stands. Python is the compile-target language; the *runtime engine* on the first target stays single-node columnar (DuckDB-via-Python or Polars). Distributed targets (Spark, Dask) admitted as future compile targets only with declared per-asset-class volume evidence | Steel-thread pacing: prove the pipeline end-to-end on one stack before pluralizing compile targets |

The third decision is the load-bearing one for "big data processing." Single-node DuckDB-on-Parquet at 100 GB/day *is* big-data processing for coal-book scale. The compile-target-pluggable design preserves the option to retarget Spark later without rewriting Eval specs.

## 3. Part 1 — Eval Authoring Surface

### 3.1 Where Evals live

```text
specification/
├── INTENT.md
├── PRODUCT.md
├── GOALS.md
├── requirements/
│   └── REQ-F-OTE-*.md
└── evals/
    ├── EVAL-CATALOG.md                # index, status, lineage
    ├── thermal_atlantic/
    │   ├── EVAL-COAL-001-daily-desk-threshold/
    │   │   ├── frontmatter.yaml       # categorization tuple (§3.2)
    │   │   ├── predicate.md           # the rule body
    │   │   └── commentary_template.md # the prose projection (§3.5)
    │   ├── EVAL-COAL-002-leg-delta-drift/
    │   │   └── ...
    │   └── ...
    ├── metallurgical/
    ├── freight/
    └── cross_desk/
```

Evals are first-class spec surfaces parallel to `requirements/`. Each Eval lives in its own directory and carries three governed files: a frontmatter declaration of the categorization tuple, a predicate body, and a commentary template. The three-file shape is load-bearing — see §3.5 for the commentary commitment.

### 3.2 Eval declaration shape

Each Eval declares the categorization vectors as required frontmatter fields. The body declares the predicate.

```yaml
---
eval_id: EVAL-COAL-001
name: daily_desk_pnl_threshold
status: ratified                       # candidate | ratified | superseded
version: 1
effective_from: 2026-06-01
supersedes: null
owning_module: coal_trading

# Categorization vectors (required)
window:
  kind: day                            # day | rolling_n_days | week | month | quarter | ytd | custom
  alignment: business_day
scope:
  aggregate_span: desk                 # row | leg | trade | book | desk | portfolio | firm
  entities: [Desk, Trade, AttributionRow]
product_category: coal_thermal         # coal_thermal | coal_met | freight | cross_asset | ...
regime:
  deterministic: true                  # if true → F_D compile target
  probabilistic: false                 # if true → F_P compile target (S3 only)
reasoning_pattern: threshold           # threshold | delta | ratio | trend | correlation
                                       # | gain_surface_deviation | explanatory_manifold
gain_surface_link:                     # optional; links eval to a declared surface
  surface_ref: null

# Cascade binding
stage: S1                              # S1 | S2 (S3/S4 evals are different shape — see §3.4)
breach_kind: desk_pnl_threshold_exceeded
severity_calculator: linear_by_magnitude

# Commentary projection (required; see §3.5)
commentary_template: ./commentary_template.md
---

# Rule body (predicate)

Given:
  - AttributionRow grouped by (date, desk_id)
  - Threshold from threshold_registry where desk_id matches, effective at date

Assert:
  sum(attribution_row.total_pnl) over the group < threshold.limit

On violation, emit BreachCandidate with:
  - magnitude = sum(total_pnl) - threshold.limit
  - mechanical_evidence_refs = [attribution_row_ids in group]
  - threshold_ref = threshold.rule_id@version
```

### 3.3 The required fields are the categorization vectors

The seven vectors from prior discussion become required Eval frontmatter:

| Vector | Frontmatter field |
|---|---|
| Window size | `window.kind`, `window.alignment` |
| Deterministic | `regime.deterministic` |
| Probabilistic | `regime.probabilistic` |
| Entities | `scope.entities` |
| Aggregate span | `scope.aggregate_span` |
| Product category | `product_category` |
| Reasoning pattern | `reasoning_pattern` |

The eighth vector — gain-surface linkage — is optional and only populated when the Eval evaluates behavior against a declared GainSurface (see §6). A ninth required field, `commentary_template`, governs the prose projection of the eval's output and is the load-bearing companion to the predicate — see §3.5.

### 3.4 Stage binding

Each Eval declares which cascade stage it compiles into:

| Stage | Eval shape | Compile target |
|---|---|---|
| S1 | Universal/mechanical predicate over AttributionRow | F_D SQL or Polars expression + F_D commentary renderer over a deterministic-finding payload |
| S2 | Overlay-style residual decomposition | F_D as a `CandidateFamily<Overlay>` member + F_D commentary renderer over a deterministic-resolution payload |
| S3 | Hypothesis-synthesis configuration (not a predicate; declares hypothesis priors / synthesis_method binding) | F_P configuration + **F_D commentary renderer** over an F_P-produced HypothesisRanking payload (renders ranked hypotheses with confidence intervals; never asserts closure) |
| S4 | Reviewer interface contract + remediation taxonomy | F_H UI configuration; commentary at S4 appends the human ratification narrative to the rendered S3 commentary |

S1/S2 are the F_D-heavy stages and the focus of the compilation pipeline. S3/S4 Evals declare *configuration*, not rules — they don't compile to executable predicates. They still carry a commentary template; the renderer formats whatever structured output the stage produces (hypothesis ranking at S3, ratification record at S4).

### 3.5 Two-part report commitment (inherent)

Every Eval emits a two-part report when it fires. Part (A) is the structured eval payload (`BreachCandidate` / `ResolvedBreach` / `HypothesisRanking` / `RatifiedExplanation`) admitted through ABG. Part (B) is the rendered commentary — a read-model projection over (A), produced by the `commentary_template` declared in §3.2.

The architectural commitment, settled in [[20260523T151425Z_DESIGN_eval_suite_commodities_and_coal]] §2.1: **the eval's structured payload inherently contains the reason for the breach**. Commentary is not an F_P inference step over an opaque finding; it is the mechanical projection of structured evidence into prose for human consumption. This preserves the F_D-throughout-runtime invariant from `BOOTSTRAP.md` §10.

**Commentary regime invariant** (named explicitly per [[20260523T154035Z_REVIEW_design_state_and_scaling_architecture]] §4.5.1): the *renderer* is F_D at every stage. The *payload being rendered* may be deterministic (S1/S2 BreachCandidate/ResolvedBreach) or probabilistic (S3 HypothesisRanking) or human-extended (S4 RatifiedExplanation appended on top of the rendered S3 base). The template's *language* matches the regime of the payload (closure-asserting for F_D, candidate-evidence framing for F_P, human-anchored for F_H), but the template's *execution* is always F_D. There is no runtime path where commentary requires F_P inference per row. The load-bearing reason: per-row F_P at attribution-row scale (100k-10M/day) is economically and operationally impossible.

Four invariants the compiler enforces on `commentary_template`:

- **Replay-safe** — same structured payload → byte-identical commentary.
- **No phantom facts** — every template field reference must resolve against the structured payload type at compile time (§4.3 step VALIDATE). A template that references a missing field fails closed at compile time.
- **Regime-faithful** — the commentary inherits the regime of its source eval; an F_P commentary template that renders as if its claims were closure fails compile.
- **Versioned with the eval** — a commentary template change is a version bump on the Eval. Historical replay binds to the template version active at the replay date (§7).

Humans never write commentary at S1/S2/S3. The system writes it. Humans enter commentary only at S4 ratification, as appended narrative on top of the rendered F_P commentary. This inversion — system writes, humans ratify — is the load-bearing property that makes a 50-100k/day coal book and a millions/day high-volume desk both tractable for human review.

## 4. Part 2 — Compilation as Customized SDLC Traversal

### 4.1 SDLC-as-compiler positioning

odd_sdlc carries the construction graph (per `BOOTSTRAP.md` §3). The customization for this project: extend the substrate's traversal mechanics with a code-generation stage that treats Eval specs as source code and emits Python F_D modules as compiled output.

| Compiler concept | Eval-pipeline analog |
|---|---|
| Source code | Eval spec directory under `specification/evals/`: `frontmatter.yaml` + `predicate.md` + `commentary_template.md` |
| Lexer / parser | Frontmatter schema validator + predicate parser + commentary template parser |
| AST | Typed Eval IR (intermediate representation) covering both predicate and commentary projection |
| Type checking | Cross-reference against domain schema (§9), risk factor taxonomy (§10), threshold registry; commentary-template purity check against the structured payload type |
| Optimizer | Eval grouping by (window, scope, product) — co-compile evals that scan the same data |
| Code generator | Per-target module emitter: predicate function + commentary renderer function |
| Compiled binary | A deployable F_D/F_P module exposing both `evaluate(...)` and `render_commentary(...)` + manifest |
| Linker | Cascade routing wiring in `Job<post_trade_attribution_run>` |

Compilation is itself F_D: same Eval spec + same target = byte-identical Python output. Recompilation is an admitted event in the rule-store ledger; the `effective_from` field on the Eval determines which compiled artifact a replay at a given date binds to.

### 4.2 Compile targets

The compiler is target-pluggable. Each target is a separate code-generation backend behind a common Eval IR.

| Target | Engine | Scale band | Status |
|---|---|---|---|
| `duckdb-sql` | DuckDB embedded in Node, predicates emitted as SQL | up to ~100 GB/day | **Default for v0.1** — matches `BOOTSTRAP.md` App. D |
| `polars-python` | Polars DataFrame expressions in a Python F_D worker | up to ~100 GB/day | Secondary v0.1 target for predicates harder to express in SQL |
| `dask-python` | Dask distributed DataFrames | 100 GB – 1 TB/day | Admitted only with declared volume evidence per §8.4 |
| `spark-python` | PySpark on a cluster | 1 TB+/day | Admitted only at petabyte-scale evidence |

The Eval IR is the same across targets. Choosing a target is a build-tenant decision per asset-class Module, not an Eval-author decision. Authors write the rule once; the SDLC selects the target at compile time based on the Module's declared scale band.

This satisfies "big data processing in Python F_D" without breaking the single-node-first bound: the *language* is Python (for `polars-python` and beyond), the *engine* starts single-node and scales out only with evidence.

### 4.3 Traversal stages

The customized odd_sdlc traversal that compiles Evals:

```text
1. INTAKE       — accept new/updated Eval spec directory; assign change_class (per TICKET_METHOD)
2. VALIDATE     — frontmatter schema check; entity refs resolve against §9 domain types;
                  risk factor refs resolve against §10 taxonomy; commentary-template
                  purity check — every field referenced in the template must exist on
                  the eval's declared structured payload type (fail closed on miss)
3. LOWER        — parse predicate body and commentary template to typed IR;
                  cross-check window/scope coherence
4. OPTIMIZE     — group co-scannable Evals; eliminate dead branches; constant-fold
5. EMIT         — generate target module: predicate function + commentary renderer
                  function against the Module's declared compile target
6. ATTEST       — emit compilation provenance (eval_id, version, target, predicate_hash,
                  commentary_template_hash, output_hash, compiler_version) as an admitted event
7. BIND         — register compiled module in the cascade Job's stage routing
```

Steps 1–6 are F_D. Step 7 is F_D wiring in the cascade. No probabilistic step in the compilation pipeline itself. Both the predicate hash and the commentary-template hash are admitted so a replay at a historical date binds to *both* the predicate logic and the commentary prose active then.

### 4.4 Output shape

Each compiled Eval produces:

```text
build_tenants/typescript/compiled_evals/<target>/<module_name>/
├── <eval_id>_v<version>.py                  # generated predicate function
├── <eval_id>_v<version>_commentary.py       # generated commentary renderer
├── <eval_id>_v<version>.manifest.json       # predicate_hash, commentary_template_hash,
│                                            # output_hash, target, deps
└── __init__.py                              # registers eval in the Module's S1/S2 binding
```

The compiled module exposes a uniform interface — both functions are pure over their inputs:

```python
def evaluate(
    attribution_rows: pl.LazyFrame,        # input slice
    snapshot: SnapshotContext,             # snapshot_id, rule_store, threshold_registry
) -> BreachCandidateFrame:                 # zero or more BreachCandidates as a typed frame
    ...

def render_commentary(
    payload: BreachCandidate,              # one structured eval output
) -> str:                                  # rendered prose, replay-stable
    ...                                    # pure function: no disk, network, model,
                                           # or state outside `payload`
```

Node side calls these via the integration pattern from `BOOTSTRAP.md` App. D (DuckDB-in-process for SQL target; child_process or subprocess pool for Python targets). The Eval author never sees the Node ↔ Python boundary — it lives entirely in the generated code. The `render_commentary` function fires whenever ABG admits the corresponding `BreachCandidate` / `ResolvedBreach` / `HypothesisRanking` event; its output is admitted as part of the event payload and indexed by the same `(eval_id, version, snapshot_id)` tuple as the predicate's structured output.

## 5. Reconciliation with BOOTSTRAP Surfaces

This proposal touches three BOOTSTRAP surfaces and clarifies (does not contradict) each:

| BOOTSTRAP section | Clarification |
|---|---|
| §6.2 cascade Markov objects | Eval is not added to the table. AttributionRow remains the cascade input; BreachCandidate remains S1's output. Eval sits above the cascade as the spec-layer rule definition |
| §8 build tenant | Compile targets `duckdb-sql` (App. D-native) and `polars-python` (new, single-node F_D in Python) belong to the `typescript` tenant. Adding `dask-python` / `spark-python` requires a tenant-level scale declaration |
| §10 GTL module | `s1_broad_breach_detection` and `s2_model_overlay_detection` graph functions remain. Their *bodies* are the compiled-Eval outputs. The graph-function declarations don't change; the realization underneath them does |
| §12 requirement family skeleton | A new family is implied: `REQ-F-OTE-090..n Eval authoring & compilation contract`. To be carried into the first ratification pass |

This is a `design_reframe` per `SPEC_METHOD.md` Lawful Re-Entry: realization structure changes (Evals now author/compile), requirements stay stable in INTENT/PRODUCT terms. The added requirement family covers the contract surface, not new intent.

## 6. GainSurface as S3 Hypothesis Extension (Carried Forward)

GainSurface stays out of §9 (typed domain layer) and §10 (RiskFactor union). It lives at S3 as an inferred artifact.

Concretely:

```text
Module: coal_trading (extends attribution_base)
  S3 hypothesis space ← +{
    gain_surface_violation,        # observed behavior outside declared surface bounds
    dimension_neglect,             # one or more declared dimensions ignored
    surface_drift,                 # implied surface diverges from mandate
    gain_surface_pareto_inferior   # within bounds but dominated by alternative path
  }
```

GainSurface inference is an S3 synthesis method, declared per Module. The synthesis output carries:

- `inferred_surface` — the multi-dimensional surface fit to observed trade behavior
- `declared_surface_ref` — link to mandate or strategy declaration (Postgres rule store)
- `deviation_vector` — per-dimension drift between declared and inferred
- `confidence_interval` — per-dimension uncertainty

This stays consistent with `BOOTSTRAP.md` §10's rule: F_P output is candidate evidence, never closure authority. Ratification of a gain-surface violation as a recurring class still happens at S4 and feeds the rule store as a new S2 overlay or threshold.

## 7. Eval Lifecycle and Versioning

Evals are versioned. The rule store (Postgres, per `BOOTSTRAP.md` §6.2) carries the mapping from `(date, eval_id) → effective version`. Replay at any historical date binds to the version active then.

Lifecycle states for an Eval:

| State | Meaning | Cascade visibility |
|---|---|---|
| `candidate` | Authored, not yet ratified | Not bound to runtime |
| `ratified` | Approved through odd_sdlc traversal; compiled and bound | Active in cascade routing |
| `superseded` | Replaced by a later version | Bound for replay at dates before successor's `effective_from` |
| `withdrawn` | No longer used; no successor | Bound for replay at dates within its active window only |

Eval transitions are admitted as events in the same log that carries `RatifiedExplanation` events. The feedback loop from `BOOTSTRAP.md` §10 — S4 ratification → new rule → next run's S2 catches the class — is realized as a new ratified Eval being authored, compiled, and bound.

## 8. Open Questions for the Next Pass

Items deliberately not decided here; flagged for the ratification traversal that converts BOOTSTRAP into INTENT/PRODUCT/GOALS:

1. **Eval predicate language** — three lawful candidates:
   (a) YAML-with-schema body fields (declarative, simple, limited expressiveness),
   (b) Custom textual DSL parsed to IR (most flexible, requires grammar),
   (c) TypeScript types + functions as the spec (executable, tightly coupled to one tenant).
   Trade-off: portability vs author ergonomics vs tooling cost. No decision implied by this proposal.

2. **Default compile target for v0.1 Module `coal_trading`** — `duckdb-sql` (matches App. D's "S1 and S2 are pure SQL" line) or `polars-python` (matches user's Python F_D preference). My read: start `duckdb-sql` for S1 (universal threshold/integrity is naturally SQL), and admit `polars-python` for S2 overlays that need richer computation (Shapley, hedge-effectiveness decomposition).

3. **Eval co-compilation strategy** — whether the optimizer groups Evals sharing `(window, scope)` into a single scan pass, or compiles each independently. Co-compilation matters at scale; independence matters for debugging. Decide at first volume measurement.

4. **Eval ↔ rule_store schema** — `BOOTSTRAP.md` §6.2 defines rule_store as carrying overlay rules, thresholds, mapping rules. Does the Eval registry sit *inside* rule_store (one schema, multiple kinds) or *alongside* it (separate but cross-referenced)? Coupling vs separation of concerns.

5. **Compiler-as-traversal vs compiler-as-tool** — whether Eval compilation runs as a node in the odd_sdlc traversal graph (admitted as graph events) or as a separate offline tool whose output is admitted. The former is more constitutional; the latter is simpler to build. Steel-thread pacing argues for offline-tool first, traversal-node later.

6. **GainSurface declaration surface** — where mandates / declared surfaces live. Candidates: Postgres operational table, Eval-style frontmatter, or a third surface under `specification/gain_surfaces/`. Defer until first GainSurface use case is concrete.

## 9. Recommended Sequence

A staged plan that respects steel-thread pacing and the single-node-first bound:

1. Ratify BOOTSTRAP into INTENT.md, PRODUCT.md, GOALS.md, requirements (per `BOOTSTRAP.md` §15 closure law). Add `REQ-F-OTE-090..n` family for Eval authoring & compilation.
2. Pick predicate language (open question 1). Author 3–5 seed Evals against the coal data model: one S1 threshold, one S1 reconciliation invariant, one S2 quality-differential overlay, one S2 freight-basis overlay.
3. Build the minimum compiler: validator + IR + `duckdb-sql` emitter. No optimizer yet. Run the seed Evals end-to-end against one synthetic coal trade per `BOOTSTRAP.md` §15 closure condition.
4. Add `polars-python` target for the S2 overlays that need richer computation. Demonstrate one overlay compiled to both targets producing equivalent BreachCandidates.
5. Lock the Eval ↔ rule_store coupling decision (open question 4).
6. Introduce the optimizer + co-compilation only after a real volume measurement.
7. Defer `dask-python` / `spark-python` until a declared per-asset-class volume measurement crosses §8.4's threshold.

This sequence keeps the cascade closure law from `BOOTSTRAP.md` §15 reachable: the first end-to-end attribution slice can run on 3–5 Evals compiled to `duckdb-sql`, no Python target required, no distributed target required.

---

## 10. Status and Next Action

This proposal is commentary. It is not ratified spec. It proposes:

- Eval as a spec-layer artifact, parallel to `requirements/`
- A customized odd_sdlc traversal that compiles Evals to F_D Python (or SQL) modules
- Three pre-decisions: GainSurface placement (S3), Eval-not-runtime-object (rejected as Markov object), single-node-first bound (held)
- Compile-target-pluggable architecture with `duckdb-sql` and `polars-python` as v0.1 targets

The lawful next action is one of:

- (a) Open a ticket under `TICKET_METHOD.md` carrying this as a `design_reframe` for ratification through the first odd_sdlc traversal pass; or
- (b) Capture this in the first ratification traversal directly as input alongside BOOTSTRAP §10 and §11.

This document is provenance-only once that traversal admits or rejects the proposal.
