# Data Sources

Every input carries four properties before it is admitted anywhere near a
score: an identified **universe** (what population it measures), an identified
**sign** (which direction indicates stress), an acceptable **vintage**, and an
**independent lineage**. A source that fails any one of these is not admitted,
regardless of how closely its label resembles something the model needs.

---

## Roles

Admissibility is binary, not a continuous confidence weight. Every input holds
exactly one role:

| Role | Meaning |
| --- | --- |
| `SCORE` | Contributes to a composite score at a configured weight |
| `DIAGNOSTIC` | Displayed and monitored; contributes no score |
| `CONTEXT` | Background reference; not compared or ranked |
| `REJECT` | Examined and excluded, with the reason recorded |

A continuous weight would let weak evidence leak into a score at low volume.
Binary admissibility forces the question to be answered.

**Persisted and displayed does not mean admitted or scored.** Several series in
this system are fetched, stored, computed and rendered on the dashboard while
remaining formally inadmissible as governed measurements — because their
universe, deduplication rule or specificity is unresolved. Storing a series is
an engineering decision. Admitting it is a governance one. The dashboard
displays which is which, on the face of each card.

---

## Lineage families

Independence is a property of **measurement families**, not of publication
count. Two figures published separately but derived from the same underlying
dataset are one family and cannot corroborate each other.

| Family | Description | Used for |
| --- | --- | --- |
| **A — Traffic / AIS** | Vessel-tracking and traffic interpretation of Strait movements | `hormuz_transit`; LNG crossings as diagnostic |
| **B — Agency balance** | Published supply, export and balance aggregates, and manual continuations of them | Gulf-export and balance context; **cannot adjudicate against Family A** where the agency series is itself vessel-tracking derived |
| **C — Government petroleum stocks** | US weekly petroleum inventory and refinery data | `inventory_burn`, `refinery_stress`; independent downstream evidence |
| **D — European gas transparency** | Transmission-operator and terminal-operator published gas data | Gas subsystem, non-scoring |
| **E — Market prices** | Settled futures and derived crack spreads | `product_cracks`; `term_structure` currently abstaining |
| **F — Macro** | Macroeconomic indicator series | Contextual demand-side reference |

The critical constraint: a published agency export series that attributes its
own figures to a vessel-tracking vendor belongs to **Family A**, not Family B,
whatever the publisher's name on the cover. This single rule is why the Gulf
export component abstains.

---

## Sources

### Oil subsystem

| Source | Family | Cadence | Role | Notes |
| --- | --- | --- | --- | --- |
| US EIA weekly petroleum status | C | Weekly | `SCORE` + `DIAGNOSTIC` | Ten series: crude, gasoline, distillate, jet and strategic stocks; crude inputs; refinery utilisation; imports; products supplied. Approx. 500 stored observations. Exact-date week-over-week and year-over-year matching; no interpolation or nearest-date tolerance. |
| Market price data | E | Daily | `SCORE` | Settled bars only. Weekend rows and the newest unsuperseded bar are excluded so a partial session cannot move the alert band. Cracks are constructed independently per product; no crack is derived from another. |
| Macro indicator series | F | Monthly | `CONTEXT` | Idempotent merge that permits source-owned historical revision without disturbing other columns. |
| Strait traffic log | A | Manual, irregular | `SCORE` | Aggregate daily crossings by commodity class. Small hand-entered observation set; LNG crossings excluded from the score. |
| Gulf export aggregates | A / B | Publication-dependent | `REJECT` (for scoring) | See below. |

### Gas subsystem

| Source | Family | Cadence | Role | Notes |
| --- | --- | --- | --- | --- |
| **Gas Infrastructure Europe — AGSI+** | D | Daily gas day | `DIAGNOSTIC` | EU aggregate underground storage: gas in storage, working gas volume, fullness, injection, withdrawal, net withdrawal. 2,199 observations from 2020-08-17. |
| **Gas Infrastructure Europe — ALSI+** | D | Daily gas day | `DIAGNOSTIC` | EU aggregate LNG terminals: inventory in energy and volume terms, send-out, declared total maximum inventory, declared total reference send-out. 2,199 observations from 2020-08-17. |
| ENTSOG external pipeline flow | D | Daily gas day | `DIAGNOSTIC` — provisional | **Persisted and rendered** (4,401.8 GWh/d in the published snapshot). **Not admitted as a governed measurement.** Not topology-cleansed; no complete EXIT-side deduplication rule exists. Distinct published identifiers can carry near-identical flow with no declared relationship, so no summation rule, representative point or regional aggregate has been authorised. Displayed under a `PROVISIONAL` badge. |
| ENTSOG reported EU production | D | Daily gas day | `DIAGNOSTIC` — provisional | Persisted and rendered. Transmission-operator-visible aggregation across a subset of EU countries — not EU production. Some countries absent entirely and at least one materially under-covered; a falling member count is not falling production. Displayed under a `PARTIAL EU COVERAGE` badge. |
| IMF PortWatch | A | Daily | `CONTEXT` | **Persisted and rendered** as an AIS-derived shipping-transit proxy. **Explicitly not LNG-specific and not Qatar LNG throughput.** Not additive to the European balance. Non-scoring. Displayed under a `NOT LNG-SPECIFIC` badge. |
| U.S. LNG supply (EIA) | C | Monthly | `CONTEXT` | Persisted and rendered as upstream context. Not additive to the European balance: U.S. LNG reaching Europe already appears in terminal inventory and send-out. Daily feedgas is a disclosed gap, not an estimate. Any accompanying STEO figure is labelled a forecast and is not in the persisted actual series. Displayed under a `MONTHLY / STALE` badge. |

Storage and terminal panels retain **separate** evidence dates, source status
and freshness. A combined gas artifact displays the difference between them
rather than reconciling it to a single subsystem date.

---

## Why the Gulf export component abstains

A structured admission search was run against five criteria frozen **before**
any source was consulted:

```text
1  scope         comparable universe and route definition to the
                 published pre-war aggregate
2  vintage       an observation later than the existing stored aggregate
3  lineage       not vessel-tracking derived; not reconstructed from
                 country legs; not a carry-forward of the project's own rows
4  cadence       re-obtainable on a stated publication clock
5  independence  not the same measurement family as the traffic component
```

The candidate list was named before searching and was not extended during it —
a deliberate guard against selecting a source by its output.

Every candidate failed at least one criterion. The most instructive failure:
the leading agency export series attributes its own figures to a
vessel-tracking vendor, so admitting it would have supplied no independence
from the traffic component while appearing, on the cover, to be a second
source.

The component ships abstaining. The pre-disruption denominator remains null.

---

## External comparators

External research is used to check the magnitude of already-constructed
estimates **after** construction — never to reverse-engineer a construction
toward a preferred answer. No comparison below has changed a score, a weight, a
threshold or an admission status.

### Public benchmarks

- **IEA Oil Market Report Gulf-export aggregates**, including volumes bypassing
  the Strait, and the corresponding pre-war reference. These establish the
  approximately 24 mb/d pre-war figure as an externally defined candidate
  calibration anchor — while its vessel-tracking lineage keeps it out of the
  measurement-admission path.
- **IEA alternative-route export figures**, materially larger than this
  project's stored bypass field. That gap is the basis for the finding that the
  local bypass value cannot represent comprehensive non-Strait exports.
- **The published ~20 mb/d normal Strait throughput benchmark**, against which
  the project's configured normalisation anchor is approximately corroborated
  but not validated.
- **The publicly stated 8–10 mb/d official transit range.**

### Subscription research

Two Goldman Sachs notes from August 2026 — "Oil Comment: How Much Oil Is the
Gulf Exporting?" and "Natural Gas Comment" — were reviewed after the relevant
project observations were already recorded. Their Gulf-export estimates
(14.8–15.8 mb/d) and Strait-flow assessment (near 8–10 mb/d) fell close to this
project's stored values; their European LNG finding matched the dashboard's
directionally.

Only the minimum figures needed to describe the comparison are cited. No
report text, exhibit, table, chart or forecast framework is reproduced, and no
subscription research is treated as a validation authority — external agreement
does not confer admissibility. The comparisons are differently scoped and test
magnitude rather than like-for-like accuracy.

---

## Absence handling

Absence is never filled, interpolated, nearest-matched or substituted. Three
absence states are distinct and are displayed:

```text
UNAVAILABLE      the observation does not exist for that date
STALE            the observation exists but is behind the expected clock
FETCH DEGRADED   acquisition failed or returned partial data
```

None of the three is a zero. A missing comparison date makes the derived
change unavailable rather than shifting the comparison to a nearby date. A
source defect fails closed and identifies itself; it does not degrade quietly.

---

## Attribution

European gas storage and LNG terminal data are sourced from the **Gas
Infrastructure Europe AGSI+ and ALSI+ transparency platforms**. This
attribution is rendered verbatim on every table, chart, export and dashboard
artifact built from those panels, and is not abbreviated or reconstructed from
the source name.
