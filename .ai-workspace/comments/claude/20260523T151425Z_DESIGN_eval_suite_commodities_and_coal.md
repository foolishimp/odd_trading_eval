# Proposed Eval Suite: Cross-Asset Commodities + Coal Specialization

**Date**: 2026-05-23
**Author**: claude
**Status**: Design commentary / candidate evidence. Not ratified. Extends the Eval-authoring proposal in [[20260523T144237Z_DESIGN_eval_authoring_to_fd_compilation_pipeline]] with a concrete catalog of authorable Evals. Each entry below is a candidate Eval — the actual ratified set is decided through the first odd_sdlc traversal pass under TICKET_METHOD.
**Scope**: Two-tier Eval suite. The `attribution_base` Module carries cross-asset Evals (work for any commodity, derivative, or paper desk). The `coal_trading` Module extends with coal-specific Evals. Pattern matches `BOOTSTRAP.md` §11 (asset-class generalization) and Appendix C of the research source.

---

## 1. Position

This document proposes:

- A compact Eval declaration shape suitable for the authoring surface in §3 of the prior ADR
- A starter catalog of cross-asset Evals (attribution_base)
- A coal-specific extension catalog (coal_trading)
- A compile-target distribution that demonstrates the §8.4 single-node-first bound holds for the coal volume band

The Eval catalog is candidate evidence. Final naming, predicate wording, threshold sources, and stage binding are settled through the ratification traversal. The goal here is to make the *shape* and *coverage* of an Eval suite concrete enough to argue about.

## 2. Eval Declaration Shape (Compact)

Each Eval declares the following tuple. Full per-Eval YAML files live under `specification/evals/<module>/` once authored; the catalog below is the compressed form.

```text
EVAL-<MODULE>-<NNN>
  name                : kebab-case short name
  stage               : S1 | S2 | S3 | S4
  regime              : F_D | F_P | F_H
  window              : intraday | single_day | rolling_n_days | multi_day | trade_lifetime | period
  scope               : row | leg | trade | book | desk | portfolio | counterparty
  entities            : [Trade, Leg, Desk, RiskFactor, AttributionRow, ...]
  reasoning_pattern   : threshold | invariant | reconciliation | coverage | delta | ratio
                        | correlation | gain_surface_deviation | explanatory_manifold
  predicate           : one-line description of what must hold
  breach_kind         : kebab-case breach identifier emitted on failure
  compile_target      : duckdb-sql | polars-python | (others by Module volume_band)
  gain_surface_link   : optional ref to a declared GainSurface
  commentary_template : reference to the prose-rendering template (see §2.1)
```

Volume band is declared once per Module (per the prior turn's reshape). Coal sits at `low`, so `coal_trading` and most cross-asset Evals over the coal book compile to `duckdb-sql` by default.

### 2.1 Two-Part Report Structure (Inherent)

Every Eval emits a **two-part report** when it fires. Both parts are inherent — neither is optional, and the second is not an F_P inference step.

| Part | Owner | Authority |
|---|---|---|
| **(A) Eval details** | The Eval's predicate + evidence refs | Authoritative structured output. Becomes the `BreachCandidate` / `ResolvedBreach` / `HypothesisRanking` payload admitted through ABG |
| **(B) Commentary** | The Eval's `commentary_template` rendered against (A) | Read-model projection. Replay-derivable from (A). Carries no facts not present in (A) |

The architectural commitment: **the eval contains the reason for the breach inside its structured output**. The commentary is not a separate F_P pass that synthesizes a story over an opaque breach — it is a mechanical projection of the structured evidence into prose for human consumption. This preserves the F_D-throughout-runtime invariant from `BOOTSTRAP.md` §10: the commentary at S1 and S2 is deterministic; it inherits the regime of its source eval.

Properties:

- **Replay-safe**. Same eval output → same commentary, byte-identical.
- **No phantom facts**. Every claim in the commentary maps 1:1 to a field or evidence ref in the structured eval. If the commentary asserts something the eval doesn't carry, the eval is incomplete and must be enriched — the template must never invent.
- **Regime-faithful** — and the regime applies to *what is rendered*, not to the *renderer*. The renderer is always F_D template execution. F_D Evals (S1/S2) render structured deterministic findings into prose that asserts closure where appropriate. F_P Evals (S3) render F_P-produced HypothesisRanking payloads into prose that uses "candidate evidence" framing and explicit confidence intervals and never asserts closure. F_H ratification (S4) appends the human-written narrative to the rendered S3 base. **The rendering mechanic is F_D at every stage** — there is no runtime path where commentary requires F_P inference per row. See the commentary regime invariant in `[[20260523T154035Z_REVIEW_design_state_and_scaling_architecture]]` §4.5.1 for the load-bearing reason: per-row F_P is economically and operationally impossible at attribution scale.
- **Audit-tractable**. A reviewer reading the commentary can drill back to the structured eval and the underlying admitted events without leaving the artifact.

Commentary templates are themselves spec-layer artifacts, governed alongside the predicate. They live next to the eval file:

```text
specification/evals/<module>/<eval_id>/
├── predicate.md              # the rule body
├── commentary_template.md    # the prose projection
└── frontmatter.yaml          # the tuple from §2 above
```

The SDLC compiler emits both: a runtime F_D predicate function and a runtime commentary renderer. Both are pure functions over admitted state. The renderer takes the structured `BreachCandidate` (or equivalent) and produces a string; it cannot access disk, network, model, or any state not in the admitted payload.

Human-written commentary enters the system only at S4 (`RatifiedExplanation.notes`). At every prior stage, the commentary is generated. This is the load-bearing inversion: humans usually *write* breach reports; here the system writes them, and humans ratify or correct them. The commentary template is the mechanism that makes that inversion lawful.

## 3. attribution_base Module — Cross-Asset Eval Catalog

These Evals work over any asset class. They live in the base Module that `coal_trading`, `oil_trading`, `power_trading`, etc. import. Predicate bodies use generic vocabulary; asset-specific specialization happens through extension Modules.

Every catalog entry below inherits the per-Eval directory shape from §2.1: each `EVAL-BASE-NNN` resolves to `specification/evals/attribution_base/EVAL-BASE-NNN/` with `frontmatter.yaml` + `predicate.md` + `commentary_template.md`. The tables omit the `commentary_template` field because it is universal — every Eval carries one, and the compiler enforces the §2.1 purity invariants on every template.

### 3.1 S1 — F_D universal mechanical Evals

| Eval | Stage | Window | Scope | Reasoning | Predicate | Breach kind |
|---|---|---|---|---|---|---|
| `EVAL-BASE-001` | S1 | single_day | desk | threshold | sum(desk_pnl) ≥ desk.daily_stop_loss limit | `desk_stop_loss_exceeded` |
| `EVAL-BASE-002` | S1 | single_day | book | threshold | sum(book_pnl) within (low_limit, high_limit) | `book_pnl_threshold_breach` |
| `EVAL-BASE-003` | S1 | single_day | trade | invariant | sum(attribution_buckets) ≈ total_pnl within ε | `attribution_arithmetic_identity_breach` |
| `EVAL-BASE-004` | S1 | single_day | trade | reconciliation | abs(APL − HPL − new_trades − fees) < ε | `apl_hpl_drift` |
| `EVAL-BASE-005` | S1 | single_day | row | invariant | every AttributionRow has resolvable trade_id, desk_id, factor_id | `attribution_row_fk_integrity_breach` |
| `EVAL-BASE-006` | S1 | single_day | trade | coverage | every position with non-trivial factor exposure has at least one greek | `risk_factor_coverage_gap` |
| `EVAL-BASE-007` | S1 | single_day | trade | invariant | greek timestamp ≤ EOD-1 business day (no staleness > one day) | `greek_staleness_breach` |
| `EVAL-BASE-008` | S1 | single_day | trade | invariant | trade with no lifecycle event in N business days while position open | `aged_trade_detection` |
| `EVAL-BASE-009` | S1 | single_day | desk | threshold | net delta exposure per benchmark within desk.delta_limit | `desk_delta_limit_breach` |
| `EVAL-BASE-010` | S1 | single_day | desk | threshold | net vega exposure per surface within desk.vega_limit | `desk_vega_limit_breach` |
| `EVAL-BASE-011` | S1 | single_day | desk | threshold | net DV01 per curve bucket within desk.dv01_limit | `desk_dv01_limit_breach` |
| `EVAL-BASE-012` | S1 | single_day | counterparty | threshold | counterparty exposure within counterparty.credit_limit | `counterparty_credit_limit_breach` |
| `EVAL-BASE-013` | S1 | single_day | portfolio | threshold | concentration: top-N counterparty share within concentration limit | `counterparty_concentration_breach` |
| `EVAL-BASE-014` | S1 | single_day | row | invariant | lifecycle observation date passed AND no admitted lifecycle event | `lifecycle_observation_gap` |
| `EVAL-BASE-015` | S1 | single_day | trade | reconciliation | source_system_trade_id resolves to admitted canonical trade | `source_system_reconciliation_gap` |
| `EVAL-BASE-016` | S1 | single_day | desk | threshold | new_trades_in_period count within configured turnover threshold | `turnover_threshold_breach` |
| `EVAL-BASE-017` | S1 | multi_day | book | threshold | cumulative APL < stop-loss over rolling N days | `rolling_stop_loss_breach` |
| `EVAL-BASE-018` | S1 | single_day | trade | invariant | barrier/autocall observation date passed AND lifecycle_state inconsistent | `lifecycle_state_inconsistency` |
| `EVAL-BASE-019` | S1 | single_day | row | invariant | snapshot_id on AttributionRow resolves to admitted market snapshot | `snapshot_id_resolution_gap` |
| `EVAL-BASE-020` | S1 | single_day | row | invariant | rule_store version effective at row.date is admitted | `rule_store_version_resolution_gap` |

These are the load-bearing F_D evals. They compile to DuckDB SQL with `GROUP BY` on date/desk/trade/factor and run in seconds against tens of millions of rows. Most "unexplained P&L" investigations terminate inside this set — the failure is a reconciliation breach, a coverage gap, or a stale-greek artefact, not a model failure.

### 3.2 S2 — F_D generic overlay Evals

| Eval | Stage | Window | Scope | Reasoning | Predicate | Breach kind |
|---|---|---|---|---|---|---|
| `EVAL-BASE-101` | S2 | single_day | trade | delta | ½γ·ΔS² explains residual within ε given measured ΔS and SOD gamma | `convexity_overlay_resolved` (or `unexplained` if not) |
| `EVAL-BASE-102` | S2 | single_day | trade | delta | vanna·ΔS·Δσ + ½volga·Δσ² closes spot-vol cross residual | `cross_greek_overlay_resolved` |
| `EVAL-BASE-103` | S2 | single_day | trade | delta | Shapley decomposition over admitted factors closes residual | `shapley_overlay_resolved` |
| `EVAL-BASE-104` | S2 | single_day | book | ratio | paper hedge P&L offsets directional underlier P&L within hedge_band | `hedge_effectiveness_breach` |
| `EVAL-BASE-105` | S2 | single_day | trade | invariant | admitted lifecycle event (barrier hit, autocall, fixing) accounts for residual | `lifecycle_overlay_resolved` |
| `EVAL-BASE-106` | S2 | single_day | trade | delta | theta + carry accrual closes deterministic residual portion | `carry_theta_overlay_resolved` |
| `EVAL-BASE-107` | S2 | single_day | book | delta | new trades + unwinds explain APL − HPL gap within ε | `trade_activity_overlay_resolved` |

Each is a `CandidateFamily<Overlay>` member per `BOOTSTRAP.md` §10. Each overlay is deterministic — given inputs, the residual reduction is replay-stable. A row that does not close at S2 escalates to S3.

### 3.3 S3 — F_P configuration (not predicate)

S3 Evals declare hypothesis-space configuration, not rule predicates (per the prior ADR §3.4 stage-binding table). They configure synthesis when F_D cannot close.

| Eval | Stage | Window | Scope | Reasoning | Predicate (config) | Breach kind |
|---|---|---|---|---|---|---|
| `EVAL-BASE-201` | S3 | single_day | trade | explanatory_manifold | hypothesis space: { market_regime_change, operational_error, model_inadequacy, hedge_mismatch, counterparty_event, data_quality_issue, lifecycle_drift } | `hypothesis_ranking_emitted` |
| `EVAL-BASE-202` | S3 | rolling_n_days | desk | gain_surface_deviation | inferred surface drift from declared mandate exceeds tolerance vector | `gain_surface_drift_candidate` |
| `EVAL-BASE-203` | S3 | trade_lifetime | trade | correlation | cross-trade correlation manifold suggests common-cause failure across N trades | `correlated_breach_manifold_candidate` |

Per prior commitment ([[20260523T144237Z_DESIGN_eval_authoring_to_fd_compilation_pipeline]] §6), GainSurface lives only at S3. `EVAL-BASE-202` is the realization site for that decision.

### 3.4 S4 — F_H reviewer config

| Eval | Stage | Window | Scope | Reasoning | Predicate (config) | Output |
|---|---|---|---|---|---|---|
| `EVAL-BASE-301` | S4 | n/a | breach | n/a | reviewer role binding by breach_kind; remediation taxonomy { amend_trade, update_mapping_rule, recalibrate_model, adjust_threshold, extend_overlay_coverage, escalate_to_credit, no_action } | `RatifiedExplanation` admitted |

## 4. coal_trading Module — Coal-Specific Eval Catalog

Coal-specific Evals extend the base via the four extension points (`Trade.product`, `Overlay`, `RiskFactor`, S3 hypothesis space). All declarations below assume the coal domain layer from `BOOTSTRAP.md` §9 and the research source Appendix A.

Same inheritance rule as §3: each `EVAL-COAL-NNN` resolves to `specification/evals/coal_trading/EVAL-COAL-NNN/` with frontmatter + predicate + commentary template. The commentary templates render the coal-specific structured payloads — quality differential deltas, freight basis components, pricing-period mismatches, demurrage breakdowns — into the prose form shown in §5.4.

### 4.1 S1 — F_D coal-specific mechanical Evals

| Eval | Stage | Window | Scope | Reasoning | Predicate | Breach kind |
|---|---|---|---|---|---|---|
| `EVAL-COAL-001` | S1 | single_day | trade | invariant | INCOTERM ∈ {FOB, CIF, CFR, DES, DAP} AND freight handling consistent with incoterm | `incoterm_freight_handling_inconsistent` |
| `EVAL-COAL-002` | S1 | single_day | trade | invariant | PhysicalCargo.delivery.laycan_start ≤ vessel.eta_arrival ≤ laycan_end + tolerance | `laycan_compliance_breach` |
| `EVAL-COAL-003` | S1 | single_day | trade | invariant | PhysicalCargo.parcel.actual_delivered_mt within nominal_size_mt × (1 ± tolerance_pct) | `parcel_tolerance_breach` |
| `EVAL-COAL-004` | S1 | single_day | trade | coverage | every PhysicalCargo has resolvable benchmark_index_ref AND pricing_period | `coal_pricing_reference_gap` |
| `EVAL-COAL-005` | S1 | single_day | trade | invariant | quality_spec calorific_value basis declared (GAR/NAR/ADB) AND consistent across trade events | `coal_calorific_basis_inconsistency` |
| `EVAL-COAL-006` | S1 | single_day | trade | invariant | met coal trade carries required met_specs (CSR, fluidity, vitrinite) | `met_coal_spec_gap` |
| `EVAL-COAL-007` | S1 | single_day | trade | coverage | every PhysicalCargo has admitted load_quality_certificate after BL event | `load_quality_certificate_gap` |
| `EVAL-COAL-008` | S1 | single_day | trade | coverage | every PhysicalCargo has admitted discharge_quality_certificate after arrival event | `discharge_quality_certificate_gap` |
| `EVAL-COAL-009` | S1 | single_day | trade | invariant | quality_claim_raised event without paired settlement within SLA → flag | `quality_claim_aged` |
| `EVAL-COAL-010` | S1 | single_day | trade | reconciliation | demurrage_invoiced − demurrage_settled within ε of laytime calculation | `demurrage_reconciliation_gap` |
| `EVAL-COAL-011` | S1 | single_day | book | coverage | every paper trade against benchmark has FFA hedge coverage if desk policy requires | `ffa_hedge_coverage_gap` |
| `EVAL-COAL-012` | S1 | single_day | desk | threshold | basis position (API2-API4, API4-Newcastle) within desk.basis_limit_pairs | `cross_benchmark_basis_limit_breach` |
| `EVAL-COAL-013` | S1 | single_day | desk | threshold | freight route exposure (C5, C7, P2A, ...) within desk.freight_limit_routes | `freight_route_limit_breach` |
| `EVAL-COAL-014` | S1 | single_day | desk | threshold | physical position MT within desk.physical_position_mt limit | `physical_position_limit_breach` |
| `EVAL-COAL-015` | S1 | single_day | trade | reconciliation | cleared (ICE/SGX/CME) trade vs admitted exchange settlement reconciles | `cleared_trade_settlement_gap` |

### 4.2 S2 — F_D coal-specific overlay Evals

These are the load-bearing overlays for coal forensic value. They convert most "unexplained" coal residuals into explained closures, matching the research source's §"Common attribution edge cases in coal".

| Eval | Stage | Window | Scope | Reasoning | Predicate | Breach kind |
|---|---|---|---|---|---|---|
| `EVAL-COAL-101` | S2 | single_day | trade | delta | (realized_quality_spec − assumed_spec) × quality_adjustment_formula closes quality residual | `quality_differential_overlay_resolved` |
| `EVAL-COAL-102` | S2 | single_day | trade | delta | freight route move × hedged tonnage closes freight residual | `freight_basis_overlay_resolved` |
| `EVAL-COAL-103` | S2 | single_day | trade | delta | pricing-period basis (month-of-loading vs month-of-trade) closes residual | `pricing_period_overlay_resolved` |
| `EVAL-COAL-104` | S2 | trade_lifetime | trade | delta | demurrage P&L over voyage lifecycle closes residual against laytime model | `demurrage_overlay_resolved` |
| `EVAL-COAL-105` | S2 | single_day | trade | delta | calendar spread move (M1-M2, Q1-Q2, prompt vs cal-year) closes roll residual | `calendar_spread_overlay_resolved` |
| `EVAL-COAL-106` | S2 | single_day | trade | delta | location basis move (API2-API4, API4-Newcastle) closes basis residual | `location_basis_overlay_resolved` |
| `EVAL-COAL-107` | S2 | single_day | book | delta | FX move on USD-denominated coal exposure vs reporting currency closes FX residual | `coal_fx_overlay_resolved` |
| `EVAL-COAL-108` | S2 | trade_lifetime | trade | delta | storage carry + weathering loss (calorific degradation) closes inventory residual | `storage_weathering_overlay_resolved` |
| `EVAL-COAL-109` | S2 | single_day | book | delta | carbon (EU ETS / UK ETS) price move on consumption-side position closes carbon residual | `carbon_cost_overlay_resolved` |
| `EVAL-COAL-110` | S2 | single_day | trade | invariant | quality_claim_settled event accounts for residual on closed cargoes | `quality_claim_overlay_resolved` |
| `EVAL-COAL-111` | S2 | trade_lifetime | trade | ratio | physical-vs-paper hedge effectiveness within tolerance for paired (PhysicalCargo, FFA/Future) | `coal_hedge_pair_effectiveness_breach` |
| `EVAL-COAL-112` | S2 | single_day | trade | invariant | INCOTERM-implied costs (freight, insurance for CIF/CFR) accounted in net realized price | `incoterm_cost_allocation_breach` |

### 4.3 S3 — coal hypothesis extensions

Adds to the base S3 hypothesis space per the research source Appendix C extension pattern.

| Eval | Stage | Window | Scope | Reasoning | Predicate (config) | Breach kind |
|---|---|---|---|---|---|---|
| `EVAL-COAL-201` | S3 | single_day | trade | explanatory_manifold | additional hypotheses: { quality_underestimate, freight_basis_drift, demurrage_unhedged, pricing_period_mismatch, met_spec_substitution, vessel_substitution_cost } | `coal_hypothesis_ranking_emitted` |
| `EVAL-COAL-202` | S3 | trade_lifetime | trade | gain_surface_deviation | mandate vs realized: hedge-pair desk surface (Carry, Hedge_Quality, Limit_Compliance) deviation | `coal_hedge_pair_surface_drift_candidate` |

### 4.4 S4 — coal reviewer role binding

| Eval | Stage | Scope | Role binding |
|---|---|---|---|
| `EVAL-COAL-301` | S4 | breach (thermal) | `thermal_atlantic_desk_head`, `pnl_control_thermal`, `model_risk_reviewer` |
| `EVAL-COAL-302` | S4 | breach (met) | `met_origination_desk_head`, `pnl_control_met`, `quality_specialist` |
| `EVAL-COAL-303` | S4 | breach (freight) | `freight_desk_head`, `pnl_control_freight` |

## 5. Representative Full-YAML Examples

Three Evals expanded to the full frontmatter shape to anchor the catalog. The remaining catalog entries inherit the same shape.

### 5.1 EVAL-BASE-004 (S1 reconciliation, cross-asset)

```yaml
---
eval_id: EVAL-BASE-004
name: apl_hpl_drift_reconciliation
status: candidate
version: 1
effective_from: 2026-06-01
owning_module: attribution_base

window:
  kind: single_day
  alignment: business_day
scope:
  aggregate_span: trade
  entities: [Trade, AttributionRow, LifecycleEvent]
product_category: cross_asset
regime:
  deterministic: true
  probabilistic: false
reasoning_pattern: reconciliation

stage: S1
breach_kind: apl_hpl_drift
severity_calculator: linear_by_magnitude
gain_surface_link: null
compile_target: duckdb-sql
commentary_template: ./commentary_template.md
---

# Predicate

For each trade × business_day:

  | APL − HPL − sum(new_trades_in_period) − sum(fees_in_period) | < ε

Where:
  - APL = sum(attribution_row.actual_pnl) for the day
  - HPL = sum(attribution_row.hypothetical_pnl) for the day
  - new_trades_in_period = sum(pnl_at_trade_date) for trades booked on the day
  - fees_in_period = sum(fee_amount) admitted on the day
  - ε = threshold_registry[desk_id, "apl_hpl_drift_epsilon"].value at effective date

On violation, emit BreachCandidate with:
  - magnitude = | APL − HPL − new_trades − fees |
  - mechanical_evidence_refs = [
      attribution_row_ids for the (trade × day),
      lifecycle_event_ids in the day,
      fee_event_ids in the day
    ]
  - threshold_ref = threshold_registry.rule_id@version
  - structured_cause_signals = {
      late_booked_trade_count: int,
      manual_amendment_count: int,
      late_fee_count: int,
      intraday_correction_count: int
    }

Classic cause taxonomy (not part of S1 closure; carried to S2/S3 as structured signals above):
  - late_booking
  - wrong_trade_date
  - manual_amendment_not_flowed
  - intraday_correction
```

```markdown
# commentary_template.md  (rendered against BreachCandidate)

## APL vs HPL Reconciliation Breach

Desk `{{ desk_id }}` on `{{ business_day }}` shows an APL/HPL drift of
**{{ magnitude | currency }}** against a threshold of
{{ threshold | currency }} (rule `{{ threshold_ref }}`).

| Component | Value |
|---|---|
| Actual P&L (APL)        | {{ apl | currency }} |
| Hypothetical P&L (HPL)  | {{ hpl | currency }} |
| New trades in period    | {{ new_trades_pnl | currency }} ({{ structured_cause_signals.late_booked_trade_count }} late-booked) |
| Fees                    | {{ fees | currency }} ({{ structured_cause_signals.late_fee_count }} admitted after EOD) |
| Manual amendments       | {{ structured_cause_signals.manual_amendment_count }} |
| Intraday corrections    | {{ structured_cause_signals.intraday_correction_count }} |
| **Residual drift**      | **{{ magnitude | currency }}** |

{% if structured_cause_signals.late_booked_trade_count > 0 %}
Late-booked trades account for the dominant share of the drift. See
attribution rows {{ mechanical_evidence_refs.attribution_row_ids | first_n(5) }}.
{% elif structured_cause_signals.manual_amendment_count > 0 %}
The drift coincides with {{ structured_cause_signals.manual_amendment_count }}
manual amendment(s) admitted on the day. Amendment events:
{{ mechanical_evidence_refs.lifecycle_event_ids | filter(kind="amended") }}.
{% else %}
Neither late booking nor manual amendment is present. The drift implies a
data-quality or feed-integrity issue not yet classified at S1. Routed to S2.
{% endif %}

Evidence: {{ mechanical_evidence_refs }}
Threshold authority: {{ threshold_ref }}
```

### 5.2 EVAL-COAL-101 (S2 overlay, coal-specific)

```yaml
---
eval_id: EVAL-COAL-101
name: quality_differential_overlay
status: candidate
version: 1
effective_from: 2026-06-01
owning_module: coal_trading

window:
  kind: single_day
  alignment: business_day
scope:
  aggregate_span: trade
  entities: [PhysicalCargo, QualityDifferential, AttributionRow]
product_category: coal
regime:
  deterministic: true
  probabilistic: false
reasoning_pattern: delta

stage: S2
breach_kind: quality_differential_overlay_resolved   # or _unresolved
severity_calculator: residual_reduction_ratio
gain_surface_link: null
compile_target: duckdb-sql
candidate_family_member: QualityDifferentialOverlay
commentary_template: ./commentary_template.md
---

# Predicate

Given a BreachCandidate of kind apl_hpl_drift or unexplained_residual on a PhysicalCargo trade:

  realized_quality = cargo.discharge_quality_certificate.spec
  assumed_quality  = cargo.price_formula.quality_adjustment_ref.assumed_spec

  for each spec_axis in {calorific, sulfur, ash, csr, ...}:
    quality_delta[axis] = realized_quality[axis] - assumed_quality[axis]

  quality_pnl_explained =
    sum over spec_axis of
      quality_delta[axis] × quality_adjustment_formula[axis] × actual_delivered_mt

  residual_after_overlay = breach_candidate.magnitude - quality_pnl_explained

Closure rule:
  | residual_after_overlay | < overlay_close_epsilon[product_category]
  → state = resolved_by_overlay
  otherwise
  → state = remains_unexplained, escalate to next S2 overlay or S3

Evidence carried (becomes ResolvedBreach.overlay_evidence_refs):
  - quality_certificates: { load_ref, discharge_ref }
  - quality_adjustment_formula_ref
  - per_axis_quality_delta: { calorific: Δ, sulfur: Δ, ash: Δ, ... } with units
  - quality_pnl_explained_scalar
  - residual_after_overlay_scalar
  - dominant_spec_axis: the axis contributing most to quality_pnl_explained
```

```markdown
# commentary_template.md  (rendered against ResolvedBreach)

## Quality Differential Overlay {{ "Resolved" if state == "resolved_by_overlay" else "Partial" }}

Trade `{{ trade_id }}` (cargo `{{ cargo_id }}`,
{{ coal_type }} {{ benchmark }}, {{ actual_delivered_mt | mt }})
showed a residual of {{ original_magnitude | currency }} pre-overlay.

The quality differential overlay closes
**{{ quality_pnl_explained_scalar | currency }}** of that residual
({{ quality_pnl_explained_scalar / original_magnitude | percent }}),
leaving **{{ residual_after_overlay_scalar | currency }}** unexplained.

| Spec axis | Assumed | Realized | Δ | Adjustment $/MT | Contribution |
|---|---|---|---|---|---|
{% for axis, delta in per_axis_quality_delta %}
| {{ axis }} | {{ assumed_quality[axis] }} | {{ realized_quality[axis] }} | {{ delta }} | {{ quality_adjustment_formula[axis] | per_unit_currency }} | {{ delta * quality_adjustment_formula[axis] * actual_delivered_mt | currency }} |
{% endfor %}

The dominant axis is **{{ dominant_spec_axis }}**: realized {{ realized_quality[dominant_spec_axis] }} vs
assumed {{ assumed_quality[dominant_spec_axis] }}, a divergence of
{{ per_axis_quality_delta[dominant_spec_axis] }} units against an adjustment
of {{ quality_adjustment_formula[dominant_spec_axis] | per_unit_currency }}.

{% if state == "resolved_by_overlay" %}
Closure: this breach is explained by realized vs assumed quality. No further
escalation. Authority: load certificate {{ quality_certificates.load_ref }},
discharge certificate {{ quality_certificates.discharge_ref }}.
{% else %}
Closure: partial. {{ residual_after_overlay_scalar | currency }} remains and
is routed to the next S2 overlay candidate. If no further overlay closes,
escalates to S3 for hypothesis synthesis.
{% endif %}
```

### 5.3 EVAL-BASE-202 (S3 GainSurface deviation, configuration not predicate)

```yaml
---
eval_id: EVAL-BASE-202
name: gain_surface_deviation_synthesis
status: candidate
version: 1
effective_from: 2026-06-01
owning_module: attribution_base

window:
  kind: rolling_n_days
  alignment: business_day
  rolling_n: 5
scope:
  aggregate_span: desk
  entities: [Desk, Trade, AttributionRow, GainSurface]
product_category: cross_asset
regime:
  deterministic: false
  probabilistic: true
reasoning_pattern: gain_surface_deviation

stage: S3
breach_kind: gain_surface_drift_candidate
severity_calculator: per_dimension_deviation_vector
gain_surface_link:
  surface_ref: desk_mandate://${desk_id}/active
compile_target: polars-python   # S3 synthesis path; not the SQL hot path
synthesis_method: bayesian_network   # ratified per Module
commentary_template: ./commentary_template.md
---

# Configuration (not predicate)

This Eval is an S3 hypothesis-synthesis configuration, not a rule that closes.

Inputs:
  - declared GainSurface from desk_mandate://${desk_id}/active (Postgres rule store)
  - observed AttributionRow set over the rolling window
  - admitted BreachCandidate / ResolvedBreach events from S1/S2 in the window

Output: HypothesisRanking with:
  - inferred_surface: multi-dimensional fit to observed behavior
  - declared_surface_ref: mandate link
  - deviation_vector: per-dimension drift {carry, risk_adjusted_return,
    liquidity, capital_efficiency, hedging_balance, strategic_view}
  - confidence_interval: per-dimension uncertainty
  - top_hypotheses: [
      (gain_surface_violation, p, evidence_refs),
      (dimension_neglect, p, evidence_refs),
      (surface_drift, p, evidence_refs)
    ]

Authority: candidate evidence only. Closure remains S4 (F_H) ratification per
BOOTSTRAP.md §10. F_P does not close.
```

```markdown
# commentary_template.md  (rendered against HypothesisRanking)

## Gain-Surface Deviation — Candidate Synthesis (F_P)

> This commentary renders **candidate evidence**, not closure. Every claim
> below carries an explicit confidence interval and points to admitted S1/S2
> evidence. Closure requires S4 ratification.

Desk `{{ desk_id }}` over the rolling {{ window.rolling_n }} business days
ending `{{ window.end_date }}` shows behavior consistent with drift from the
declared mandate
({{ declared_surface_ref }}, version {{ declared_surface_version }}).

**Per-dimension deviation vector** (declared vs inferred):

| Dimension | Declared target | Inferred (95% CI) | Drift | Direction |
|---|---|---|---|---|
{% for dim, row in deviation_vector %}
| {{ dim }} | {{ row.declared }} | {{ row.inferred_low }} – {{ row.inferred_high }} | {{ row.drift }} | {{ row.direction }} |
{% endfor %}

**Top-ranked hypotheses** (synthesis_method: `{{ synthesis_method }}`):

{% for h in top_hypotheses %}
{{ loop.index }}. **{{ h.hypothesis_kind }}** — probability
   {{ h.probability | percent }} (CI {{ h.confidence_low | percent }} –
   {{ h.confidence_high | percent }}).
   Supporting evidence: {{ h.evidence_refs | summarize }}.
{% endfor %}

The dominant inferred dimensions are
{{ deviation_vector | top_n_dimensions(2) | join_and }};
the dominant declared dimensions are
{{ declared_surface | top_n_dimensions(2) | join_and }}.
The asymmetry is the candidate evidence for `gain_surface_drift_candidate`.

**Not asserted**: the cause. F_P can rank hypotheses but cannot declare a
ratified cause. Ratification belongs to {{ reviewer_role }} via S4.

Provenance:
- Synthesis input window: {{ window.start_date }} → {{ window.end_date }}
- Input AttributionRow scope: {{ scope_filter }}
- Admitted upstream breach events: {{ upstream_breach_refs | count }}
  ({{ upstream_breach_refs | first_n(3) }} ...)
- Declared mandate authority: {{ declared_surface_ref }}
- Replay anchor: synthesis_run_ref {{ synthesis_run_ref }}
```

### 5.4 Concrete Rendered Commentary (worked example)

To show what an analyst actually sees, here is the §5.2 template rendered against
a hypothetical `ResolvedBreach` for one coal cargo. This is the full Part (B)
artifact — Part (A) is the structured payload above it that this prose mirrors.

```markdown
## Quality Differential Overlay Resolved

Trade `T-CL-2026-04881` (cargo `CG-2026-Q2-1142`, thermal API2,
75,000 MT) showed a residual of $312,500 pre-overlay.

The quality differential overlay closes **$294,375** of that residual
(94.2%), leaving **$18,125** unexplained.

| Spec axis | Assumed | Realized | Δ | Adjustment $/MT | Contribution |
|---|---|---|---|---|---|
| calorific (kcal/kg NAR) | 6000 | 5750 | -250 | $0.0150/kcal | -$281,250 |
| sulfur (%)              | 1.00 | 1.18 | +0.18 | -$50/0.1% | -$13,500 |
| ash (%)                 | 15.0 | 15.1 | +0.1  | -$2.50/0.1% | +$375 |
| total moisture (%)      | 10.0 | 10.0 | 0     | n/a       | $0 |

The dominant axis is **calorific**: realized 5750 kcal/kg NAR vs assumed
6000 kcal/kg NAR, a divergence of -250 units against an adjustment of
$0.015/kcal/MT.

Closure: this breach is explained by realized vs assumed quality. No further
escalation. Authority: load certificate `cert://load/CG-2026-Q2-1142`,
discharge certificate `cert://discharge/CG-2026-Q2-1142`.
```

Observations on the worked example:

- Every number above is sourced from a field in the structured `ResolvedBreach`. The template performed arithmetic only on values already present (column products for the contribution column). No new authority is introduced.
- The closure decision in the final paragraph is read from `state == "resolved_by_overlay"` in the structured payload — the template does not decide closure, it reports it.
- An analyst can audit the commentary by inspecting the underlying admitted events at `cert://load/CG-2026-Q2-1142` and `cert://discharge/CG-2026-Q2-1142`. The prose is faithful to those records by construction.
- Adding a richer narrative (e.g., "this is consistent with the typical Indonesian thermal quality shortfall pattern") would be **out of bounds** — that's an inference the eval did not make. If such narrative is wanted, it belongs in an S3 hypothesis (declared, ranked, with confidence) and renders through that eval's commentary, not by smuggling it into the S2 template.

## 6. Compile-Target Distribution

Applying the prior turn's `(window, scope, volume_band)` selector to the catalog above, with `coal_trading` declaring `volume_band: low` (50-100k attribution rows/day per the user's stated coal volume):

| Compile target | Count of Evals | Reason |
|---|---|---|
| `duckdb-sql` | All S1 evals (35 across base + coal); most S2 overlays (~17) | single-date partition scans, GROUP BY, threshold checks; natural SQL form. Stays inside `BOOTSTRAP.md` §8.4 single-node bound |
| `polars-python` | S2 overlays needing richer math: Shapley (`EVAL-BASE-103`), hedge effectiveness (`EVAL-BASE-104`, `EVAL-COAL-111`), demurrage lifetime calc (`EVAL-COAL-104`), storage weathering (`EVAL-COAL-108`) | Polars expression DAGs handle structural computation cleanly; column-friendly |
| `polars-python` (S3) | `EVAL-BASE-201`, `EVAL-BASE-202`, `EVAL-BASE-203`, `EVAL-COAL-201`, `EVAL-COAL-202` | hypothesis synthesis; not in S1/S2 hot path; lower volume |
| `dask-python` / `spark-python` | 0 | not used in v0.1 for coal; admitted only with declared per-Module volume evidence above the §8.4 threshold |

The distribution validates the prior ADR's claim that "Python big-data F_D" at coal scale is *single-node Polars + DuckDB*, not Spark. A higher-volume Module (e.g., `power_trading` with millions of rows/day) declares `volume_band: high` and the compiler picks `polars-python` or `dask-python` for the wider-scan tuples without changing any Eval frontmatter.

## 7. trade_lifecycle Materialised View Triggers

Evals where `window: trade_lifetime` need the `trade_lifecycle/trade_id=<id>/*.parquet` materialised view from the prior turn (worst-case date-partition scan pattern). Catalog rows that trigger this:

| Eval | Why |
|---|---|
| `EVAL-COAL-104` | demurrage P&L spans the full voyage lifecycle, weeks to months |
| `EVAL-COAL-108` | inventory storage weathering accumulates over months |
| `EVAL-COAL-111` | physical-vs-paper hedge effectiveness measured over the cargo's full pricing period |
| `EVAL-COAL-202` | hedge-pair surface drift measured over the trade's lifetime, not a calendar window |
| `EVAL-BASE-203` | correlated-breach manifold over a trade's full life when investigation is forensic |

This is the smallest set of Evals that would justify standing up the lifecycle view in v0.1. If the lifecycle view is deferred, these five Evals defer with it.

## 8. Open Questions Carried Forward

Items not decided here; flagged for the ratification traversal:

1. **Threshold sources** — where do the actual numbers come from (desk mandates, risk policy, manual stand-up)? Each S1 threshold Eval has a `threshold_registry` lookup; that registry is empty until populated. Sequencing: spec the registry shape before the Evals can be authored against it.

2. **Severity calculators** — `linear_by_magnitude`, `residual_reduction_ratio`, `per_dimension_deviation_vector` are placeholders. The actual severity function affects S2→S3 escalation routing. Decide per breach_kind, not per Eval.

3. **Eval co-compilation grouping** — many S1 Evals scan `attribution_rows` for the same (date, desk). The compiler optimizer should co-compile them into one scan pass. Open question 3 from the prior ADR carries here.

4. **Coal-specific catalog completeness** — this list covers the load-bearing buckets from the research source. Edge cases not covered: term-contract embedded options (renewal, price reset, volume flex), spread-position leg-level netting attribution, cleared-vs-bilateral basis effective-price drift. Adding these is a v0.2 increment, not v0.1.

5. **S3 synthesis method per Module** — `EVAL-BASE-202` declares `synthesis_method: bayesian_network` as placeholder. Three lawful alternatives per the research source: Bayesian network, learned classifier, agent reasoning. Decide once per Module; carry through all S3 Evals in that Module.

6. **First-pass authoring order** — recommended sequence: `EVAL-BASE-{003,004,005,006,007}` first (universal invariants and coverage), then `EVAL-BASE-{001,002,009,010,011}` (desk threshold checks against a stand-up threshold registry), then `EVAL-COAL-{001,002,004}` (incoterm/laycan/pricing-reference invariants), then `EVAL-COAL-{101,102,103}` (the three load-bearing S2 overlays). This sequence produces a working end-to-end attribution slice for one coal trade — the §15 closure condition from `BOOTSTRAP.md`.

7. **Commentary-template language** — three lawful candidates: (a) Jinja2-style templates with a restricted filter set (rendering shown in §5 above), (b) typed function templates emitted by the compiler in the target language (no separate template engine), (c) a compact custom DSL. (a) is the most ergonomic for authors; (b) is the most replay-stable; (c) is the highest authoring cost. Recommendation: (b), with the compiler emitting a `render_commentary(payload: ResolvedBreach) -> str` Python or TypeScript function alongside the `evaluate(...)` predicate function. The §5 markdown is then notation for the same compilation target — what the author writes — not the runtime artifact.

8. **Commentary-template purity test** — every template must pass a deterministic check at compile time: every field referenced in the template must be present in the structured `BreachCandidate` / `ResolvedBreach` / `HypothesisRanking` payload type. A template that references a field the payload does not carry fails closed at compile time. This makes the §2.1 "no phantom facts" property a compile-checkable invariant, not a code-review convention.

9. **Commentary versioning** — commentary template changes are version bumps on the Eval (per §7 lifecycle). A breach replayed at an older Eval version renders against the commentary of that version, not current. This preserves audit faithfulness: a 2026-08 reviewer reading a 2026-06 breach sees the same prose that 2026-06 reviewers saw.

## 9. Status

Commentary. Candidate Eval catalog. Not ratified. The shape and coverage decisions encoded above feed the first ratification traversal that produces:

- `specification/requirements/REQ-F-OTE-090..n` (Eval authoring & compilation contract, from the prior ADR §5)
- `specification/evals/attribution_base/` and `specification/evals/coal_trading/` populated from this catalog
- `build_tenants/typescript/compiled_evals/duckdb-sql/...` and `.../polars-python/...` as compilation output

The catalog supersedes itself when ratified Evals exist under `specification/evals/`. Until then, this document is the authoring backlog.
