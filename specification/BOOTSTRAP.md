# odd_trading_eval — SPEC_METHOD Bootstrap

**Status**: Superseded seed. Retained as provenance for the product state that preceded the 2026-07-12 intent reprice.

**Authority**: `INTENT.md` and `PRODUCT.md` define the current constitutional line. Claims in this document are source material until ratified through that line and the live requirement surface.

**Revision**: 2026-05-24. Consolidates the design work captured in `.ai-workspace/comments/claude/` (Eval pipeline ADR, Eval catalog, scaling architecture review, two-part report and commentary regime commitments). Supersedes the earlier seed-only revision.

**Method inheritance**: `SPEC_METHOD.md`, `ODD_METHOD.md`, `TICKET_METHOD.md`, `DESIGN_MODULE_METHOD.md` (master at `https://github.com/foolishimp/specification_methodology`; installed at `.abiogenesis/docs/standards/` after odd_sdlc install).

**Substrate (installed product)**: `odd_sdlc` — the released ODD-shaped SDLC product carrying the GTL/ABG construction graph, typed stage carriers, cascade attribution patterns, edge-accounting register, complexity-admitted traversal selection, and the F_D/F_P/F_H regime split.

**Research source**: `.ai-workspace/comments/claude/20260519T230000Z_RESEARCH_pnl_explain_forensic_attribution.md` — the seed knowledge surface (landscape, coal data model, cascade architecture, generalization pattern, implementation stack). Commentary, not law.

**Design commentary stack**: four `.ai-workspace/comments/claude/` documents converge into this revision —
- `20260523T144237Z_DESIGN_eval_authoring_to_fd_compilation_pipeline.md` (Eval pipeline ADR)
- `20260523T151425Z_DESIGN_eval_suite_commodities_and_coal.md` (Eval catalog + two-part report commitment)
- `20260523T154035Z_REVIEW_design_state_and_scaling_architecture.md` (scaling architecture, gap analysis, commentary regime invariant)
- (the seed research doc above)

---

## 1. Position

`odd_trading_eval` is an ODD product built using `odd_sdlc` as the installed substrate. The product realizes post-trade P&L attribution and forensic breach analysis for multi-asset derivatives. Coal is the first proving instance; the architecture is asset-class-agnostic by construction.

Per `ODD_METHOD.md` §6, the installed product (odd_sdlc) provides the graph-native method, traversal-control substrate, runtime, command surfaces, and scaffold templates. This project owns its constitutional surfaces (this bootstrap, the derived INTENT/PRODUCT/GOALS/requirements, the `specification/evals/` Eval catalog) and its realization root (`build_tenants/typescript/`). The substrate is not co-equal authority; it carries `HOW` for construction. The constitutional `WHAT` belongs to this project.

The product is built as: typed domain assets + published GTL graph functions over the four-stage cascade + ABG runtime + Parquet/Postgres projection surface + replay-derived proof surface. Per BOOTSTRAP §10 and `ODD_METHOD.md` §11.

## 2. Recursive Product Taxonomy

| Role | Identity | Mutability |
|---|---|---|
| **Substrate** | `odd_sdlc` released product | Immutable for this project's first release cycle |
| **Source Project** | `odd_trading_eval` workspace (this repo) | Mutable; builds toward `odd_trading_eval` v0.1 release cut |
| **Domain** | Post-trade P&L attribution + forensic breach analysis for multi-asset derivatives | Constitutional `WHAT`, owned by this project |
| **Build Tenant** | `typescript` | First realization; canonical root `build_tenants/typescript/` |
| **First Asset-Class Module** | `coal_trading` | Proves the architecture; generalizes per §15 |
| **Shared Base Module** | `attribution_base` | Cross-asset Eval catalog and shared cascade functions |

The substrate/source/product/install boundary is strict per `SPEC_METHOD.md`. odd_sdlc as substrate is not a rival constitution for this project. Its method governance applies; its product-specific surfaces do not.

## 3. Method Inheritance and Authority Boundary

| Layer | Source | Role for odd_trading_eval |
|---|---|---|
| Constitutional method | `SPEC_METHOD.md` | Goals → Intent → Product → Requirements → Design → Code chain governs all change |
| Graph-native product law | `ODD_METHOD.md` | Product is built as typed assets + GTL graph functions + ABG runtime + projection surfaces |
| Ticket and sprint mechanics | `TICKET_METHOD.md` | Every substantive change goes through intake triage with explicit `change_class`. Shadow→ratified Eval promotion is admitted through this surface |
| Design decomposition | `DESIGN_MODULE_METHOD.md` | Design surfaces decompose into governed modules with explicit derivation. Eval pipeline compiler stages, threshold registry, T&R mapping all decompose under this law |
| Construction substrate | `odd_sdlc` v3.7.1-rc.4+ released line | Provides typed F_P stage carriers, cascade attribution patterns, edge accounting, complexity-admitted traversal selection, F_D/F_P/F_H regime split, evaluate.C/F_P semantic_judgment_rule pattern |
| Domain semantic law | This project's specification | Owns trade/desk/counterparty/risk-factor/attribution semantics; owns the Eval catalog; owns the commentary commitment |

The composition rule from `ODD_METHOD.md` §3 applies: SPEC_METHOD and ODD_METHOD tell *how* to build the product as a spec-driven graph-native ODD product; odd_sdlc as substrate provides the *construction graph*; this project's local INTENT/PRODUCT/requirements/evals tell *what* semantic law the product honors.

The substrate is the inheritance layer for **construction**, not for **domain meaning**. Domain meaning is owned exclusively here. The commentary regime invariant (§13.3) is the project-local refinement of the substrate's F_D/F_P/F_H discipline — the substrate permits F_P generation of artifacts at any scale; this project forbids per-row F_P generation because attribution-row volume forbids it.

## 4. Project Identity

- **Project slug**: `odd_trading_eval`
- **Primary build tenant**: `typescript`
- **Active tenant runtime**: Node 20+ with embedded DuckDB, Postgres for operational state, Parquet for snapshot store
- **Repository location**: `/Users/jim/src/apps/odd_trading_eval/`
- **Installed runtime root**: `.abiogenesis/` — single folder holding all installed substrate after the first odd_sdlc install pass

## 5. INTENT (provisional, to be ratified into `INTENT.md`)

### 5.1 Purpose

Build a post-trade P&L attribution and forensic breach analysis system for multi-asset derivatives. Operate over data received from an existing trade and risk system. Canonicalize that data into a typed domain model. Compute multi-bucket attribution decomposition. Detect breaches through a governed catalog of authored Evals that compile to deterministic runtime predicates. Explain breaches through a deterministic F_D-first cascade that escalates to bounded F_P synthesis only when F_D cannot close, and to F_H ratification only when F_P cannot decide. Accumulate institutional knowledge as ratified Evals through a shadow→ratified promotion lifecycle.

### 5.2 Outcomes

- Canonical model surface for trades, desks, counterparties, and risk factors that maps cleanly from upstream T&R system feeds and supports cross-asset attribution
- Daily and historical attribution pipeline computing actual P&L (APL), hypothetical P&L (HPL), and risk-theoretical P&L (RTPL) with full attribution-bucket decomposition
- Governed Eval catalog (`specification/evals/<module>/`) that authors business rules as spec-layer artifacts; the SDLC traversal compiles them into deterministic runtime predicates + F_D commentary renderers
- Four-stage cascade (S1 F_D broad → S2 F_D model overlay → S3 F_P probability synthesis → S4 F_H human ratification) running over admitted attribution rows
- Two-part report at every Eval firing: (Part A) structured payload admitted through ABG; (Part B) F_D-templated commentary rendered from Part A
- Replay-visible event-sourced lifecycle so any historical breach is recomputable from admitted events + market snapshots + rule store + Eval version at any timestamp
- Eventual-consistency rule-store feedback loop with explicit shadow→ratified Eval promotion: every S4 ratification opens a path to admit a new ratified Eval that ratchets one class of breaches from F_P/F_H back into F_D for future runs
- Coal as the proving asset-class instance; the architecture generalizes to oil/gas, power, rates, FX, equity derivatives, and credit through Module composition

### 5.3 Constraints

- **F_D throughout the runtime path** — every per-row transformation in the cascade is deterministic. F_P enters only at S3 synthesis; F_H only at S4 ratification. No agent reasoning inside per-row attribution.
- **F_D commentary renderer everywhere** — the commentary regime invariant (§13.3) forbids any per-row F_P inference for commentary. Renderers are F_D templates at every stage; only the *payload being rendered* and the *template's language* vary by regime.
- **Domain data is a separate typed schema layer**, external to GTL. GTL handles workflow, lifecycle, governance; the domain records (Trade, Desk, Counterparty, RiskFactor, AttributionRow) live in a typed schema language referenced from GTL nodes.
- **No worker prompt may carry domain content that should live in admitted records**. Prompt rendering is consumer-side; the typed carrier is authority.
- **Snapshot-pinned reproducibility** — every record carries a snapshot_id; every rule has a versioned effective date; every Eval is versioned; replay must produce identical output for the same (snapshot, rule_version, eval_version) tuple.
- **Single-node first per Module** — each Module declares a `volume_band` (`low` / `mid` / `high` / `extreme`); compile target follows the band. Project-level default is `low` (coal). Distributed compile targets admitted only with declared per-Module volume evidence per §16.4 settlement item.
- **No FRTB-grade regulatory PLA test obligation in v0.1**. The architecture supports it; the v0.1 closure does not require it.

## 6. PRODUCT Definition (provisional, to be ratified into `PRODUCT.md`)

### 6.1 Product position

`odd_trading_eval` is a post-trade analytical system with two operational tiers. Tier 1 is the production-critical runtime cascade (SLA-bound, daily batch). Tier 2 is the analyst workbench (notebook-driven, exploratory, hosts shadow Eval execution). The two tiers share the immutable admitted-event log and the Parquet snapshot store; they share nothing else (§16 scaling architecture).

It is not a front-office trade booking system, not a real-time risk system, and not a pricing engine. It consumes admitted state from upstream systems (trades, market data, greeks) and produces structured attribution rows, breaches, hypothesis rankings, ratified explanations, and rendered commentary as durable analytical truth.

### 6.2 Key terms

| Term | Definition |
|---|---|
| **Trade** | Canonical typed record carrying a deal between counterparties. Discriminated union over `PhysicalCargo` (coal first), `PaperTrade`, future product subtypes |
| **Desk** | Organizational unit owning trades; hierarchy with parent_desk_ref; carries limits, reporting currency, attribution scope |
| **Counterparty** | Legal entity in a trade; carries credit, master agreement, parent group ref |
| **RiskFactor** | Typed market quantity that drives P&L. Discriminated union: coal benchmark curve, quality differential, location basis, freight route, FX rate, interest rate, carbon cost, plus generic cross-asset factors |
| **AttributionRow** | Wide-DataFrame record: one row per (date × trade × leg × factor) with attribution buckets as columns |
| **Eval** | A spec-layer business-rule artifact authored under `specification/evals/<module>/<eval_id>/`. Compiled by the SDLC traversal into a runtime predicate function + F_D commentary renderer. Not a runtime Markov object — see §11 |
| **BreachCandidate** | S1 output: typed breach detected by a compiled F_D Eval predicate |
| **ResolvedBreach** | S2 output: BreachCandidate with applied overlays and reduced residual |
| **HypothesisRanking** | S3 output: F_P-produced probabilistic ranking of breach causes with evidence refs and confidence intervals |
| **RatifiedExplanation** | S4 output: human-confirmed cause and remediation action |
| **Commentary** | F_D-templated prose rendered from one of the above structured payloads. Part (B) of the two-part report. Replay-stable. See §13 |
| **GainSurface** | A multi-dimensional surface inferred from observed trade behavior, compared against a declared desk mandate. Lives at S3 hypothesis space only. See §11 pre-decision and §10.4 |
| **Shadow Eval** | A candidate-status Eval that runs against admitted data without binding to the runtime cascade; produces tagged shadow events for promotion-path evaluation. See §14 |
| **Rule store** | Versioned, transactional store (Postgres) of overlay rules, thresholds, mapping rules, declared mandates, and ratified Eval bindings; each entry has an effective date so historical replay uses the version active at that date |
| **Snapshot** | Pinned (date, snapshot_id) tuple identifying market data, positions, greeks, and rule-store version for replay |

### 6.3 Boundaries (in / out of scope for v0.1)

**In scope:**
- Coal as the proving asset class (thermal + metallurgical)
- Canonical trade model with PhysicalCargo and PaperTrade subtypes
- Risk factor taxonomy covering coal benchmark curves, quality differentials, location basis, freight routes, FX, interest rate, carbon
- Eval authoring surface (`specification/evals/<module>/<eval_id>/` per-Eval directory)
- SDLC compiler with the `duckdb-sql` backend; the `polars-python` backend admitted only for Evals whose predicate cannot be expressed in SQL (~5-10 of the v0.1 catalog)
- S1 universal threshold/integrity checks; S2 coal-specific overlays (quality, freight, demurrage, pricing-period, hedge effectiveness) + generic overlays (convexity, cross-greek, lifecycle, Shapley)
- S3 Bayesian-network synthesis (ML deferred); S4 notebook-embedded reviewer UI capturing ratification
- Two-part report at every firing with F_D commentary rendering
- Shadow Eval execution path in Tier 2 with promotion contract through TICKET_METHOD admission
- Forensic notebook surface (Python + DuckDB + Plotly) reading the same Parquet snapshot store
- Module composition pattern documented; a second asset class scaffolded as validation per §15
- Trade-lifecycle materialised view (`trade_lifecycle/trade_id=<id>/*.parquet`) for the bounded set of `trade_lifetime`-window Evals

**Out of scope for v0.1:**
- Multi-asset cross-correlation pricing (greeks consumed, not computed)
- Real-time intraday breach detection (daily/period batch only; §16.4 settlement item 1)
- Distributed compile targets (`dask-python`, `spark-python`) — admitted only with declared per-Module volume evidence
- FRTB regulatory PLA test certification
- Concurrent F_P worker dispatch (deferred to ABG concurrent execution support per odd_sdlc T-173)
- ML-driven S3 classifier (Bayesian net first; ML in a later release)
- Cross-firing F_P synthesis on the per-row path (lives exclusively in Tier 2 analyst-initiated queries; never bound to the cascade)

## 7. GOALS (current work wave, to be ratified into `GOALS.md`)

The v0.1 work wave consists of a ten-step dependency-ordered sequence per `[[20260523T154035Z_REVIEW_design_state_and_scaling_architecture]]` §6. No time estimates per project convention; each step gates the next.

| Step | Outcome | Closes |
|---|---|---|
| **1** | Ratify INTENT.md, PRODUCT.md, GOALS.md from §§5-7; add `REQ-F-OTE-090..n` family for Eval authoring & compilation; answer the seven BOOTSTRAP §24 open questions to the v0.1 minimum | constitutional surface alive |
| **2** | Author threshold registry spec (Postgres schema, authoring contract, versioning model). DESIGN_MODULE_METHOD-shaped design surface | Gap A; foundation for all 35 S1 threshold Evals |
| **3** | Author upstream T&R mapping contract (mapping language, F_D/F_P split at boundary, reconciliation contract, snapshot semantics across systems) | Gap B; entire data ingress |
| **4** | Publish the `post_trade_attribution` GTL module (the four cascade graph functions, CandidateFamily declarations, Job binding); specify the Eval IR | Gaps G and I; compiler has a target |
| **5** | Author and ratify the first five Evals (`EVAL-BASE-{003,004,005,006,007}`); build minimum compiler (validator + IR + `duckdb-sql` emitter); run against one synthetic coal trade per §21 closure condition | first end-to-end attribution slice |
| **6** | Implement Module manifest, notebook-embedded reviewer UI, S3 inference budget mechanism, run-boundary EOD batch | Gaps D, E, J; settlement items 1, 2, 4, 5 |
| **7** | Author and ratify `EVAL-COAL-101` (quality differential); demonstrate S2 closure on a synthetic breach with rendered commentary | S2 overlay path proven on coal-specific content |
| **8** | Implement shadow-Eval execution and the shadow→ratified promotion contract; demonstrate a full promotion lifecycle on synthetic data | Gap F; the institutional-knowledge accumulator operational. **Architecturally most important step after Step 1** |
| **9** | Construct a synthetic unexplained breach; demonstrate S3 Bayesian-net synthesis, S4 ratification through the notebook UI, RatifiedExplanation admitted, next-run S2 closure via the Step 8 promotion path | steel-thread loop demonstrated end-to-end |
| **10** | Implement `trade_lifecycle/` event-sourced materialised view; specify GainSurface declaration surface (recommend: Postgres `desk_mandate` table, structured form = constraint set + weighted scalarization + per-dimension confidence) | Gaps C and H; v0.1 §21 closure conditions satisfied |

The work wave is bounded by this sequence. Catalog enumeration of additional Evals beyond the first end-to-end slice belongs to subsequent v0.x work-waves under future GOALS.md versions, not v0.1.

## 8. Build Tenant Declaration

### 8.1 Tenant identity

- **Tenant name**: `typescript`
- **Realization root**: `build_tenants/typescript/`
- **Language**: TypeScript on Node 20+ (orchestration, GTL/ABG runtime, governance); Python on 3.11+ (F_D heavy-lifting in `polars-python` compile target, analyst notebooks in Tier 2)
- **Operational footprint**: Tier 1 — single Node process per Module + Postgres + filesystem/S3 Parquet store. Tier 2 — analyst-local Marimo/Jupyter notebooks reading the shared Parquet store via DuckDB

### 8.2 Tech stack

| Layer | Library / system | Role |
|---|---|---|
| Workflow / governance | GTL/ABG via `@odd-sdlc/typescript-tenant` | Workflow, cascade routing, event sourcing, replay, F_D/F_P/F_H regime split, evaluate.C/F_P semantic_judgment_rule pattern |
| Analytical engine (Tier 1 F_D path) | `@duckdb/node-api` | All S1/S2 SQL queries; Parquet reads/writes; cross-join to Postgres via `postgres_scanner` extension |
| Operational state | Postgres + `pg` or `postgres` client | Rule store, threshold registry, overlay registry, declared mandate registry, reviewer workflow, configuration, ratified Eval bindings, shadow-Eval promotion ledger |
| Snapshot store | Parquet (Zstd compression) on disk or S3, partitioned by date and snapshot_id | All durable analytical data; universal format readable by DuckDB, Python notebooks, future distributed engines |
| Trade-lifecycle view | Parquet partitioned by trade_id | Event-sourced materialised view for `trade_lifetime`-window Evals; rebuilt from admitted lifecycle events |
| Eval IR + compiler | TypeScript (in the typescript tenant) | Per-Eval directory → IR → target-specific code generator (`duckdb-sql` default, `polars-python` secondary) |
| Polars-python F_D backend | `polars` (Python) | Compile target for Evals whose predicate is hard to express in SQL (Shapley, hedge effectiveness, demurrage lifetime, storage weathering) |
| Cross-language interop | Apache Arrow IPC format | Zero-copy data exchange with analyst Python notebooks |
| Parallelism (intra-process) | `worker_threads` (Node), Python subprocess pool | S2 overlay fan-out, per-trade greek bumps, polars-python subprocess dispatch |
| S3 synthesis (v0.1) | Bayesian network library (in-process Python) | Per-Module-declared synthesis_method; ML-classifier and agent-reasoning alternatives deferred |
| Analyst notebook surface (Tier 2) | Python + Marimo/Jupyter + DuckDB + Polars + Plotly + `shap` | External to the runtime tenant; reads the same Parquet store; hosts shadow Eval execution |
| Schema language | TypeScript types (primary), Avro for upstream interop where required, Pydantic for Python notebook side | Domain data records; external to GTL nodes |

### 8.3 Storage layout

```text
Bucket / disk root:
├── trades/snapshot_id=<id>/*.parquet
├── lifecycle_events/date=<YYYY-MM-DD>/*.parquet
├── market_snapshots/date=<YYYY-MM-DD>/snapshot_id=<id>/*.parquet
├── greeks/date=<YYYY-MM-DD>/snapshot_id=<id>/*.parquet
├── attribution_rows/date=<YYYY-MM-DD>/snapshot_id=<id>/*.parquet
├── trade_lifecycle/trade_id=<id>/*.parquet              # event-sourced view per §12.4
├── breach_candidates/date=<YYYY-MM-DD>/*.parquet
├── resolved_breaches/date=<YYYY-MM-DD>/*.parquet
├── hypothesis_rankings/date=<YYYY-MM-DD>/*.parquet
├── ratified_explanations/date=<YYYY-MM-DD>/*.parquet
├── shadow_breach_candidates/date=<YYYY-MM-DD>/*.parquet # shadow Eval outputs per §14
├── shadow_resolved_breaches/date=<YYYY-MM-DD>/*.parquet
└── rendered_commentaries/date=<YYYY-MM-DD>/*.parquet    # F_D-templated Part (B) per §13

Postgres:
├── rule_store              (rule_id, version, effective_date, payload)
├── threshold_registry      (per-desk, per-product)
├── overlay_registry        (active overlays, version, effective_date)
├── desk_mandate_registry   (desk_id, version, gain_surface constraints + scalarization)
├── ratified_eval_bindings  (module, eval_id, version, effective_date, compile_target, predicate_hash, commentary_template_hash)
├── shadow_eval_promotion_ledger (candidate_eval_id, shadow_run_evidence, agreement_count, promotion_status)
├── reviewer_workflow       (breach_id, status, assigned_reviewer, sla)
├── ratified_explanation_mirror (operational view of latest ratifications)
└── config, users, roles, desks
```

### 8.4 Scale threshold rationale

Volume bands declared per Module. Project default `low` (coal sits here). See §16 scaling architecture for the full per-axis treatment.

| Per-Module daily attribution volume | Compile target | Tier 1 infrastructure |
|---|---|---|
| `low`: ≤ 100k rows/day | `duckdb-sql` default; `polars-python` for SQL-hostile predicates | Single 8-vCPU 32GB node + Postgres + filesystem/S3 Parquet |
| `mid`: 100k – 1M rows/day | Same; partitioned scans dominate | Single 32-vCPU 128GB node + NVMe Parquet cache |
| `high`: 1M – 10M rows/day | `polars-python` over partitioned Parquet, or `dask-python` | ClickHouse cluster or Polars-on-Dask |
| `extreme`: > 10M rows/day | `spark-python` or warehouse-native | Snowflake / BigQuery / Trino-on-Iceberg / Spark cluster |

Distributed compile targets (`dask-python`, `spark-python`) are admitted into a Module only with declared per-Module volume evidence. Coal v0.1 stays at `low` and proves the steel thread there.

## 9. Domain Asset Layer (external to GTL)

Per `ODD_METHOD.md` §11.1, typed assets must be explicit. Per the research source, GTL is workflow-shaped not record-shaped. The split:

| Concern | Where it lives |
|---|---|
| Trade record shape, units, validation | TypeScript types + JSON Schema validators |
| Discriminated union over product subtypes (PhysicalCargo, PaperTrade, IRS, Option, ...) | TypeScript discriminated union, exported as the `Trade.product` shape |
| Currency-tagged amounts, unit-tagged quantities | Domain value types |
| Cross-record FK integrity | DuckDB SQL constraints at admission; supplemented by application checks |
| Cross-language interop | Arrow / Parquet on disk |

GTL nodes reference these typed records by name. GTL does not parse or validate them; it carries the payload as opaque-to-graph data with declared input/output asset types per vector.

## 10. ODD Graph Layer (via odd_sdlc substrate)

Per `ODD_METHOD.md` §11.2 and §11.4, the constructive carrier is graph functions in a published GTL module.

### 10.1 Module publication

```text
Module: post_trade_attribution

CandidateFamily<Trade.product> = { PhysicalCargo, PaperTrade, ... }
CandidateFamily<Overlay> = {
  // generic, inherited from base
  ConvexityOverlay, CrossGreekOverlay, ShapleyOverlay,
  HedgeEffectivenessOverlay, LifecycleOverlay,
  // coal-specific extensions
  QualityDifferentialOverlay, FreightBasisOverlay,
  DemurrageOverlay, PricingPeriodOverlay,
  // ratified-via-shadow-promotion additions accumulate here over time
  ...
}

GraphFunction<s1_broad_breach_detection>          # F_D evaluator
GraphFunction<s2_model_overlay_detection>         # F_D evaluator
GraphFunction<s3_probability_synthesis>           # F_P evaluator
GraphFunction<s4_human_evaluation>                # F_H evaluator

Job<post_trade_attribution_run>
  route:
    AttributionRow → s1
    s1.BreachCandidate → s2
    s2.ResolvedBreach[unexplained ∨ over_threshold] → s3
    s3.HypothesisRanking[uncertain ∨ critical] → s4
    s4.RatifiedExplanation → admitted_event → rule_store
                                            → triggers candidate Eval authoring path (§14)

Role: pnl_control, model_risk_reviewer, desk_head, quality_specialist, freight_desk_head
```

### 10.2 Runtime cascade

The runtime path is **F_D end-to-end**. F_P enters only at S3. F_H only at S4. Every stage output is admitted as an event in the ABG event log; the attribution rows table is a projection over admitted truth.

The four-stage cascade compresses the operator-facing semantics:

| Stage | Regime | Input | Output | Compile target (v0.1, coal `low`) |
|---|---|---|---|---|
| S1 | F_D | AttributionRow | BreachCandidate | `duckdb-sql` |
| S2 | F_D | BreachCandidate | ResolvedBreach (resolved_by_overlay \| remains_unexplained) | `duckdb-sql` or `polars-python` |
| S3 | F_P | ResolvedBreach[unexplained ∨ over_threshold] | HypothesisRanking | `polars-python` + in-process Bayesian net |
| S4 | F_H | HypothesisRanking[escalated] | RatifiedExplanation | notebook-embedded reviewer UI |

Per the architecture feedback in odd_sdlc: F_P exists because F_D cannot handle ambiguity. The cascade resolves ambiguity by F_P+F_H ratifying a rule once; F_D handles the class deterministically forever after. The S4 → rule store feedback loop, mediated by the §14 shadow→ratified promotion path, is the eventual-consistency steel thread.

### 10.3 Per-row F_P forbidden

Every per-row artifact, including the commentary humans read, is F_D-produced. The only F_P invocation on the per-row path is S3 synthesis, which fires on roughly 0.1% of rows and is bounded by the per-Module declared inference budget (§16.4 settlement item 2). Cross-firing F_P synthesis (analyst-initiated questions like "is this a recurring pattern?") lives exclusively in Tier 2 and never binds to the runtime cascade.

### 10.4 GainSurface placement

GainSurface stays out of §9 (typed domain layer) and out of the RiskFactor union. It lives at S3 as an inferred artifact only.

```text
Module: coal_trading (extends attribution_base)
  S3 hypothesis space ← +{
    gain_surface_violation,
    dimension_neglect,
    surface_drift,
    gain_surface_pareto_inferior
  }
```

GainSurface inference is an S3 synthesis method, declared per Module. The synthesis output carries `inferred_surface`, `declared_surface_ref` (link to `desk_mandate_registry`), per-dimension `deviation_vector`, and per-dimension `confidence_interval`. Per BOOTSTRAP §10.2's regime split: F_P output is candidate evidence, never closure authority. Ratification of a gain-surface violation as a recurring class still happens at S4 and feeds the rule store as a new S2 overlay or threshold via the §14 promotion path.

## 11. Eval Authoring and Compilation Pipeline

The Eval is the load-bearing realization concept introduced by this revision. Settled in `[[20260523T144237Z_DESIGN_eval_authoring_to_fd_compilation_pipeline]]`; constitutionalized here.

### 11.1 Position

An **Eval** is a spec-layer business-rule artifact. It is not a runtime Markov object. The runtime Markov objects already declared in §6.2 — `AttributionRow`, `BreachCandidate`, `ResolvedBreach`, `HypothesisRanking`, `RatifiedExplanation` — stay intact. The Eval sits one level up: it is the specification artifact that, when compiled, becomes the F_D code at S1 and S2 (or the F_P synthesis configuration at S3, or the F_H reviewer binding at S4) that produces those runtime objects.

This resolves the vocabulary tension that arose during design: Eval and BreachCandidate are not competing names for the same thing. Eval is constitutional (`WHAT must hold`); BreachCandidate is realization-emitted (`an instance where an Eval did not hold`).

### 11.2 Authoring surface

Evals are first-class spec surfaces parallel to `requirements/`. Each Eval lives in its own directory under `specification/evals/<module>/<eval_id>/` with three governed files:

```text
specification/evals/<module>/<eval_id>/
├── frontmatter.yaml       # categorization tuple per §11.3
├── predicate.md           # the rule body
└── commentary_template.md # the prose projection per §13
```

The three-file shape is load-bearing. The frontmatter declares the categorization; the predicate declares what must hold; the commentary template declares how the structured output is rendered into prose. The §11.5 compiler emits all three into runtime artifacts.

A single-file shorthand may exist for `status: candidate` Evals (deferred decision per §24 open question 3); ratified Evals are always three-file.

### 11.3 Frontmatter tuple

```text
EVAL-<MODULE>-<NNN>
  name                : kebab-case short name
  status              : candidate | ratified | superseded
  version             : integer
  effective_from      : YYYY-MM-DD
  supersedes          : eval_id@version | null
  owning_module       : attribution_base | coal_trading | ...
  stage               : S1 | S2 | S3 | S4
  regime              : F_D | F_P | F_H (matches stage by §10.2)
  window              : intraday | single_day | rolling_n_days | multi_day | trade_lifetime | period
  scope               : row | leg | trade | book | desk | portfolio | counterparty
  entities            : [Trade, Leg, Desk, RiskFactor, AttributionRow, ...]
  reasoning_pattern   : threshold | invariant | reconciliation | coverage | delta | ratio
                        | correlation | gain_surface_deviation | explanatory_manifold
  breach_kind         : kebab-case breach identifier emitted on failure
  severity_calculator : linear_by_magnitude | residual_reduction_ratio | per_dimension_deviation_vector
  compile_target      : duckdb-sql | polars-python | (others per Module volume_band)
  gain_surface_link   : optional ref to a declared GainSurface
  commentary_template : ./commentary_template.md (always relative)
```

The `(window, scope)` tuple plus the Module's declared `volume_band` (§8.4) drives compile-target selection — see §12.2.

### 11.4 Stage binding

| Stage | Eval shape | Compile output |
|---|---|---|
| S1 | Universal/mechanical predicate over AttributionRow | F_D SQL or Polars expression + F_D commentary renderer over a deterministic-finding payload |
| S2 | Overlay-style residual decomposition | F_D as a `CandidateFamily<Overlay>` member + F_D commentary renderer over a deterministic-resolution payload |
| S3 | Hypothesis-synthesis configuration (not a predicate; declares hypothesis priors, synthesis_method binding, inference budget participation) | F_P configuration + **F_D commentary renderer** over an F_P-produced HypothesisRanking payload (renders ranked hypotheses with confidence intervals; never asserts closure) |
| S4 | Reviewer interface contract + remediation taxonomy | F_H UI configuration; commentary at S4 appends the human ratification narrative to the rendered S3 commentary |

S1/S2 are the F_D-heavy stages and the focus of the compilation pipeline at v0.1.

### 11.5 SDLC traversal as compiler

The odd_sdlc traversal is extended with a deterministic compiler stage. The seven-stage pipeline:

```text
1. INTAKE       — accept new/updated Eval spec directory; assign change_class (per TICKET_METHOD)
2. VALIDATE     — frontmatter schema check; entity refs resolve against §9 domain types;
                  risk factor refs resolve against §10 taxonomy; commentary-template
                  purity check (every field referenced in template must exist on the eval's
                  structured payload type — fail closed on miss)
3. LOWER        — parse predicate body and commentary template to typed IR;
                  cross-check window/scope coherence
4. OPTIMIZE     — group co-scannable Evals; eliminate dead branches; constant-fold
5. EMIT         — generate target module: predicate function + commentary renderer function
                  against the Module's declared compile target
6. ATTEST       — emit compilation provenance (eval_id, version, target, predicate_hash,
                  commentary_template_hash, output_hash, compiler_version) as an admitted event
7. BIND         — register compiled module in the cascade Job's stage routing
                  (BIND fails closed for status: candidate Evals — see §14)
```

Steps 1–6 are F_D. Step 7 is F_D wiring in the cascade. No probabilistic step in the compilation pipeline itself. Both the predicate hash and the commentary-template hash are admitted so a replay at a historical date binds to *both* the predicate logic and the commentary prose active then.

### 11.6 Compile output shape

```text
build_tenants/typescript/compiled_evals/<target>/<module_name>/
├── <eval_id>_v<version>.{sql|py}            # generated predicate function
├── <eval_id>_v<version>_commentary.{py|ts}  # generated commentary renderer
├── <eval_id>_v<version>.manifest.json       # predicate_hash, commentary_template_hash,
│                                            # output_hash, target, deps, status
└── __init__.py                              # registers eval in the Module's stage binding
```

Both functions are pure over their inputs:

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

### 11.7 Three locked pre-decisions

Settled in the design commentary and constitutionalized here:

| Decision | Resolution |
|---|---|
| GainSurface placement | S3 hypothesis-space extension only. Not in §9 domain layer. Not in §10 RiskFactor union. Inferred artifact, not source-feed data. See §10.4 |
| Eval as runtime Markov object | Rejected. Eval is a spec-layer artifact compiled to runtime code. Runtime objects stay `BreachCandidate` / `ResolvedBreach` / `HypothesisRanking` / `RatifiedExplanation`. See §11.1 |
| Single-node-first bound | Stands at the project default. Per-Module `volume_band` declaration is the lever for escalation. Coal v0.1 runs at `low`. Distributed targets require declared volume evidence per Module |

## 12. Eval Catalog Architecture

The Eval catalog (full enumeration of v0.1 candidate Evals) lives in `[[20260523T151425Z_DESIGN_eval_suite_commodities_and_coal]]` as candidate evidence. This section constitutionalizes the *shape* of the catalog; the catalog *contents* are ratified individually as Evals get authored per the §7 sequence.

### 12.1 Two-tier Module structure

Per BOOTSTRAP §15 asset-class generalization:

- **`attribution_base`** — cross-asset Evals (work over any asset class). v0.1 catalog comprises ~27 entries (20 S1 universal mechanical + 7 S2 generic overlays + 3 S3 hypothesis configs + 1 S4 reviewer binding)
- **`coal_trading`** — coal-specific extension. v0.1 catalog comprises ~30 entries (15 S1 coal-specific mechanical + 12 S2 coal-specific overlays + 2 S3 coal hypothesis extensions + 3 S4 reviewer role bindings split by sub-desk)

Future Modules (`oil_trading`, `power_trading`, `rates_trading`, `fx_trading`, `equity_derivatives`, `credit_trading`) follow the same shape via the §15 extension points.

### 12.2 Two-axis categorization + volume_band

The seven categorization vectors from initial discussion collapse to two axes plus a per-Module band:

| Axis | Values |
|---|---|
| **window** | `intraday` / `single_day` / `rolling_n_days` / `multi_day` / `trade_lifetime` / `period` |
| **scope** | `row` / `leg` / `trade` / `book` / `desk` / `portfolio` / `counterparty` |
| **volume_band** (per Module, not per Eval) | `low` ≤ 100k/day · `mid` 100k–1M · `high` 1M–10M · `extreme` > 10M |

The compiler picks the compile target from the cross-product `(window, scope, volume_band)`. Authors write the rule once; the Module's declared band selects single-node SQL vs single-node Polars vs distributed.

Example bindings at coal `low`:

- `(single_day, row\|leg, low)` → DuckDB SQL, one date partition
- `(single_day\|multi_day, desk\|book, low)` → DuckDB SQL with `GROUP BY` across N partitions
- `(period, portfolio, low)` → DuckDB SQL or `polars-python` for richer math
- `(trade_lifetime, trade, low)` → reads `trade_lifecycle/` materialised view (§12.4)

### 12.3 Reasoning patterns

The `reasoning_pattern` field is descriptive metadata. The compiler does not use it; analysts and reviewers do. Values: `threshold`, `invariant`, `reconciliation`, `coverage`, `delta`, `ratio`, `correlation`, `gain_surface_deviation`, `explanatory_manifold`.

### 12.4 Trade-lifecycle materialised view

A small subset of v0.1 Evals (~5 entries from the catalog) carry `window: trade_lifetime`. Their rows are scattered across N daily partitions over months and would scan the full date-partitioned `attribution_rows` table to read 0.01% of each — worst-case for date partitioning.

Mitigation: an event-sourced materialised view at `trade_lifecycle/trade_id=<id>/*.parquet`, rebuilt from admitted lifecycle events. Compile target for `trade_lifetime` Evals binds to this view, not to `attribution_rows`. The view's rebuild mechanics are Step 10 of the §7 sequence.

## 13. Two-Part Report Structure and Commentary Regime

Settled in `[[20260523T151425Z_DESIGN_eval_suite_commodities_and_coal]]` §2.1 and clarified in `[[20260523T154035Z_REVIEW_design_state_and_scaling_architecture]]` §4.5.1. Constitutionalized here.

### 13.1 Two-part report

Every Eval emits a **two-part report** when it fires. Both parts are inherent. Neither is optional.

| Part | Owner | Authority |
|---|---|---|
| **(A) Eval details** | The Eval's predicate + evidence refs | Authoritative structured output. Becomes the `BreachCandidate` / `ResolvedBreach` / `HypothesisRanking` / `RatifiedExplanation` payload admitted through ABG |
| **(B) Commentary** | The Eval's `commentary_template` rendered against (A) | Read-model projection. Replay-derivable from (A). Carries no facts not present in (A) |

The architectural commitment: **the eval's structured payload inherently contains the reason for the breach inside its evidence refs**. The commentary is not a separate F_P pass that synthesizes a story over an opaque finding — it is a mechanical projection of the structured evidence into prose for human consumption.

### 13.2 Four compile-enforced invariants on commentary templates

- **Replay-safe** — same structured payload → byte-identical commentary
- **No phantom facts** — every template field reference must resolve against the structured payload type at compile time (§11.5 step VALIDATE). A template that references a missing field fails closed at compile time
- **Regime-faithful** — see §13.3 below; the renderer is always F_D but the template's *language* matches the regime of the payload
- **Versioned with the eval** — a commentary template change is a version bump on the Eval. Historical replay binds to the template version active at the replay date

### 13.3 Commentary regime invariant

> **For every Eval at every stage, the commentary renderer is F_D.** It is a pure deterministic function over the admitted structured payload. The template may render F_P-produced structured evidence (at S3, with explicit "candidate evidence" framing and confidence intervals) and may include F_H-written narrative (appended at S4 as the human's ratification note). The *rendering mechanic* is always F_D template execution. There is no path in the runtime cascade where commentary requires F_P inference per row.

The load-bearing reason: per-row F_P at attribution-row scale (100k–10M rows/day per Module) is economically and operationally impossible. A 5M-rows/day desk firing 5,000 S3 escalations with F_P per-firing inference is already a meaningful inference bill; if commentary also required F_P per row, it would be an order-of-magnitude larger bill — defeating the architecture.

This invariant is the project-local refinement of the substrate's F_D/F_P/F_H discipline. The substrate permits F_P generation of artifacts at any scale (odd_sdlc generates requirements, design modules, test cases through F_P workers); this project forbids per-row F_P generation because attribution-row volume forbids it.

### 13.4 Cross-firing synthesis is a separate question

Statements like "this is the third Indonesian thermal quality breach this week — consider extending overlay coverage" are genuinely F_P and genuinely valuable. They live in Tier 2 (analyst notebook), are bounded by analyst query rate, and are never on the per-row cascade path. They appear in the system as *analyst-initiated synthesis queries* over admitted breach history, not as fields in any per-firing commentary template. The §16.2 two-tier deployment is what makes this separation operational.

### 13.5 Humans only write at S4

Humans never write commentary at S1/S2/S3. The system writes it via the templated F_D renderer. Humans enter commentary only at S4 ratification, as appended narrative on top of the rendered F_P commentary. This inversion — *system writes, humans ratify* — is the load-bearing operator property that makes a 50-100k/day coal book and a millions/day high-volume desk both tractable for human review. It is the architectural reason this product is feasible at all.

## 14. Shadow→Ratified Promotion Path (Institutional Knowledge Accumulator)

Settled in `[[20260523T154035Z_REVIEW_design_state_and_scaling_architecture]]` §4.3 and §5.5 settlement item 3. Constitutionalized here as the load-bearing operator-facing property.

### 14.1 The problem

The eventual-consistency steel thread from §10.2 says: F_P+F_H ratify ambiguity once → new rule admitted → F_D handles the class forever. The mechanic by which an F_P-suggested overlay becomes a ratified S2 overlay was the most under-specified piece of the architecture pre-revision. Without an operational lifecycle, the architecture is a one-time setup that decays as new patterns out-pace the ratification cadence.

### 14.2 The lifecycle

A new pattern emerges in trading (e.g., "Indonesian thermal cargoes routinely underreport sulfur by 0.2% because the loading certificate uses ADB basis but the price formula uses NAR basis"):

1. **S3 hypothesis ranking** shows recurring `quality_underestimate` for the pattern in last N runs
2. **S4 reviewer ratification** identifies cause and remediation, admitted as `RatifiedExplanation` event
3. **Candidate Eval authored** — by analyst, F_P assistant, or hybrid — in `specification/evals/<module>/<eval_id>/` with `status: candidate`
4. **Compiler accepts the candidate Eval through VALIDATE/LOWER/OPTIMIZE/EMIT/ATTEST**. BIND fails closed for status `candidate` — the candidate cannot enter the runtime cascade
5. **Shadow execution** — Tier 2 runs the compiled candidate Eval against admitted historical data and live admitted data, writing outputs to `shadow_breach_candidates/` and `shadow_resolved_breaches/`. Shadow outputs are tagged with `shadow_run_id` and the source candidate Eval ref. They are not visible to S4 reviewer routing
6. **Agreement accumulation** — for each shadow firing, the system compares the shadow output against the actual cascade outcome at the same `(date × trade × leg × factor)`. Agreement means the shadow Eval's resolution agrees with the eventual S4 ratification (or with the runtime S1/S2 closure if the cascade closed without S4 escalation). Disagreements are also logged
7. **Promotion gate** — when the candidate accumulates N (default 20) agreements over M (default 10) business days with no significant disagreements, the `shadow_eval_promotion_ledger` opens a promotion request
8. **TICKET_METHOD admission** — the promotion request enters the standard ticket lifecycle. Reviewers see the shadow evidence as admission authority. Approval flips `status: candidate` → `status: ratified`, compiles the binding (BIND now succeeds), and admits a `ratified_eval_bindings` event
9. **Next runtime cascade run** picks up the ratified Eval; the class of breaches the original pattern represented now closes at F_D

### 14.3 Why this works

The candidate-status Eval is structurally distinct from a ratified Eval at every governance gate. The compiler's BIND step is the fail-closed boundary — candidate Evals compile but never bind to the runtime cascade. Shadow runs are operationally separate from the SLA-bound Tier 1 path. The promotion ledger is content-addressed; the admission carries the shadow evidence as authority.

The N=20 / M=10 BD defaults are settable per Module via the manifest (§16.4). Higher-stakes Modules can require longer bake-ins; lower-stakes Modules can promote faster. The defaults are conservative.

### 14.4 What this is not

The promotion path is not a way to bypass governance. It is a way to make governance *operational* at the cadence trading desks actually need (days to weeks per new overlay) rather than the cadence pure code-review imposes (none — code reviews don't accumulate domain agreement). The architecture trades latency-to-ratification (candidate Eval can run in shadow on day 1) for confidence-at-ratification (the shadow evidence is concrete).

## 15. Asset-Class Generalization

Coal is the first instance. The architecture generalizes through four named extension points (GTL-native, requires no cascade rewrite):

| Extension point | GTL mechanism |
|---|---|
| Product subtypes | `CandidateFamily<Trade.product>` |
| Overlay family members at S2 | `CandidateFamily<Overlay>` |
| RiskFactor kinds | Discriminated union in the typed schema layer |
| S3 hypothesis space | Per-Module configuration |

Future asset-class Modules (`oil_trading`, `power_trading`, `rates_trading`, `equity_derivatives`, `credit_trading`) compose by adding members to these extension points without modifying base cascade graph functions or runtime. Each Module declares its own `volume_band` (§8.4), its own `s3_inference_budget`, its own reviewer role bindings, and its own Eval catalog directory under `specification/evals/<module>/`. v0.1 closes when one asset-class Module (coal) is end-to-end functional and the second-instance scaffolding is demonstrably parallel.

Per BOOTSTRAP §11 (settled): asset-class generalization is the architecture's payoff. Each new Module earns its volume_band and compile-target through declared evidence.

## 16. Scaling Architecture

Settled in `[[20260523T154035Z_REVIEW_design_state_and_scaling_architecture]]` §5. The architecture must scale on five orthogonal axes; the response per axis is different.

### 16.1 Five orthogonal scaling axes

| Axis | Range | Response |
|---|---|---|
| **(1) Rows per day** | 100k → 10M+ | Per-Module `volume_band` selects compile target (§8.4, §12.2) |
| **(2) Eval catalog size** | 50 → 5,000 | OPTIMIZE step fuses co-scannable Evals (§11.5); compilation parallelizable across Modules |
| **(3) Module count** | 1 (coal) → 10+ | Module as publication boundary (§15); independent compile/deploy/band |
| **(4) Replay depth** | 1y → 7y+ | Date-partitioned Parquet (§8.3); cold-storage tiering operationally separate; predicate pushdown |
| **(5) Analyst concurrency** | 5 → 500 | Tier 2 shared-nothing: DuckDB-per-analyst over shared Parquet (§16.2) |

These axes do not co-scale. A power desk might be 5M rows/day but one Module. A multi-asset trading house might be 1M rows/day total but 8 Modules. A regulator using the system for audit cares about (4) but not (5). Coal v0.1 sits at low ends of all five.

### 16.2 Two-tier deployment

The architecture splits into two tiers with different scaling profiles.

**Tier 1 — Runtime cascade (production-critical, SLA-bound)**

- Trigger: EOD batch (recommendation; settlement item 1, §16.4)
- Deployment: one process per Module (or one process running multiple Modules)
- Engine: embedded DuckDB + Postgres + Parquet snapshot store
- F_D path: S1 + S2 compiled to `duckdb-sql` or `polars-python` per Module's `volume_band`
- F_P path: S3 bounded by declared inference budget
- F_H path: S4 ratifications rate-limited by reviewer queue throughput
- Output: BreachCandidate / ResolvedBreach / HypothesisRanking / RatifiedExplanation admitted; rendered commentary admitted alongside; both indexed by `(eval_id, version, snapshot_id)`

**Tier 2 — Analyst workbench (exploratory)**

- Trigger: analyst-initiated query
- Deployment: shared-nothing — each analyst gets their own DuckDB-on-Parquet, reading from the shared snapshot store
- Engine: Python + DuckDB + Polars + Plotly in Marimo/Jupyter notebooks
- Read scope: same admitted-event log and Parquet snapshot store as Tier 1
- Shadow runs: candidate Evals run against admitted data without binding to the cascade (§14)
- Cross-firing F_P synthesis: lives here exclusively; never on per-row path

The two tiers share *one* substrate: the immutable admitted-event log and the Parquet snapshot store. They never share compute. They never share writable state. Tier 1 emits events; Tier 2 reads them. Tier 2 produces shadow evidence; ratification through TICKET_METHOD admits selected shadow Evals back into Tier 1's catalog.

### 16.3 Per-axis scaling techniques

| Axis | Technique |
|---|---|
| Rows per day | Per-Module `volume_band` → compile target. Coal `low` → `duckdb-sql`; mid → `polars-python`; high → ClickHouse or Polars-on-Dask; extreme → warehouse |
| Eval catalog size | OPTIMIZE-stage co-compilation. N S1 Evals scanning attribution_rows for same (date, desk) fuse into one scan with N predicate branches |
| Module count | Independent compile + deploy + band per Module. Cross-Module sharing restricted to shared RiskFactor union and base Module's S1/S2 cascade graph functions |
| Replay depth | Date-partitioned Parquet with predicate pushdown. Cold-storage tiering operationally separate. Replay cost O(matching partitions), not O(history) |
| Analyst concurrency | Tier 2 shared-nothing. Object storage IOPS scales linearly; analytical workloads are read-heavy and cache-friendly |

### 16.4 Five settlement items

Five decisions convert the scaling architecture from candidate evidence to ratifiable design. These are open items that GOALS §7 Step 6 closes.

1. **Run boundary semantics** — EOD batch (recommended). Matches settlement vocabulary, snapshot model, reviewer cadence. Continuous or event-triggered runs deferred until a v0.2+ use case requires sub-daily attribution
2. **F_P inference budget mechanism** — per-Module declared `s3_inference_budget` with `max_firings_per_run`, `confidence_floor`, `escalation_threshold`. Run exceeding `max_firings_per_run` degrades S3 (weaker synthesis method / shorter prompts / batch grouping); degradation admitted as event
3. **Shadow-eval promotion path** — see §14. Default thresholds N=20 successful agreements over M=10 BD; settable per Module via manifest
4. **Reviewer UI for v0.1** — notebook-embedded. Reviewer opens a Marimo notebook scoped to their reviewer_role; notebook queries `breach_review_queue`; displays Part (A) JSON + Part (B) rendered commentary + structured input fields; submits via callback writing `RatifiedExplanation` event back. No separate web app
5. **Module manifest** — `build_tenants/typescript/modules/<module>/manifest.yaml` declares `volume_band`, optional per-`(window, scope)` compile-target overrides, runtime resource profile, `s3_inference_budget`, `s3_synthesis_method`, reviewer role bindings, `shadow_eval_bake_in_policy`. Single point where tenant-local realization decisions live

## 17. Initial Requirement Family Skeleton

To be elaborated under `specification/requirements/` once this bootstrap is ratified. The seed families:

| Family | Working title | Scope |
|---|---|---|
| REQ-F-OTE-001..n | Canonical trade representation | Typed records for Trade / Product subtypes / Desk / Counterparty / RiskFactor; mapping admission from upstream T&R feed |
| REQ-F-OTE-010..n | Snapshot-pinned replay | Every record carries snapshot_id; replay at any (date, snapshot_id, rule_version, eval_version) produces identical output |
| REQ-F-OTE-020..n | Cascade F_D admission | S1 universal checks; arithmetic identities; reconciliation tolerances; coverage-gap detection |
| REQ-F-OTE-030..n | Cascade F_D model overlays | CandidateFamily<Overlay> at S2; each overlay deterministic; residual reduction additive |
| REQ-F-OTE-040..n | F_P probability synthesis at S3 | Candidate-evidence framing; explicit confidence intervals; declared synthesis_method; never independent closure authority; inference-budget bounded |
| REQ-F-OTE-050..n | F_H human ratification at S4 | Reviewer interface contract (notebook-embedded for v0.1); remediation action taxonomy; eventual-consistency rule store update |
| REQ-F-OTE-060..n | Asset-class Module extension | Four extension points (Trade.product, Overlay, RiskFactor, S3 hypothesis space) admitted as the only sanctioned generalization mechanism; per-Module manifest |
| REQ-F-OTE-070..n | Reproducibility and audit | Every breach explanation chain (S1 → S2 → S3 → S4) reconstructable from admitted events + Eval version + commentary template version |
| REQ-F-OTE-080..n | Coal domain coverage | Thermal + metallurgical product subtypes; quality / freight / pricing-period / demurrage overlays; benchmark + quality differential + freight + FX factor coverage |
| REQ-F-OTE-090..n | **Eval authoring & compilation contract** | Per-Eval directory shape; SDLC compiler stages; compile-target-pluggable backends; commentary-template purity check; ATTEST admits predicate_hash + commentary_template_hash |
| REQ-F-OTE-100..n | **Two-part report + commentary regime invariant** | Part (A) structured payload, Part (B) F_D-templated commentary; commentary renderer always F_D; no per-row F_P path |
| REQ-F-OTE-110..n | **Shadow→ratified promotion contract** | Candidate-status Evals compile but do not BIND; shadow execution; agreement accumulation; TICKET_METHOD admission with shadow evidence as authority |
| REQ-F-OTE-120..n | **Scaling architecture** | Five orthogonal axes; two-tier deployment (Tier 1 runtime cascade / Tier 2 analyst workbench); shared-nothing analyst tier; per-Module volume_band selects compile target |

REQ-F-OTE-090 through REQ-F-OTE-120 are the new families introduced by this revision; the first eight (001-080) carry forward from the prior seed.

## 18. Reference Material

| Document | Role |
|---|---|
| `.ai-workspace/comments/claude/20260519T230000Z_RESEARCH_pnl_explain_forensic_attribution.md` | Research seed — landscape, coal data model, cascade architecture (Appendix B), generalization pattern (Appendix C), implementation stack (Appendix D). Imported from `odd_sdlc/.ai-workspace/comments/claude/`; treat as commentary, not law |
| `.ai-workspace/comments/claude/20260523T144237Z_DESIGN_eval_authoring_to_fd_compilation_pipeline.md` | Eval pipeline ADR — design_reframe consolidating Eval authoring + SDLC-as-compiler. Constitutionalized into §§11-12 of this bootstrap |
| `.ai-workspace/comments/claude/20260523T151425Z_DESIGN_eval_suite_commodities_and_coal.md` | Eval catalog (60 entries cross-asset + coal-specific) + two-part report commitment. Catalog stays in commentary as authoring backlog; commitment constitutionalized into §13 |
| `.ai-workspace/comments/claude/20260523T154035Z_REVIEW_design_state_and_scaling_architecture.md` | Design state review + scaling architecture + commentary regime invariant + ten-step sequence to v0.1 closure. Constitutionalized into §§14-16 of this bootstrap |
| `specification_methodology/specification/standards/SPEC_METHOD.md` | Constitutional method governing all change |
| `specification_methodology/specification/standards/ODD_METHOD.md` | Graph-native product-construction law |
| `specification_methodology/specification/standards/TICKET_METHOD.md` | Change admission mechanics; shadow→ratified promotion admitted through this surface |
| `specification_methodology/specification/standards/DESIGN_MODULE_METHOD.md` | Design decomposition law |
| `specification_methodology/specification/standards/UX_METHOD.md` | Reviewer UI design surface |
| `abiogenesis/docs/LLM_GTL_APP_BUILDER_GUIDE.md` | Substrate ontology and operating contract; F_P/F_D constitutional boundary section is project-load-bearing |
| `odd_sdlc/specification/requirements/16-edge-gain-closure-contract.md` | Closure law inherited from substrate |
| `odd_sdlc/specification/requirements/18-typed-construction-algebra.md` | Typed F_P stage carrier inheritance |
| `odd_sdlc/.ai-workspace/tickets/active/T-172-realize-staged-disambiguation-graph-and-decomposition-admission.md` | Staged decomposition inheritance |
| `odd_sdlc/.ai-workspace/tickets/active/T-173-realize-complexity-admitted-min-fp-traversal-selection.md` | Complexity-admitted traversal selection inheritance |

## 19. Reading Order for the Operator

When an agent or operator picks up this project for the first time:

1. Read this `BOOTSTRAP.md` for the seed shape, method inheritance, eval pipeline, two-part report commitment, scaling architecture
2. Read `SPEC_METHOD.md` and `ODD_METHOD.md` for the constitutional and graph-native method law
3. Read `LLM_GTL_APP_BUILDER_GUIDE.md` for substrate ontology and the F_P/F_D constitutional boundary
4. Read the research source (`20260519T230000Z_RESEARCH_...`) for landscape understanding of the domain
5. Read the four referenced odd_sdlc requirement and ticket surfaces for the substrate's typed-stage construction algebra, edge accounting, complexity-admitted traversal selection
6. Read the three converging design commentary documents (`20260523T144237Z_...`, `20260523T151425Z_...`, `20260523T154035Z_...`) for the full design rationale that constitutional sections §§11-16 compress
7. Install the odd_sdlc TypeScript tenant into this workspace. The installer materializes a single `.abiogenesis/` root
8. Initiate the §7 ratification traversal sequence, starting at Step 1

## 20. Bootstrap Closure Law

This document is **provisional** — the bootstrap seed, not the ratified constitutional surface.

This document is **closed** and becomes provenance-only when all of the following are true:

- `specification/INTENT.md` exists, derived from §5, and is ratified
- `specification/PRODUCT.md` exists, derived from §6, and is ratified
- `specification/GOALS.md` exists, derived from §7, and is ratified
- `specification/requirements/` contains the thirteen families from §17 (with concrete IDs and ACs) and they are ratified
- `specification/evals/attribution_base/` contains the first ratified Evals (`EVAL-BASE-{003,004,005,006,007}` minimum, per §7 Step 5)
- `specification/evals/coal_trading/` contains at least `EVAL-COAL-101` ratified (per §7 Step 7)
- `build_tenants/typescript/` exists with the minimum scaffold per §8 including the Eval compiler with `duckdb-sql` backend
- `build_tenants/typescript/modules/coal_trading/manifest.yaml` exists with declared `volume_band: low`, `s3_inference_budget`, `s3_synthesis_method: bayesian_network`, reviewer role bindings, `shadow_eval_bake_in_policy`
- `.abiogenesis/` install root is materialized with the active odd_sdlc TypeScript tenant
- The `post_trade_attribution` GTL module is published with the four cascade graph functions
- The first traversal pass under odd_sdlc produces an admitted attribution run for one synthetic coal trade with at least one rendered commentary (Part B)
- The shadow→ratified promotion path is demonstrated on synthetic data (§7 Step 8)
- The steel-thread loop is demonstrated end-to-end on synthetic data: S1/S2 fail to close → S3 hypothesis ranking → S4 ratification → shadow candidate Eval authored → shadow agreement accumulates → TICKET_METHOD admission → next-run F_D closure (§7 Step 9)
- `trade_lifecycle/` materialised view rebuild mechanic is implemented and demonstrated (§7 Step 10)

These conditions record the former bootstrap target. They are not current
closure obligations unless ratified through `INTENT.md`, `PRODUCT.md`, and the
live requirement surface. This file is provenance for the seed that preceded
the current constitutional chain.

## 21. Non-Closure Conditions

This bootstrap cannot be closed if any of the following are true:

- domain data records are encoded *inside* GTL nodes rather than referenced from external typed schemas
- any cascade stage runtime path consults F_P or F_H during per-row attribution
- **any commentary path on the per-row cascade requires F_P inference at runtime** (violation of §13.3 commentary regime invariant)
- prompt text carries domain content that should be admitted record state
- attribution row computation is non-replayable from admitted events + snapshot + rule_store + Eval version + commentary template version at a timestamp
- coal-specific overlay or risk factor names leak into the base cascade graph functions or runtime
- the S4 → rule store feedback loop is implemented as a side channel rather than as admitted events with versioned rule effective dates
- the shadow→ratified promotion path is implemented as direct ratification without the shadow evidence accumulation gate (violation of §14)
- candidate-status Evals BIND to the runtime cascade (violation of §11.5 step 7 fail-closed gate)
- the project depends on `dask-python`, `spark-python`, ClickHouse, or warehouse compile targets in any v0.1 Module without declared per-Module volume evidence
- Tier 1 and Tier 2 share writable state (violation of §16.2 shared-nothing-on-writes commitment)
- Cross-firing F_P synthesis appears as a field in any per-firing commentary template (violation of §13.4 separation)

## 22. Inheritance and Substrate Update Policy

When `odd_sdlc` releases a new version, this project may upgrade by:

1. Reviewing the substrate's changelog for changes to the cascade architecture, typed F_P stage carriers, edge accounting, complexity-admitted traversal selection, or the evaluate.C/F_P semantic_judgment_rule pattern
2. Re-running the odd_sdlc installer; the new release re-materializes `.abiogenesis/` with refreshed `cli-runtime.mjs`, `docs/standards/`, `odd_sdlc/typescript/`, and install manifests in place
3. Running deterministic replay against a pinned historical attribution snapshot to confirm zero behavioral drift on existing data
4. Recompiling all ratified Evals; the ATTEST step admits new `predicate_hash` and `commentary_template_hash` values if the IR or backend changed. Recompilation is an admitted event; replay at historical dates binds to the historical compilation per §13.2 invariant 4
5. Repricing any local design or requirement surface whose dependency on substrate behavior has changed

The project does not vendor or fork the substrate. It depends on the released line and re-installs forward.

## 23. Open Questions

Items deferred from the BOOTSTRAP seed and the design commentary; to be answered during the §7 sequence:

1. **Regulatory scope** — internal forensic only, or FRTB PLA test compliance? Affects HPL/RTPL strictness and PLA-test statistical infrastructure. Recommendation: internal forensic only for v0.1
2. **Greek provenance** — consumed from upstream T&R system, or computed locally? Recommendation: consumed upstream for v0.1; pricer integration deferred
3. **Breach granularity** — trade-level, leg-level, factor-level, desk-level? Recommendation: `(date × trade × leg × factor)` is the AttributionRow granularity; breaches aggregate from there per the eval `scope` field
4. **Multi-asset onboarding sequence after coal** — oil/gas next? power? rates? Decision deferred to v0.2 GOALS
5. **Reviewer UI scope** — web app, terminal-based, notebook-embedded? Settlement item 4: **notebook-embedded for v0.1**
6. **S3 synthesis method choice** — Bayesian network, learned classifier, agent reasoning? Recommendation: **Bayesian network for coal v0.1**; settable per Module via manifest
7. **Eval predicate language** — YAML-with-schema body fields, custom textual DSL, or TypeScript types? Open question 1 from the pipeline ADR; deferred to §7 Step 5 implementation
8. **Eval IR specification** — defined before backend authoring. Open question I from the design review; closed by §7 Step 4
9. **Co-compilation grouping strategy** — when the OPTIMIZE step fuses Evals scanning the same data. Open question 3 from the pipeline ADR; deferred until first volume measurement
10. **Eval ↔ rule_store schema coupling** — does the Eval registry sit inside `rule_store` or alongside it? Open question 4 from the pipeline ADR; deferred to §7 Step 4

These ten questions are not closure blockers for this bootstrap. They are blockers for individual ratification steps in §7. Each step's outcome carries the resolution of the relevant questions.

---

**End of bootstrap provenance.** Current authority begins at `INTENT.md` and
flows through `PRODUCT.md`, the future `GOALS.md`, and the future live
requirement surface.
