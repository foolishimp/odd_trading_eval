# Review: Design State, Deeper Reflection, and Proposed Scaling Architecture

**Date**: 2026-05-23
**Author**: claude
**Status**: Design commentary / candidate evidence. Not ratified. Synthesizes:
- `specification/BOOTSTRAP.md` (provisional constitutional seed)
- `.ai-workspace/comments/claude/20260519T230000Z_RESEARCH_pnl_explain_forensic_attribution.md` (research seed)
- `.ai-workspace/comments/claude/20260523T144237Z_DESIGN_eval_authoring_to_fd_compilation_pipeline.md` (Eval pipeline ADR)
- `.ai-workspace/comments/claude/20260523T151425Z_DESIGN_eval_suite_commodities_and_coal.md` (Eval catalog + two-part report commitment)

**Scope**: Three-part deliverable. §§1-3 review what exists, what is solid, what is hand-waved. §4 reflects on the problem under the surface. §§5-6 propose a scaling architecture and a sequence to v0.1 closure. §7 records where I would push back on my own prior proposals if asked.

---

## 1. State of the Constitutional Surface

`BOOTSTRAP.md` is the only constitutional artifact in `specification/`. It is explicitly **provisional** per its §15 closure law. None of the closure conditions are satisfied:

| §15 closure condition | State |
|---|---|
| `specification/INTENT.md` derived and ratified | Absent |
| `specification/PRODUCT.md` derived and ratified | Absent |
| `specification/GOALS.md` derived and ratified | Absent |
| `specification/requirements/` populated and ratified | Absent |
| `build_tenants/typescript/` scaffolded | Absent |
| `.abiogenesis/` install root materialized | Absent |
| First admitted attribution run for one synthetic coal trade | Absent |

The seven open questions in BOOTSTRAP §18 (regulatory scope, greek provenance, breach granularity, asset-class onboarding sequence, reviewer UI scope, S3 synthesis method, threshold sources) remain unanswered. None of them are blocking BOOTSTRAP itself, but they block the next-layer ratification of `INTENT.md` and `PRODUCT.md`.

`coal_trading` and `attribution_base` Modules referenced throughout the design commentary do not exist as ratified `Module` declarations — they exist only as conceptual anchors in the proposed Eval catalog.

The four `.ai-workspace/comments/claude/` documents (three I have authored, one carried forward as research) are commentary, not law. They propose. They do not bind.

**The honest state**: a thorough seed bootstrap, a thorough research grounding, and three converging proposal threads. Zero ratified surface beyond BOOTSTRAP itself.

## 2. State of the Realization Plan

The realization plan in commentary is the union of:

### 2.1 Eval pipeline architecture (`20260523T144237Z_DESIGN_eval_authoring_to_fd_compilation_pipeline.md`)

- Eval as spec-layer artifact under `specification/evals/<module>/<eval_id>/` (per-Eval directory: `frontmatter.yaml` + `predicate.md` + `commentary_template.md`)
- odd_sdlc traversal repurposed as a deterministic compiler with seven stages (INTAKE → VALIDATE → LOWER → OPTIMIZE → EMIT → ATTEST → BIND)
- Compile-target-pluggable (`duckdb-sql`, `polars-python`, `dask-python`, `spark-python`) behind a common Eval IR
- Three pre-decisions locked: GainSurface → S3 hypothesis space only; Eval rejected as runtime Markov object; single-node-first bound from BOOTSTRAP §8.4 holds at the project level
- Two-part report inherent at every stage (Part A structured payload → Part B rendered commentary)

### 2.2 Eval catalog (`20260523T151425Z_DESIGN_eval_suite_commodities_and_coal.md`)

- 27 cross-asset Evals in `attribution_base` Module (20 S1 + 7 S2 + 3 S3 + 1 S4)
- 30 coal-specific Evals in `coal_trading` Module (15 S1 + 12 S2 + 2 S3 + 3 S4 role bindings)
- Two-axis (`window` × `scope`) categorization + per-Module `volume_band` selector for compile-target picking
- `trade_lifecycle/trade_id=<id>/*.parquet` materialised view identified for the five `trade_lifetime` Evals (worst-case for date-partitioned attribution rows)
- Three full-YAML examples with commentary templates; one concrete rendered prose worked example

### 2.3 Substrate inheritance

- `odd_sdlc` v3.7.1-rc.4+ as construction substrate (BOOTSTRAP §3)
- F_D/F_P/F_H regime split inherited (`LLM_GTL_APP_BUILDER_GUIDE.md` §"F_P / F_D constitutional boundary")
- Cascade S1 → S2 → S3 → S4 with rule-store feedback loop (BOOTSTRAP §10; research source Appendix B)
- Single-node first up to ~100GB/day per BOOTSTRAP §8.4

### 2.4 Coverage

The four converging threads do cover:

- The cascade architecture (substrate-inherited, well-grounded in the research source)
- The Eval authoring shape (proposed, internally coherent)
- A worked Eval catalog (proposed, ~60 entries, asset-class generalization pattern demonstrated)
- The commentary commitment (proposed, with four compile-enforced invariants)

The four threads do not cover, even at the candidate-evidence level, the items in §3.2 below.

## 3. What Is Solid, What Is Hand-Waved

### 3.1 Solid

The following are sufficiently grounded that v0.1 work can proceed against them without further design before authoring:

- **Cascade architecture** (BOOTSTRAP §10, research Appendix B). The F_D→F_P→F_H staging is a known industry pattern with explicit mechanics. Substrate provides the regime carriers.
- **Coal domain model** (BOOTSTRAP §9, research Appendix A). Schemas are complete enough to declare in TypeScript types and map from a T&R feed.
- **F_D/F_P/F_H regime separation**. Substrate-owned (`LLM_GTL_APP_BUILDER_GUIDE.md`). The boundary discipline is encoded as the recurring B-class bug list — applies directly here.
- **Two-part report commitment**. The architectural inversion (system writes commentary, humans ratify) is clean, replay-safe, and compile-time enforceable. Load-bearing for analyst throughput.
- **Per-Module volume_band → compile-target picking**. Preserves the §8.4 bound at the project level while admitting per-Module scale escalation through declared evidence rather than blanket re-design.

### 3.2 Hand-waved (load-bearing gaps)

The following are gaps in the current design. Each one is load-bearing for some part of v0.1; each one is currently unaddressed beyond mention.

#### Gap A: Threshold registry authoring and versioning

All 35 S1 threshold Evals reference `threshold_registry[desk_id, ...].value at effective_date`. The registry's schema, authoring path, ratification model, versioning, and reviewer ownership are unspecified. Without this, no S1 threshold Eval can fire against ratified state.

The decision is more than a schema. It includes: who authors thresholds (desk head, risk policy, both?), how thresholds get promoted from candidate to ratified, how the registry version interacts with Eval version, and what happens when desks change mandate (which is constant in practice).

#### Gap B: Upstream T&R mapping contract

BOOTSTRAP §6.1 declares the system "consumes admitted state from upstream systems". The entire data ingress is then unspecified: mapping language, F_D vs F_P split at the mapping boundary, source-system reconciliation contract (`EVAL-BASE-015` references this without an admission contract existing), snapshot semantics across systems (T&R EOD vs market data EOD vs greeks-computed-at differ in practice). Mapping is the place where the constitutional `WHAT` first contacts external `HOW` — getting it wrong leaks downstream into every Eval.

#### Gap C: GainSurface declaration surface

The catalog references `desk_mandate://${desk_id}/active` (eval `EVAL-BASE-202`). Where mandates live, who authors them, how they version, and what their structured form is (constraint set, weighted-sum scalarization, Pareto vertices) is open. Carrying the open question forward from BOOTSTRAP §18.

Note this gap is not blocking S1/S2 (no Eval at S1/S2 references GainSurface) but it is blocking S3 hypothesis-space evals from running against ratified state.

#### Gap D: Run boundary semantics

The cascade is acyclic per run, cyclic across runs (the steel-thread loop in BOOTSTRAP §10). The run boundary itself — what triggers a run, what its idempotency contract is, what happens when S4 ratification lands mid-run — is unspecified. The §5 scaling architecture below assumes EOD batch as the recommendation, but ratification is still required.

#### Gap E: Reviewer UI for S4

BOOTSTRAP §6.3 requires "Reviewer UI minimum — capture S4 ratification". `EVAL-COAL-301/302/303` declare reviewer roles. The actual surface (web app, terminal, notebook-embedded) is open question 5 from BOOTSTRAP §18. The mechanics — how the reviewer sees Part (A) + Part (B), where the input field is, how the appended human commentary gets admitted, how the remediation taxonomy is editable — are unspecified.

#### Gap F: Shadow-eval promotion path (the institutional-knowledge accumulator)

The eventual-consistency steel thread is the architecture's soul (BOOTSTRAP §10): F_P+F_H ratify ambiguity once → new rule admitted → F_D handles the class forever. The mechanic by which an F_P-suggested overlay becomes a ratified S2 overlay is currently nowhere. This is the load-bearing operator-facing property — without it, the architecture is a one-time setup that decays as new patterns emerge.

The honest framing: this is the single hardest design problem in the project. See §4.3 below.

#### Gap G: GTL graph function publication

BOOTSTRAP §10 sketches `GraphFunction<s1_broad_breach_detection>` etc., but the actual GTL module declaration in the typescript tenant is not authored. The compiler in the Eval pipeline ADR emits modules that bind into this — but the binding surface is hypothetical. Until the GTL module is published, the compiler output has nowhere to land.

#### Gap H: Trade-lifecycle materialised view mechanics

Identified as needed for `trade_lifetime` window Evals (eval suite doc §7). How the view is rebuilt (event-sourced from admitted lifecycle events?), what its rebuild cost vs lookup savings is, how it stays consistent with date-partitioned `attribution_rows` when events arrive out-of-order — all unspecified. This is the v0.1 storage addition that the five identified Evals need.

#### Gap I: Eval IR

The pipeline ADR commits to a compile-target-pluggable architecture behind a common Eval IR but the IR itself is not specified. Defining four backends without a settled IR risks each backend developing its own dialect with the IR retrofitted. The IR is the contract; settle it before authoring backends.

#### Gap J: F_P inference budget mechanism

The cascade routes uncertain S2 outputs to S3 (BOOTSTRAP §10) but the budget for S3 inference is not bounded anywhere. At 100k rows/day with 0.1% escalation, S3 fires 100 times — cheap. At 5M rows/day with the same rate, S3 fires 5000 times — and a Bayesian net or LLM call per firing is real money plus real latency. The architecture needs a declared budget mechanism.

### 3.3 The honest gap count

Ten substantial gaps. They are not all the same weight:

- A, B, D, E are **operator-facing prerequisites** — without them v0.1 cannot ship to a real desk
- C, F, J are **architectural completeness** — without them the cascade's higher stages are skeletal
- G, H, I are **realization-layer infrastructure** — without them the candidate compiler cannot bind to anything

Closing all ten is the work between "candidate evidence" and "v0.1 ratified". The §6 sequence below proposes an order.

## 4. Deeper Reflection on the Problem

Stepping back from the artifact stack.

### 4.1 What is actually being built

The surface goal — post-trade P&L attribution and forensic breach analysis — is well-defined and has commercial precedent (Murex PLA, Bloomberg MARS PnL, Calypso PL Explain). The deeper goal is structurally distinct from any commercial system in this space:

**Convert ambiguous, narrative-heavy analyst work into governed, replayable, deterministic-first traversal without losing the semantic depth that makes the analysis valuable.**

Commercial systems do attribution arithmetic and rule-based breach detection. They do not do governed institutional-knowledge accumulation. The system being designed here does both — and it inherits its governance from a substrate (odd_sdlc) that was built for exactly the same kind of conversion in a different domain (software work).

This is structurally identical to what odd_sdlc does:

| odd_sdlc | odd_trading_eval |
|---|---|
| Spec → design → code → proof | Trade → attribution → breach → forensic explanation |
| F_D mechanics; F_P semantic judgment; F_H ratification | Same |
| Cascade with eventual deterministic accumulation | Same |
| Ambiguous human work converted to governed traversal | Same |
| Domain: software construction | Domain: financial analysis |

The architectural bet is that the same governance pattern works for both. The bet is sound — the substrate's regime separation is domain-agnostic by construction. The risk is that domain-specific frictions (a desk head's tolerance for paperwork before a new overlay gets bound; a quant's expectation of notebook-driven exploration) require different operator UX than what works in the software-construction case.

### 4.2 The exploitable asymmetry

The single most important fact about post-trade P&L attribution is this: **most P&L is mechanical; the forensic-interesting cases are a tiny tail.**

Daily attribution for a coal book breaks into theta + delta + gamma + vega + carry + lifecycle + quality + freight + a few coal-specific buckets + residual. On most days for most trades, the buckets close the P&L within tolerance. The residual is small or zero. There is nothing to explain.

The cases that need forensic work are the days when the residual is large, or when a known overlay over-explains (covering a gap the model should not have), or when a lifecycle event creates a step change no greek captures, or when a pricing-period basis blew up. These are the cases that matter — they are also the cases that are rare.

The compute curve is therefore power-law:

| Stage | Firing rate (rough) | Per-firing cost |
|---|---|---|
| S1 (universal mechanical) | every (date × trade × leg × factor) row | sub-ms; pure SQL |
| S2 (overlay decomposition) | ~5-15% of rows produce a BreachCandidate | low-ms; SQL + simple math |
| S3 (probabilistic synthesis) | ~0.1% of rows escalate | hundreds-of-ms; inference cost |
| S4 (human ratification) | ~0.01% escalate further | minutes-hours; human time |

The architecture should not treat these uniformly. Specifically:

- S1/S2 should scale **horizontally with data volume** — embarrassingly parallel, columnar, single-node-up-to-cluster
- S3 should scale **with quality of inference**, not volume — budget-bounded per run, per Module, per asset class
- S4 should scale **with reviewer throughput** — rate-limited; this is the queue everything backs up into

The current pipeline ADR treats S1, S2, and S3 with the same compile-target picker (`duckdb-sql`, `polars-python`, etc.). That is wrong. S1/S2 are about scanning data efficiently; S3 is about running inference under a budget. Different problem, different solution. The §5 scaling architecture below splits these explicitly.

### 4.3 The institutional-knowledge accumulator (Gap F, expanded)

This is the load-bearing operator-facing mechanism and the least specified piece of the design. Worth thinking through carefully.

A new pattern shows up in coal trading. For example: "Indonesian thermal cargoes routinely underreport sulfur by 0.2% because the loading certificate uses ADB basis while the price formula uses NAR basis."

Today, an analyst notices this after the fact, fixes the price formula on the next trade, tells colleagues, maybe writes a runbook. No system change. The knowledge degrades over a year as people leave the desk.

In odd_trading_eval, the lifecycle should be:

1. S3 hypothesis ranking shows recurring `quality_underestimate` for Indonesian thermal in the last N runs
2. S4 reviewer ratifies: cause = `basis_mismatch_adb_vs_nar`, remediation = `extend_overlay_coverage`
3. Ratification admitted as event
4. A new S2 overlay (`IndonesianThermalBasisOverlay`?) drafted — by whom?
5. New overlay goes through TICKET_METHOD ratification
6. New overlay compiled, bound, deployed
7. Next run's S2 catches the pattern in F_D

The architecture's spirit collides with operator reality at steps 4-5. A desk does not want a TICKET_METHOD ratification with code review on every new overlay. A new overlay is a specific quantitative pattern — it is closer to "tune a parameter" than "ship a feature". But the architecture requires governance.

The resolution is not "lower the bar on TICKET_METHOD". The resolution is to introduce a **shadow status** for candidate Evals, with structural rules for promotion:

- Author drafts a candidate Eval (status: `candidate`)
- Candidate Eval runs against admitted data in shadow mode — produces shadow BreachCandidate / ResolvedBreach events tagged as candidate, **not bound to the cascade**, **not visible to reviewers in S4 routing**
- After N successful shadow closures (e.g., N=20 over M=10 business days where the shadow eval's resolution agrees with the eventual S4 ratification of the underlying breach), TICKET_METHOD admits the candidate to `ratified`
- Promotion is itself an admitted event with the shadow evidence as authority

This makes the eventual-consistency steel thread operational. The friction is real (a new overlay does not bind on day 1; it bakes for ~10-20 days against real data) but the friction is the price of governance.

This whole mechanism is currently nowhere in the design. It is candidate evidence for the most important operator-facing property of the architecture. **Of all the gaps, this is the one most worth fully specifying first** — because it determines whether the system accumulates institutional knowledge or atrophies as new patterns out-pace the ratification cadence.

### 4.4 The three-way tension and its reconciliation

The system has to satisfy three constraints that pull in different directions:

| Constraint | Wants |
|---|---|
| **Replayable** | Frozen state, immutable versions, content-addressed everything |
| **Tractable** | Speed, exploration, immediate feedback loops, ad-hoc queries |
| **Governed** | Every change passing through TICKET_METHOD admission, audit trails, named authority |

The reconciliations:

- **Replayable × Governed** — event-sourced versioning. Change is always allowed, always governed, always frozen-once-admitted. The two-part report with versioned commentary templates is the example: commentary changes are version bumps, historical replay binds to the historical version, all transitions admitted as events.

- **Tractable × Governed** — compile-time governance, runtime ergonomics. Governance happens at compile time (Eval ratification, threshold registry admission, commentary purity check); analyst queries hit cached compiled artifacts. The notebook-driven analyst workbench (§5.2 below) is where ergonomics lives; the runtime cascade is where governance lives.

- **Replayable × Tractable** — the candidate/ratified split. Candidate Evals run in shadow mode against admitted data without admission to the cascade — analysts get exploration speed; the audit trail stays clean because nothing is bound until ratified. The shadow→ratified promotion path from §4.3 above is this reconciliation made operational.

These reconciliations are not novel. They are the working principles of every governed-but-iterable analytical system. The architecture proposed here applies them deliberately — and the proposal documents commit to them at the structural level (commentary versioning, two-part reports, the shadow-status pattern).

### 4.5 Where this is genuinely new

For all the inheritance from odd_sdlc, three properties of this domain are genuinely new and not addressed by the substrate:

1. **Per-row scale forbids per-row F_P.** odd_sdlc operates at ticket scale (1-100 tickets/day per workspace); odd_trading_eval operates at attribution-row scale (100k-10M rows/day). The per-firing cost economics differ by 4-6 orders of magnitude. odd_sdlc can afford F_P workers to draft narrative artifacts (requirements, design modules, test cases — all narrative, all generated through F_P semantic construction under F_D gating). odd_trading_eval cannot. Every per-row artifact, including the commentary that humans read, must be F_D-templated. This is the load-bearing scale invariant that propagates through the entire architecture — see §4.5.1 below.

2. **Domain math is exactly deterministic.** Greeks × moves + lifecycle is closed-form. Most P&L attribution does not need ambiguity at all. This is unusual in agentic-system applications; the substrate was designed for cases where ambiguity is dense. Here ambiguity is sparse, and the architecture should bet on that.

3. **Time-pinned snapshot replay.** Every record carries a snapshot_id; replay binds to `(snapshot_id, rule_version)`, not just to event causation. odd_sdlc's replay model is event-causation traversal; odd_trading_eval needs snapshot-anchored replay because the underlying market data is the load-bearing input that mutates outside the system. This is genuinely new substrate work — see Gap H (trade-lifecycle materialised view) and the data-ingress contract in Gap B for where this lands.

The institutional-knowledge accumulator (the shadow→ratified overlay catalog growing monotonically as patterns emerge) is *not* on this list because it is structurally a candidate-family extension pattern that the substrate's `CandidateFamily<Overlay>` mechanism is built for. What is new about it is the operational lifecycle around promotion (§4.3) — but the lifecycle is implementation, not new ontology.

The narrative-rendering pattern is *not* on this list either. odd_sdlc spans spec → requirements → design → test cases → code; the first four are narrative. What differs in odd_trading_eval is the regime of the renderer — F_D templates, not F_P workers — and the difference is forced by item (1) above, not by the narrative property itself.

#### 4.5.1 Commentary regime invariant

The two-part report commitment from `[[20260523T151425Z_DESIGN_eval_suite_commodities_and_coal]]` §2.1 carries an invariant worth naming explicitly:

> **For every Eval at every stage, the commentary renderer is F_D.** It is a pure deterministic function over the admitted structured payload. The template may render F_P-produced structured evidence (at S3, with explicit "candidate evidence" framing and confidence intervals) and may include F_H-written narrative (appended at S4 as the human's ratification note). The *rendering mechanic* is always F_D template execution. There is no path in the runtime cascade where commentary requires F_P inference per row.

The invariant exists because per-row F_P is economically and operationally impossible at attribution scale (item 1 above). A 5M-rows/day desk firing 5,000 S3 escalations with F_P per-firing inference is already a $50-100/day inference bill; if commentary also required F_P per row, it would be a $5,000-50,000/day bill — defeating the architecture.

**Cross-firing synthesis is a separate question.** Statements like "this is the third Indonesian thermal quality breach this week — consider extending overlay coverage" are genuinely F_P and genuinely valuable. They live in Tier 2 (analyst notebook), are bounded by analyst query rate, and are never on the per-row cascade path. They appear in the system as *analyst-initiated synthesis queries* over admitted breach history, not as fields in any per-firing commentary template. The two-tier deployment in §5.2 is what makes this separation operational.

Implication for prior commentary in this thread: where I described S3 commentary as "F_P commentary" in `[[20260523T151425Z_DESIGN_eval_suite_commodities_and_coal]]` §2.1 and `[[20260523T144237Z_DESIGN_eval_authoring_to_fd_compilation_pipeline]]` §3.4, the phrasing was loose. The correct formulation: the S3 commentary template is an F_D renderer over an F_P-produced HypothesisRanking payload. The template's *language* matches the F_P regime of the underlying evidence ("candidate evidence", confidence intervals, never closure), but the *mechanic* of rendering is F_D. Both surfaces are corrected in the propagation below.

These three (per-row scale, deterministic domain math, time-pinned snapshot replay) are why this is a real product, not a reskin of odd_sdlc. They are also why the v0.1 closure conditions are reachable on coal alone — the patterns to encode are bounded and well-known. Future asset classes earn their patterns through the same shadow→ratified loop.

## 5. Proposed Scaling Architecture

A scaling design has to address what actually scales. Five orthogonal axes; each gets its own technique.

### 5.1 The five orthogonal scaling axes

| Axis | Range | Driver |
|---|---|---|
| **(1) Rows per day** | 100k → 10M+ | trade volume × factor coverage × position count |
| **(2) Eval catalog size** | 50 → 5,000 | number of patterns encoded across all Modules |
| **(3) Module count** | 1 (coal) → 10+ | number of asset-class Modules onboarded |
| **(4) Replay depth** | 1y → 7y+ | regulatory retention, forensic audit horizon |
| **(5) Analyst concurrency** | 5 → 500 | size of the analytical user base |

These do not scale together. A power desk might be 5M rows/day but one Module. A multi-asset trading house might be 1M rows/day total but 8 Modules. A regulator using the system for audit cares about (4) but not (5). Coal v0.1 sits at low ends of all five.

### 5.2 Two-tier deployment

The architecture splits into two tiers with different scaling profiles. This split is implicit in the research source's Appendix D; making it explicit is the v0.1 architectural commitment.

#### Tier 1 — Runtime cascade (production-critical)

| Property | Value |
|---|---|
| Trigger | EOD batch (recommendation; see §5.5 settlement item 1) |
| Deployment | One process per Module (or one process running multiple Modules) |
| Engine | Embedded DuckDB + Postgres + Parquet snapshot store |
| F_D path | S1 + S2 compiled to `duckdb-sql` or `polars-python` per Module's `volume_band` |
| F_P path | S3 bounded by declared inference budget (§5.5 settlement item 2) |
| F_H path | S4 ratifications rate-limited by reviewer queue throughput |
| Output | BreachCandidate / ResolvedBreach / HypothesisRanking / RatifiedExplanation admitted; rendered commentary admitted alongside; both indexed by `(eval_id, version, snapshot_id)` |

Tier 1 is where the SLA lives. It must be deterministic, replayable, governed. It runs once per Module per business day. It does not need to be fast in absolute terms — it needs to be done before the next business day opens.

#### Tier 2 — Analyst workbench (exploratory)

| Property | Value |
|---|---|
| Trigger | Analyst-initiated query |
| Deployment | Shared-nothing: each analyst gets their own DuckDB-on-Parquet, reading from the shared snapshot store |
| Engine | Python + DuckDB + Polars + Plotly in Marimo/Jupyter notebooks |
| Read scope | Same admitted-event log and Parquet snapshot store as Tier 1 |
| Shadow runs | Candidate Evals run against admitted data without binding to the cascade |
| Promotion path | Shadow→ratified per §4.3 above; admission through TICKET_METHOD |

Tier 2 is where ergonomics lives. It must be fast, explorable, low-friction. It is intentionally outside the SLA — an analyst can crash their own notebook without affecting production.

The two tiers share *one* substrate: the immutable admitted-event log and the Parquet snapshot store. They never share compute. They never share writable state. Tier 1 emits events; Tier 2 reads them. Tier 2 produces shadow evidence; ratification through TICKET_METHOD admits selected shadow Evals back into Tier 1's catalog.

This is shared-nothing-by-construction. It scales axis (5) — analyst concurrency — by adding seats. Tier 1's cost is independent of Tier 2's concurrency.

### 5.3 Per-axis scaling techniques

**Axis (1) — Rows per day.** Per-Module `volume_band` declaration picks the compile target. Coal at `low` → `duckdb-sql`. Mid-volume desks at `mid` → `polars-python` over partitioned Parquet. High-volume desks at `high` → ClickHouse or Polars-on-Dask. The compile target is a tenant configuration decision, not an Eval-author concern.

**Axis (2) — Eval catalog size.** Co-compilation in the OPTIMIZE stage of the SDLC compiler. N S1 Evals all scanning attribution_rows for the same (date, desk) fuse into one scan with N predicate branches. A 5,000-Eval catalog compiles to ~50-200 fused scans, not 5,000 separate ones. The compilation itself is parallelizable across Modules — Module is the compilation unit.

**Axis (3) — Module count.** Module as publication boundary (BOOTSTRAP §11 inherits this from `ODD_METHOD.md`). Independent compile, independent deploy, independent `volume_band`, independent reviewer roles. Cross-module sharing is restricted to the shared RiskFactor union (FX, rates, carbon) and the base Module's S1/S2 cascade graph functions. Adding a Module is bounded work.

**Axis (4) — Replay depth.** Date-partitioned Parquet is already the answer. Cold-storage tiering (recent in NVMe, year-old in S3 Glacier) is operationally separate from the runtime. Replay queries hit the same DuckDB/Polars engine with predicate pushdown that prunes cold partitions before they ever transfer. Replay cost is therefore O(matching partitions), not O(history).

**Axis (5) — Analyst concurrency.** Tier 2 shared-nothing: one DuckDB per analyst pulling from shared object storage. Object storage IOPS scales linearly; analytical workloads are read-heavy and cache-friendly. 500 analysts × 10 GB/day each = 5 TB/day read load on S3, which is well within S3's scaling envelope.

### 5.4 Cost envelopes per volume_band

These numbers are coarse cloud-pricing estimates as of late 2025; they are useful for relative shape, not contract drafting.

| volume_band | Rows/day | Tier 1 infra | Tier 2 per-seat | S3 inference | Storage (5y) |
|---|---|---|---|---|---|
| `low` | ≤100k (coal) | 1× 8-vCPU 32GB / ~$200/month | Notebook server / ~$50-150/month/seat | ~$1/day at 0.1% S3 firing | ~$2/month per Module |
| `mid` | 100k-1M | 1× 32-vCPU 128GB / ~$1k/month | Same / +SSD cache | ~$10/day | ~$20/month per Module |
| `high` | 1M-10M | ClickHouse cluster or Polars-on-Dask / ~$2-5k/month | Same / +larger SSD | ~$100/day | ~$200/month per Module |
| `extreme` | >10M | Warehouse (Snowflake / BigQuery / Trino-Iceberg) / variable | Same / warehouse-backed | ~$1k/day | warehouse pricing |

Coal v0.1 runs for roughly the cost of a small SaaS subscription. The architecture earns its breadth one Module at a time; each new Module declares its band and the cost scales accordingly.

### 5.5 Five settlement items to make scaling real

These five decisions convert the scaling architecture from candidate evidence to ratifiable design. They are the highest-leverage open items.

1. **Run boundary semantics.** Recommendation: **EOD batch**. Matches the existing settlement vocabulary (most coal paper contracts settle monthly with daily MTM marks; daily attribution is the natural unit). Matches the snapshot model (one snapshot_id per (date, source_system)). Matches reviewer cadence (S4 reviewers operate on a daily breach queue). Continuous or event-triggered runs are deferred until a use case requires sub-daily attribution — none does in v0.1.

2. **F_P inference budget mechanism.** Per-Module declared `s3_inference_budget` with three components: `max_firings_per_run` (back-pressure threshold), `confidence_floor` (minimum confidence to consider a hypothesis), `escalation_threshold` (residual magnitude that bypasses the budget cap because the breach is too large to defer). When a run exceeds `max_firings_per_run`, S3 degrades — weaker synthesis method, shorter prompts, batch grouping. The degradation is admitted as an event; analysts can see when S3 was running degraded.

3. **Shadow-eval promotion path.** Per §4.3 above. Candidate Evals run in shadow against admitted data, produce tagged shadow events not bound to S4 routing, accumulate evidence over a declared bake-in window (e.g., N=20 successful agreements over M=10 BD), then TICKET_METHOD admits to ratified with the shadow evidence as the admission authority. This is the institutional-knowledge accumulator made operational.

4. **Reviewer UI for v0.1.** Recommendation: **notebook-embedded**. Reviewer opens a Marimo notebook scoped to their reviewer_role. Notebook queries `breach_review_queue` from the runtime via DuckDB; displays each row as [Part A JSON expanded] + [Part B rendered commentary] + [structured input fields for `confirmed_cause`, `remediation_action`, `notes`]; submits via a callback that writes a `RatifiedExplanation` event back. No separate web app. Notebook is the UI. This matches Tier 2 ergonomics, reuses Tier 1 admission, and ships in v0.1 effort.

5. **Module manifest.** Each Module declares (in `build_tenants/typescript/modules/<module>/manifest.yaml`): `volume_band`, optional per-(window,scope) compile-target overrides, runtime resource profile, `s3_inference_budget`, `s3_synthesis_method`, reviewer role bindings, `shadow_eval_bake_in_policy`. The compiler reads this manifest at module compilation; the runtime reads it at startup. Module manifest is the single point where tenant-local realization decisions live (consistent with `feedback_realization_choices_in_tenant_adrs` — tenant-local decisions go in tenant ADRs).

These five settlement items, taken together, close gaps D, E, F, J from §3.2 and make the §5.2 two-tier architecture concrete. They do not close gaps A, B, C, G, H, I — those are addressed by the §6 sequence below.

## 6. Sequence to v0.1 Closure

The BOOTSTRAP §15 closure conditions are the goalpost. No time estimates per project convention; the sequence below is dependency-ordered.

### Step 1 — Ratification traversal #1: derive INTENT, PRODUCT, GOALS

Convert BOOTSTRAP §§5-7 into ratified `INTENT.md`, `PRODUCT.md`, `GOALS.md`. Add the requirement family `REQ-F-OTE-090..n` for Eval authoring & compilation (from the pipeline ADR §5). Ratify the two-part report commitment as part of the requirement family. Answer the seven BOOTSTRAP §18 open questions — at minimum: regulatory scope is "internal forensic only" (v0.1), breach granularity is "(date × trade × leg × factor)", S3 synthesis_method is "Bayesian network for coal v0.1" (per the prior ADR recommendation).

Outcome: constitutional surface is alive. Closes part of BOOTSTRAP §15.

### Step 2 — Threshold registry spec (Gap A)

Author the Postgres schema for `threshold_registry` and `overlay_registry`. Author the authoring contract — who writes a threshold, how a candidate is admitted, how versioning works, how desk mandate changes propagate. This is a `DESIGN_MODULE_METHOD.md`-shaped design surface, not a code task.

Outcome: all 35 S1 threshold Evals have a foundation to reference. Closes Gap A.

### Step 3 — Upstream T&R mapping spec (Gap B)

Author the source-system mapping contract: mapping language (TypeScript types + validators), F_D vs F_P split at the boundary, reconciliation contract for `EVAL-BASE-015`, snapshot semantics across systems. Define the admission contract for canonical Trade / Desk / Counterparty / RiskFactor records from external feeds.

Outcome: the entire data ingress is specified. Closes Gap B.

### Step 4 — GTL module publication (Gap G) and Eval IR specification (Gap I)

Author the `post_trade_attribution` GTL module in the typescript tenant: the four cascade graph functions, the CandidateFamily declarations, the Job binding. Specify the Eval IR — the typed intermediate representation between Eval frontmatter+predicate+commentary and the compile targets.

Outcome: the compiler has a target to bind into; the IR has a contract for backends. Closes Gaps G and I.

### Step 5 — First Eval slice end-to-end

Author and ratify five Evals: `EVAL-BASE-{003,004,005,006,007}` (universal invariants and coverage). Build the minimum compiler: validator + IR + `duckdb-sql` emitter (no optimizer, no Polars target yet). Run the five compiled Evals against one synthetic coal trade per BOOTSTRAP §15 closure. Demonstrate Part (A) structured payload + Part (B) rendered commentary.

Outcome: end-to-end attribution slice for one synthetic coal trade. Closes part of BOOTSTRAP §15.

### Step 6 — Module manifest, reviewer UI, S3 budget (settlement items 5, 4, 2)

Implement the Module manifest format (settlement item 5). Implement the notebook-embedded reviewer UI (settlement item 4). Implement the S3 inference budget mechanism (settlement item 2). Run boundary semantics (settlement item 1) is implemented as part of Step 5's cascade run.

Outcome: settlement items 1, 2, 4, 5 closed. Closes Gaps D, E, J.

### Step 7 — First coal-specific overlay end-to-end

Author and ratify `EVAL-COAL-101` (quality differential). Compile to `duckdb-sql`. Demonstrate S2 closure on a synthetic breach (a coal trade with realized quality below assumed). Render the commentary against a real ResolvedBreach.

Outcome: S2 overlay path proven on coal-specific content. Validates the §11 generalization pattern.

### Step 8 — Shadow eval and promotion path (Gap F, settlement item 3)

Implement candidate-status Eval shadow execution. Implement the shadow→ratified promotion contract (settlement item 3). Demonstrate by authoring a candidate Eval, running it in shadow against admitted historical data, accumulating N=20 successful agreements over M=10 BD synthetic dates, and admitting the promotion through TICKET_METHOD.

Outcome: the institutional-knowledge accumulator is operational. Closes Gap F. **This is the architecturally most-important step after Step 1.**

### Step 9 — First S3 firing and rule-store feedback loop

Construct a synthetic unexplained breach that S1/S2 cannot close. Demonstrate S3 hypothesis ranking via Bayesian-net synthesis. Demonstrate S4 ratification routing through the notebook UI. Demonstrate the resulting RatifiedExplanation admitted as an event. Demonstrate that the next synthetic run with the same pattern closes at S2 because the ratification produced a new overlay (via the Step 8 promotion path).

Outcome: the steel-thread loop demonstrated end-to-end on synthetic data. The architecture's defining property is operational.

### Step 10 — Trade-lifecycle materialised view (Gap H) and GainSurface declaration (Gap C)

Implement the `trade_lifecycle/trade_id=<id>/*.parquet` materialised view as an event-sourced projection (Gap H). Specify the GainSurface declaration surface — recommend: Postgres `desk_mandate` table, structured form is "constraint set + weighted scalarization with per-dimension confidence" (Gap C).

Outcome: `trade_lifetime` Evals can run; S3 GainSurface Evals can reference a real mandate surface. Closes Gaps C and H.

### v0.1 closure

After Step 10, the BOOTSTRAP §15 closure conditions are satisfied. v0.1 is real. The catalog from `20260523T151425Z` (60 Evals) is authored against the established infrastructure over subsequent v0.x work-waves — each one a localized work-wave under GOALS.md, not a new architectural pass.

### v0.2 trajectory and beyond

Per BOOTSTRAP §11, asset-class generalization is the architecture's payoff. v0.2 onboards a second Module (recommend: `freight_trading` because it shares the most RiskFactor types with coal; or `power_trading` if the asset-class diversity is more valuable than infrastructure reuse). Each Module declares its `volume_band`; the compile-target picker handles the rest. The scaling architecture from §5 earns its scope one Module at a time.

## 7. What I Would Push Back On

If I were reviewing the prior three commentary docs as a fresh reader, these are the places I would push:

1. **Seven categorization vectors may be over-specified.** A working catalog might need only `(stage, window, scope, regime, predicate, commentary_template)`. `entities` is derivable from the predicate; `product_category` is derivable from the owning Module; `reasoning_pattern` is descriptive metadata that no compile pass uses. Worth trimming before the schema ossifies.

2. **Compile-target-pluggable is right; building four backends is premature.** Settle the IR (Gap I). Build the `duckdb-sql` backend. Stop. The `polars-python` backend earns its existence when an Eval cannot be expressed in SQL — and the catalog suggests this is ~5-10 Evals, not most of them. `dask-python` and `spark-python` are deferred indefinitely (per BOOTSTRAP §8.4 and the prior ADR).

3. **Per-Eval directory may be authoring friction.** Three files per Eval × 60 Evals = 180 files in v0.1. Consider a single-file shorthand for `status: candidate` Evals that explodes to three-file on ratification. The compiler accepts both. Removes a real friction in the first-pass authoring step (Step 5 above).

4. **GainSurface at S3 is correct; the multi-dimensional surface type is unspecified.** Do not author S3 Evals that reference GainSurface until the surface type is settled (recommend: Step 10 above). `EVAL-BASE-202` and `EVAL-COAL-202` are written against an undefined dependency.

5. **The candidate/ratified split needs operational teeth.** What specifically prevents a candidate Eval from accidentally being bound? The compiler should fail closed on attempting to BIND a candidate Eval to the cascade Job. The runtime should fail closed if a Module manifest references an unratified Eval. These are not free; they need explicit fail-closed gates.

6. **The "eval catalog" as commentary is a draft, not a backlog.** Some of the 60 catalog entries (e.g., `EVAL-COAL-303`, the freight reviewer binding, with no actual reviewer assignment table behind it) are placeholders, not authored Evals. The ratification traversal should split the catalog into authored-and-ratified vs deferred-as-backlog, not treat all 60 as v0.1 deliverables.

7. **Two-part report compile-time purity check is a strong claim.** The §2.1 commitment that templates fail closed at compile time if they reference missing fields requires the structured payload types to be typed precisely. If `BreachCandidate.structured_cause_signals` is typed as `Map<string, any>`, the purity check degrades to substring match. The compile-time check is only as strong as the payload typing. Worth being explicit about that dependency.

## 8. Status

This is commentary. It synthesizes the design state, reflects on the problem, and proposes a scaling architecture and a sequence to v0.1 closure. The five settlement items in §5.5 and the ten-step sequence in §6 are decision inputs to the first ratification traversal pass.

The scaling architecture is intentionally underspecified at `high` and `extreme` volume bands. Coal sits at `low` and earns v0.1 there. Other asset classes earn their bands through declared volume evidence per BOOTSTRAP §8.4 — not by adopting the upper bands prophylactically.

The single most important architectural commitment to lock in is the shadow→ratified promotion path (§4.3, settlement item 3, Step 8). It is the difference between an architecture that accumulates institutional knowledge and one that atrophies. It is also the least specified piece of the current design. Specify it first; the rest follows.
