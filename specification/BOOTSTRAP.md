# odd_trading_eval — SPEC_METHOD Bootstrap

**Status**: Seed bootstrap. Provisional until `INTENT.md`, `PRODUCT.md`, `GOALS.md`, and `specification/requirements/` are derived from this document and ratified by an odd_sdlc traversal pass.

**Method inheritance**: `SPEC_METHOD.md`, `ODD_METHOD.md` (master at `https://github.com/foolishimp/specification_methodology`; installed at `.abiogenesis/docs/standards/` after odd_sdlc install).

**Substrate (installed product)**: `odd_sdlc` — the released ODD-shaped SDLC product carrying the GTL/ABG construction graph, typed stage carriers, cascade attribution patterns, edge-accounting register, and complexity-admitted traversal selection.

**Research source**: `.ai-workspace/comments/claude/20260519T230000Z_RESEARCH_pnl_explain_forensic_attribution.md` (imported into this project; original provenance: `odd_sdlc/.ai-workspace/comments/claude/20260519T230000Z_RESEARCH_pnl_explain_forensic_attribution.md`). Treat that document as the seed knowledge surface for INTENT/PRODUCT/requirements derivation. It is research commentary, not ratified specification; this bootstrap is the conversion point.

---

## 1. Position

`odd_trading_eval` is an ODD product built using `odd_sdlc` as the installed substrate. The product realizes post-trade P&L attribution and forensic breach analysis for multi-asset derivatives. Coal is the first instance; the architecture is asset-class-agnostic by construction.

Per `ODD_METHOD.md` §6, the installed product (odd_sdlc) provides the graph-native method, traversal-control substrate, runtime, command surfaces, and scaffold templates. This project owns its constitutional surfaces (this bootstrap, the derived INTENT/PRODUCT/GOALS/requirements) and its realization root (`build_tenants/typescript/`). The substrate is not co-equal authority; it carries `HOW` for construction. The constitutional `WHAT` belongs to this project.

## 2. Recursive Product Taxonomy (per `SPEC_METHOD.md` §Recursive Product Taxonomy)

| Role | Identity | Mutability |
|---|---|---|
| **Substrate** | `odd_sdlc` released product | Immutable for this project's first release cycle |
| **Source Project** | `odd_trading_eval` workspace (this repo) | Mutable; builds toward `odd_trading_eval` v0.1 release cut |
| **Domain** | Post-trade P&L attribution + forensic breach analysis for multi-asset derivatives | Constitutional `WHAT`, owned by this project |
| **Build Tenant** | `typescript` | First realization; canonical root `build_tenants/typescript/` |
| **First Asset-Class Module** | `coal_trading_attribution` | Proves the architecture; generalizes per §11 below |

The substrate/source/product/install boundary is strict. odd_sdlc as substrate is not a rival constitution for this project. Its method governance applies; its product-specific surfaces do not.

## 3. Method Inheritance and Authority Boundary

| Layer | Source | Role for odd_trading_eval |
|---|---|---|
| Constitutional method | `SPEC_METHOD.md` | Goals → Intent → Product → Requirements → Design → Code chain governs all change |
| Graph-native product law | `ODD_METHOD.md` | Product is built as typed assets + GTL graph functions + ABG runtime + projection surfaces |
| Ticket and sprint mechanics | `TICKET_METHOD.md` | Every substantive change goes through intake triage with explicit `change_class` |
| Design decomposition | `DESIGN_MODULE_METHOD.md` | Design surfaces decompose into governed modules with explicit derivation |
| Construction substrate | `odd_sdlc` v3.7.1-rc.4+ released line | Provides typed F_P stage carriers, cascade attribution patterns, edge accounting, complexity-admitted traversal selection, F_D/F_P/F_H regime split |
| Domain semantic law | This project's specification | Owns trade/desk/counterparty/risk-factor/attribution semantics |

The composition rule from `ODD_METHOD.md` §3 applies:

- `SPEC_METHOD.md` and `ODD_METHOD.md` tell us *how* to build the product as a spec-driven graph-native ODD product
- `odd_sdlc` as substrate provides the *construction graph* the product traverses
- This project's local INTENT/PRODUCT/requirements tell us *what* semantic law the product must honor

The substrate is the inheritance layer for **construction**, not for **domain meaning**. Domain meaning is owned exclusively here.

## 4. Project Identity

- **Project slug**: `odd_trading_eval`
- **Primary build tenant**: `typescript`
- **Active tenant runtime**: Node 20+ with embedded DuckDB, Postgres for operational state, Parquet for snapshot store
- **Repository location**: `/Users/jim/src/apps/odd_trading_eval/`
- **Installed runtime root**: `.abiogenesis/` — single folder holding all installed substrate after the first odd_sdlc install pass. Contains `cli-runtime.mjs`, `docs/standards/` (installed methodology library), `odd_sdlc/typescript/` (the odd_sdlc tenant), `install-manifest.json`, `install-provenance.json`, `config/`, `package-pack/`, `package-extract/`, `typescript-installer-manifest.json`. Project-owned surfaces under `.abiogenesis/` are read-only consumer-side; they refresh on the next install.

## 5. INTENT (provisional, to be ratified into `INTENT.md`)

### 5.1 Purpose

Build a post-trade P&L attribution and forensic breach analysis system for multi-asset derivatives. Operate over data received from an existing trade and risk system; canonicalize that data into a typed domain model; compute multi-bucket attribution decomposition; detect breaches across reconciliation, model, and policy dimensions; and explain breaches through a deterministic F_D first cascade that escalates to probabilistic F_P synthesis only when F_D cannot close, and to human F_H ratification only when F_P cannot decide.

### 5.2 Outcomes

- Canonical model surface for trades, desks, counterparties, and risk factors that maps cleanly from upstream T&R system feeds and supports cross-asset attribution
- Daily and historical attribution pipeline computing actual P&L (APL), hypothetical P&L (HPL), and risk-theoretical P&L (RTPL) with full attribution-bucket decomposition
- Four-stage cascade (S1 F_D broad → S2 F_D model overlay → S3 F_P probability synthesis → S4 F_H human ratification) running over admitted attribution rows
- Replay-visible event-sourced lifecycle so any historical breach is recomputable from admitted events + market snapshots + rule store at any timestamp
- Eventual-consistency rule-store feedback loop: every S4 ratification ratchets one class of breaches from F_P/F_H back into F_D for future runs
- Coal as the proving asset-class instance; the architecture generalizes to oil/gas, power, rates, FX, equity derivatives, and credit through Module composition

### 5.3 Constraints

- **F_D throughout the runtime path**: every transformation in the cascade is deterministic. F_P enters only at S3 synthesis; F_H only at S4 ratification. No agent reasoning inside per-row attribution.
- **Domain data is a separate typed schema layer**, external to GTL. GTL handles workflow, lifecycle, governance; the domain records (Trade, Desk, Counterparty, RiskFactor, AttributionRow) live in a typed schema language (Avro / Pydantic / TypeScript types) referenced from GTL nodes.
- **No worker prompt may carry domain content that should live in admitted records**. Prompt rendering is consumer-side; the typed carrier is authority.
- **Snapshot-pinned reproducibility**: every record carries a snapshot_id; every rule has a versioned effective date; replay must produce identical output for the same snapshot + rule version.
- **Single-node first**: target single-host Node + DuckDB + Postgres deployment. Distributed compute (Spark, ClickHouse) is admitted only with declared scale evidence per §8.
- **No FRTB-grade regulatory PLA test obligation in v0.1**. The architecture supports it; the v0.1 closure does not require it.

## 6. PRODUCT Definition (provisional, to be ratified into `PRODUCT.md`)

### 6.1 Product position

`odd_trading_eval` is a post-trade analytical system. It is not a front-office trade booking system, not a real-time risk system, and not a pricing engine. It consumes admitted state from upstream systems (trades, market data, greeks) and produces attribution rows, breaches, and forensic explanations as durable analytical truth.

### 6.2 Key terms

| Term | Definition |
|---|---|
| **Trade** | Canonical typed record carrying a deal between counterparties. Discriminated union over `PhysicalCargo` (coal first), `PaperTrade`, future product subtypes |
| **Desk** | Organizational unit owning trades; hierarchy with parent_desk_ref; carries limits, reporting currency, attribution scope |
| **Counterparty** | Legal entity in a trade; carries credit, master agreement, parent group ref |
| **RiskFactor** | Typed market quantity that drives P&L. Discriminated union: coal benchmark curve, quality differential, location basis, freight route, FX rate, interest rate, carbon cost, plus generic cross-asset factors |
| **AttributionRow** | Wide-DataFrame record: one row per (date × trade × leg × factor) with attribution buckets as columns (theta, delta, gamma, vega, vanna, volga, rho, basis, freight, quality, demurrage, pricing-period, hedge effectiveness, lifecycle, new trades, fees, residual) |
| **BreachCandidate** | S1 output: typed breach detected by mechanical F_D checks |
| **ResolvedBreach** | S2 output: BreachCandidate with applied overlays and reduced residual |
| **HypothesisRanking** | S3 output: probabilistic ranking of breach causes with evidence refs and confidence intervals |
| **RatifiedExplanation** | S4 output: human-confirmed cause and remediation action |
| **Rule store** | Versioned, transactional store (Postgres) of overlay rules, thresholds, mapping rules; each entry has an effective date so historical replay uses the rule version active at that date |
| **Snapshot** | Pinned (date, snapshot_id) tuple identifying market data, positions, greeks, and rule-store version for replay |

### 6.3 Boundaries (in / out of scope for v0.1)

**In scope:**
- Coal as the proving asset class (thermal + metallurgical)
- Canonical trade model with PhysicalCargo and PaperTrade subtypes
- Risk factor taxonomy covering coal benchmark curves, quality differentials, location basis, freight routes, FX, interest rate, carbon
- S1 universal threshold/integrity checks; S2 coal-specific overlays (quality, freight, demurrage, pricing-period, hedge effectiveness) + generic overlays (convexity, cross-greek, lifecycle, Shapley); S3 Bayesian-network or rule-based synthesis (ML deferred); S4 reviewer UI capturing ratification
- Forensic notebook surface (Python + DuckDB + Plotly) sharing the Parquet snapshot store
- Module composition pattern documented and one second asset class scaffolded as validation

**Out of scope for v0.1:**
- Multi-asset cross-correlation pricing (we consume greeks, we don't compute them)
- Real-time intraday breach detection (daily/period batch only)
- Distributed deployment (Spark, ClickHouse cluster)
- FRTB regulatory PLA test certification
- Concurrent F_P worker dispatch (deferred to ABG concurrent execution support per `odd_sdlc` T-173 ABG boundary clause)
- ML-driven S3 classifier (Bayesian net or rule-based first; ML in a later release)

## 7. GOALS (current work wave, to be ratified into `GOALS.md`)

The current work wave is the v0.1 substrate slice. Concrete goals:

1. **Bootstrap ratification** — convert this BOOTSTRAP.md into governed INTENT.md, PRODUCT.md, GOALS.md, and an initial `specification/requirements/` family through one odd_sdlc traversal pass
2. **Coal domain data layer** — declare typed schemas for `Trade`, `PhysicalCargo`, `PaperTrade`, `Desk`, `Counterparty`, `RiskFactor`, `AttributionRow` in the chosen schema language; admit mapping rules from the upstream T&R feed
3. **Cascade architecture skeleton** — declare the GTL module `post_trade_attribution` with the four cascade graph functions (S1-S4), the `CandidateFamily<Overlay>` extension point, and the Job routing
4. **Snapshot store and replay** — Parquet directory layout (trades, lifecycle_events, market_snapshots, greeks, attribution_rows, breach_candidates, resolved_breaches, hypothesis_rankings, ratified_explanations); Postgres tables for rule store, threshold registry, overlay registry, reviewer workflow
5. **First end-to-end attribution slice** — one trade, one day, one breach detected by S1, resolved by S2, with replay-visible provenance
6. **Reviewer UI minimum** — capture S4 ratification and emit the RatifiedExplanation back into the event log

The work wave is bounded: do not extend to additional asset classes, FRTB compliance, or distributed execution until v0.1 closes.

## 8. Build Tenant Declaration

### 8.1 Tenant identity

- **Tenant name**: `typescript`
- **Realization root**: `build_tenants/typescript/`
- **Language**: TypeScript on Node 20+
- **Operational footprint**: single Node process + Postgres + filesystem/S3 Parquet store. No separate analytical DB server, no Spark cluster, no JVM.

### 8.2 Tech stack

| Layer | Library / system | Role |
|---|---|---|
| Workflow / governance | GTL/ABG via `@odd-sdlc/typescript-tenant` | Workflow, cascade routing, event sourcing, replay, F_D/F_P/F_H regime split |
| Analytical engine | `@duckdb/node-api` | All S1/S2 SQL queries; Parquet reads/writes; cross-join to Postgres via `postgres_scanner` extension |
| Operational state | Postgres + `pg` or `postgres` client | Rule store, threshold registry, overlay registry, reviewer workflow, configuration |
| Snapshot store | Parquet (Zstd compression) on disk or S3, partitioned by date and snapshot_id | All durable analytical data; universal format readable by DuckDB, Python notebooks, future Spark |
| Cross-language interop | Apache Arrow IPC format | Zero-copy data exchange with analyst Python notebooks |
| Parallelism (intra-process) | `worker_threads` | S2 overlay fan-out, per-trade greek bumps |
| ML inference (S3 optional) | `onnxruntime-node` | If/when ML classifier replaces Bayesian net at S3 |
| Job queue (optional) | `bullmq` + Redis | Stage dispatch across worker processes; not required for v0.1 |
| Analyst notebook surface | Python + Jupyter/Marimo + DuckDB + Polars + Plotly + `shap` | External to the runtime tenant; reads the same Parquet store |
| Schema language | TypeScript types (primary), Avro for upstream interop where required, Pydantic for Python notebook side | Domain data records; external to GTL nodes |

### 8.3 Storage layout

```text
Bucket / disk root:
├── trades/snapshot_id=<id>/*.parquet
├── lifecycle_events/date=<YYYY-MM-DD>/*.parquet
├── market_snapshots/date=<YYYY-MM-DD>/snapshot_id=<id>/*.parquet
├── greeks/date=<YYYY-MM-DD>/snapshot_id=<id>/*.parquet
├── attribution_rows/date=<YYYY-MM-DD>/snapshot_id=<id>/*.parquet
├── breach_candidates/date=<YYYY-MM-DD>/*.parquet
├── resolved_breaches/date=<YYYY-MM-DD>/*.parquet
├── hypothesis_rankings/date=<YYYY-MM-DD>/*.parquet
└── ratified_explanations/date=<YYYY-MM-DD>/*.parquet

Postgres:
├── rule_store (rule_id, version, effective_date, payload)
├── threshold_registry (per-desk, per-product)
├── overlay_registry (active overlays, version, effective_date)
├── reviewer_workflow (breach_id, status, assigned_reviewer, sla)
├── ratified_explanation_mirror (operational view of latest ratifications)
└── config, users, roles, desks
```

### 8.4 Scale threshold rationale

The single-node-first decision is bounded by data volume. From the research source:

| Daily attribution data volume | Best fit |
|---|---|
| Up to ~100 GB / day | Node + DuckDB single host (this project's target) |
| 100 GB – 1 TB / day | Node + DuckDB partitioned by date, or ClickHouse |
| 1 TB+ / day | Distributed analytical DB; Spark only at petabyte scale |

For coal at trading-house scale, daily attribution rows stay comfortably under 100 GB. The architecture admits horizontal scale-out as a future change without rewriting cascade logic — Parquet is the universal hand-off format.

## 9. Domain Asset Layer (external to GTL)

Per `ODD_METHOD.md` §11.1, typed assets must be explicit. Per the research source, GTL is workflow-shaped not record-shaped. The split:

| Concern | Where it lives |
|---|---|
| Trade record shape, units, validation (e.g., `parcel.nominal_size_mt: float`, `calorific_value: kcal/kg GAR`) | TypeScript types + JSON Schema validators |
| Discriminated union over product subtypes (PhysicalCargo, PaperTrade, IRS, Option, ...) | TypeScript discriminated union, exported as the `Trade.product` shape |
| Currency-tagged amounts, unit-tagged quantities | Domain value types (custom or `js-quantities`-style library) |
| Cross-record FK integrity | DuckDB SQL constraints at admission; supplemented by application checks |
| Cross-language interop | Arrow / Parquet on disk |

GTL nodes reference these typed records by name. GTL does not parse or validate them; it carries the payload as opaque-to-graph data with declared input/output asset types per vector.

## 10. ODD Graph Layer (via odd_sdlc substrate)

Per `ODD_METHOD.md` §11.2 and §11.4, the constructive carrier is graph functions in a published GTL module. The product publishes:

```text
Module: post_trade_attribution

CandidateFamily<Trade.product> = { PhysicalCargo, PaperTrade, ... }
CandidateFamily<Overlay> = {
  // generic, inherited from base
  ConvexityOverlay, CrossGreekOverlay, ShapleyOverlay,
  HedgeEffectivenessOverlay, LifecycleOverlay,
  // coal-specific extensions
  QualityDifferentialOverlay, FreightBasisOverlay,
  DemurrageOverlay, PricingPeriodOverlay
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

Role: pnl_control, model_risk_reviewer, desk_head
  # F_H reviewer role binding for s4
```

The runtime path is F_D end-to-end. F_P enters only at S3. F_H only at S4. Every stage output is admitted as an event in the ABG event log; the attribution rows table is a projection over admitted truth.

Per the architecture feedback in odd_sdlc (`feedback_fp_fd_probabilistic_architecture`): F_P exists because F_D can't handle ambiguity. The cascade resolves ambiguity by F_P+F_H ratifying a rule once; F_D handles the class deterministically forever after. The S4 → rule store feedback loop is the eventual-consistency steel thread.

## 11. Asset-Class Generalization

Coal is the first instance. The architecture generalizes through four named extension points (GTL-native, requires no cascade rewrite):

| Extension point | GTL mechanism |
|---|---|
| Product subtypes | `CandidateFamily<Trade.product>` |
| Overlay family members at S2 | `CandidateFamily<Overlay>` |
| RiskFactor kinds | Discriminated union in the typed schema layer |
| S3 hypothesis space | Per-Module configuration |

Future asset-class Modules (oil_trading, power_trading, rates_trading, equity_derivatives, credit_trading) compose by adding members to these extension points without modifying base cascade graph functions or runtime. v0.1 closes when one asset-class Module (coal) is end-to-end functional and the second-instance scaffolding is demonstrably parallel.

## 12. Initial Requirement Family Skeleton

To be elaborated under `specification/requirements/` once this bootstrap is ratified. The seed families:

| Family | Working title | Scope |
|---|---|---|
| REQ-F-OTE-001..n | Canonical trade representation | Typed records for Trade / Product subtypes / Desk / Counterparty / RiskFactor; mapping admission from upstream T&R feed |
| REQ-F-OTE-010..n | Snapshot-pinned replay | Every record carries snapshot_id; replay at any (date, snapshot_id, rule_version) produces identical output |
| REQ-F-OTE-020..n | Cascade F_D admission | S1 universal checks; arithmetic identities; reconciliation tolerances; coverage-gap detection |
| REQ-F-OTE-030..n | Cascade F_D model overlays | CandidateFamily<Overlay> at S2; each overlay deterministic; residual reduction additive |
| REQ-F-OTE-040..n | F_P probability synthesis at S3 | Candidate-evidence framing; explicit confidence intervals; declared synthesis_method; never independent closure authority |
| REQ-F-OTE-050..n | F_H human ratification at S4 | Reviewer interface contract; remediation action taxonomy; eventual-consistency rule store update |
| REQ-F-OTE-060..n | Asset-class Module extension | Four extension points (Trade.product, Overlay, RiskFactor, S3 hypothesis space) admitted as the only sanctioned generalization mechanism |
| REQ-F-OTE-070..n | Reproducibility and audit | Every breach explanation chain (S1 → S2 → S3 → S4) reconstructable from admitted events |
| REQ-F-OTE-080..n | Coal domain coverage | Thermal + metallurgical product subtypes; quality / freight / pricing-period / demurrage overlays; benchmark + quality differential + freight + FX factor coverage |

These are placeholder family names; ratified IDs will be assigned during the first odd_sdlc traversal that derives `specification/requirements/` from this bootstrap.

## 13. Reference Material

| Document | Role |
|---|---|
| `.ai-workspace/comments/claude/20260519T230000Z_RESEARCH_pnl_explain_forensic_attribution.md` | Research seed — landscape, coal data model, cascade architecture, generalization pattern, implementation stack. Imported from `odd_sdlc/.ai-workspace/comments/claude/` (same filename); treat as commentary, not law. |
| `specification_methodology/specification/standards/SPEC_METHOD.md` | Constitutional method governing all change |
| `specification_methodology/specification/standards/ODD_METHOD.md` | Graph-native product-construction law |
| `specification_methodology/specification/standards/TICKET_METHOD.md` | Change admission mechanics |
| `specification_methodology/specification/standards/DESIGN_MODULE_METHOD.md` | Design decomposition law |
| `specification_methodology/specification/standards/UX_METHOD.md` | If/when the reviewer UI for S4 is built |
| `odd_sdlc/specification/requirements/16-edge-gain-closure-contract.md` | Closure law inherited from substrate |
| `odd_sdlc/specification/requirements/18-typed-construction-algebra.md` | Typed F_P stage carrier inheritance |
| `odd_sdlc/.ai-workspace/tickets/active/T-172-realize-staged-disambiguation-graph-and-decomposition-admission.md` | Staged decomposition inheritance |
| `odd_sdlc/.ai-workspace/tickets/active/T-173-realize-complexity-admitted-min-fp-traversal-selection.md` | Complexity-admitted traversal selection inheritance |

## 14. Reading Order for the Operator

When an agent or operator picks up this project for the first time:

1. Read this `BOOTSTRAP.md` (this document) for the seed shape and method inheritance
2. Read `SPEC_METHOD.md` and `ODD_METHOD.md` for the constitutional and graph-native method law
3. Read the research source for landscape understanding of the domain
4. Read the four referenced odd_sdlc surfaces for the substrate's typed-stage construction algebra, edge accounting, complexity-admitted traversal selection
5. Install the odd_sdlc TypeScript tenant into this workspace. The installer materializes a single `.abiogenesis/` root containing the CLI runtime, the installed methodology library at `.abiogenesis/docs/standards/`, the odd_sdlc tenant at `.abiogenesis/odd_sdlc/typescript/`, and the install manifests (`install-manifest.json`, `install-provenance.json`, `typescript-installer-manifest.json`)
6. Initiate a traversal that derives `INTENT.md`, `PRODUCT.md`, `GOALS.md`, and an initial `specification/requirements/` family from this bootstrap
7. Begin v0.1 work-wave under ratified INTENT/PRODUCT/GOALS

## 15. Bootstrap Closure Law

This document is **provisional** — the bootstrap seed, not the ratified constitutional surface.

This document is **closed** and becomes provenance-only when all of the following are true:

- `specification/INTENT.md` exists, derived from §5 above, and is ratified
- `specification/PRODUCT.md` exists, derived from §6, and is ratified
- `specification/GOALS.md` exists, derived from §7, and is ratified
- `specification/requirements/` contains the families from §12 (with concrete IDs and ACs) and they are ratified
- `build_tenants/typescript/` exists with a minimal scaffold per §8
- `.abiogenesis/` install root is materialized with the active odd_sdlc TypeScript tenant under `.abiogenesis/odd_sdlc/typescript/`, the installed methodology library under `.abiogenesis/docs/standards/`, and consistent `install-manifest.json` / `install-provenance.json` / `typescript-installer-manifest.json`
- The first traversal pass under odd_sdlc produces an admitted attribution run for one synthetic coal trade

Until all those conditions hold, this BOOTSTRAP.md is the authoritative seed for the project's constitutional surface. After they hold, this file moves to provenance: future readers see it as the seed that produced the live constitutional chain.

## 16. Non-Closure Conditions

This bootstrap cannot be closed if any of the following are true:

- domain data records are encoded *inside* GTL nodes rather than referenced from external typed schemas
- any cascade stage runtime path consults F_P or F_H during per-row attribution
- prompt text carries domain content that should be admitted record state
- attribution row computation is non-replayable from admitted events + snapshot + rule_store at a timestamp
- coal-specific overlay or risk factor names leak into the base cascade graph functions or runtime
- the S4 → rule store feedback loop is implemented as a side channel rather than as admitted events with versioned rule effective dates
- the project depends on Spark, ClickHouse, or distributed compute in v0.1 without declared scale evidence

## 17. Inheritance and Substrate Update Policy

When `odd_sdlc` releases a new version, this project may upgrade by:

1. Reviewing the substrate's changelog for changes to the cascade architecture, typed F_P stage carriers, edge accounting, or complexity-admitted traversal selection
2. Re-running the odd_sdlc installer; the new release re-materializes `.abiogenesis/` with refreshed `cli-runtime.mjs`, `docs/standards/`, `odd_sdlc/typescript/`, and install manifests in place
3. Running deterministic replay against a pinned historical attribution snapshot to confirm zero behavioral drift on existing data
4. Repricing any local design or requirement surface whose dependency on substrate behavior has changed

The project does not vendor or fork the substrate. It depends on the released line and re-installs forward.

## 18. Open Questions to Resolve in the First Traversal Pass

Carried forward from the research source's "Direction" section; to be answered during INTENT/PRODUCT ratification:

1. **Regulatory scope** — internal forensic only, or FRTB PLA test compliance? Affects HPL/RTPL strictness and PLA-test statistical infrastructure.
2. **Greek provenance** — consumed from upstream T&R system, or computed locally? Affects whether pricer integration is in scope.
3. **Breach granularity** — trade-level, leg-level, factor-level, desk-level? Determines storage shape and explain pivot.
4. **Multi-asset onboarding sequence after coal** — oil/gas next? power? rates? The choice determines which extension points get exercised first.
5. **Reviewer UI scope** — web app, terminal-based, notebook-embedded? Affects build tenant scope.
6. **S3 synthesis method choice** — Bayesian network (interpretable, requires prior), learned classifier (good with labeled data, opaque), agent reasoning (LLM, requires careful evidence-chain audit)? Affects external dependencies.

These open questions are not closure blockers for this bootstrap. They are blockers for the next-layer ratification of INTENT.md and PRODUCT.md.

---

**End of bootstrap.** Treat this document as constitutional seed only. All authority flows downstream from the ratified `INTENT.md`, `PRODUCT.md`, `GOALS.md`, and `specification/requirements/` derived from it.
