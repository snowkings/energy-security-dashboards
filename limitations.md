# Known limitations and unresolved measurement gaps

This page records the boundaries that materially affect interpretation of the
dashboards. They are grouped as measurement gaps, methodological constraints,
open model decisions and outputs the system does not produce.

**Terminology.** Throughout this repository, *Persian Gulf producer exports*
refers to oil exports from Saudi Arabia, Iraq, the UAE, Kuwait, Qatar and Iran,
including non-Hormuz routes when available. US Gulf Coast exports are outside
this definition. The internal component name remains `gulf_exports`.

---

## 1. Measurement gaps

**Strait crossings do not establish oil volume.** The traffic record contains
aggregate daily crossings by broad commodity class. It does not contain IMO or
MMSI identifiers, vessel class, deadweight, laden-versus-ballast state, or
per-vessel origin, destination or cargo estimates. The 8.8 mb/d flow estimate
is displayed as part of the traffic/AIS evidence family, but its derivation is
not preserved and it is not used as an independent measurement.

**AIS coverage is incomplete.** The record cannot quantify vessels omitted
because their transponders were dark or not captured, and it does not observe
ship-to-ship transfers. The size of those omissions is unknown.

**The shipping context is not LNG-specific.** The PortWatch card is an
all-vessel proxy. It does not identify LNG carriers or LNG volumes and is
excluded from the European balance.

**Qatari LNG loadings are not observed.** The gas dashboard covers parts of the
European receiving end—terminal inventory and send-out, pipeline supply,
reported production, underground storage and apparent demand. It does not
measure those loadings or trace cargoes from Qatar through the Strait to
European terminals.

**The external-pipeline aggregate remains provisional.** Some ENTSOG
identities can represent the same physical connection or publish nearly
identical flows without a declared relationship. No complete deduplication or
representative-selection rule has been established. The dashboard reports the
current member denominators so changes in coverage remain visible.

**European production coverage is partial.** The displayed production figure
aggregates transmission-operator-visible observations across 11 countries. It
is not a measure of total EU production.

**No current observation of Persian Gulf producer exports satisfies the
admission rules.** The search required comparable scope, a later observation,
non-vessel-tracking lineage, a reproducible publication cadence and
independence from the Strait traffic component. No candidate met all five, so
`gulf_exports` abstains.

**No validated M1–M2 quote has been admitted.** The manual override path is
available, but no typed observation is present. `term_structure` therefore
abstains.

**The European storage series is an EU aggregate.** It does not match the
North-West European universe used in some published market analysis. A direct
comparison can therefore reflect different geographic coverage rather than a
different reading of the same physical state.

**Gas-price and daily US feedgas confirmation are absent.** The gas subsystem
contains no TTF or JKM curve and no daily US LNG feedgas series. EIA's monthly
US LNG supply figure is upstream context; the STEO figure shown beside it is an
external forecast, not a persisted actual observation.

**Several oil baselines and estimates retain incomplete provenance.** The
28-crossing oil baseline has a reconstructed crude-and-products scope, but its
empirical magnitude has not been independently validated. The 21 mb/d normal
oil-flow reference is externally plausible, but its local derivation is not
preserved. The 8.8 mb/d flow estimate, bypass series and 422 million-barrel
accessible-buffer construction also remain marked `UNVERIFIED FROM PRESERVED
PROVENANCE` where they appear.

---

## 2. Methodological constraints

**The oil score has partial coverage.** At the 2026-08-24 evidence date, 66% of
configured weight was observed, 6% assumed and 28% abstained. The 62.7/100
score is renormalised over the 72% that scored. Comparisons with another score
must therefore account for any change in coverage as well as the score itself.

**The LNG dashboard produces no score, weight, threshold or alert.** The gas
chain runs liquefaction → carrier → terminal → pipeline → storage → withdrawal.
The dashboard observes parts of the European end, with the gaps marked, and
does not reduce that partial view to a composite.

**The gas histories are latest-value records, not vintage archives.** Incoming
rows replace older values for the same gas day. Historical ranks and
percentiles therefore describe the values currently stored, not necessarily
the data available on each original publication date. Prior vintages are not
retained.

**Estimated GIE observations are admitted with disclosure.** Excluding them
would remove approximately 63% of the AGSI history, and estimated observations
do not simply age into confirmed ones. Confirmed and estimated values are both
admitted; unavailable values contribute nothing. Where historical comparisons
are shown, the dashboard reports the current quality state and the reference
sample's confirmed/estimated composition.

**Seasonal reference samples are small.** The gas comparison uses five prior
calendar years and a ±3-day window, for a maximum of 35 observations. The
dashboard reports the actual observation count and contributing years rather
than presenting the percentile without its sample depth.

**Storage and terminal denominators move.** AGSI fullness uses working gas
volume and ALSI terminal fill uses declared maximum inventory. Both change
through time. Absolute levels and ratios are reported separately; a difference
between their seasonal positions remains a denominator finding rather than a
single resolved signal.

---

## 3. Known defects and open model decisions

**Legacy score history cannot retain multiple same-day model states.** The
date-keyed history keeps only the latest row for an evidence date, so the
documented intermediate scores of 56.4 and 65.3 do not survive there. Immutable
run history now preserves separate executions, and the dashboard marks later
methodology boundaries, but the missing legacy states cannot be recreated as
recorded runs.

**The product-margin proxies use a cross-benchmark construction.** NYMEX
heating-oil and RBOB futures are converted to dollars per barrel and measured
against Brent. This embeds the Brent–WTI differential in a scored component
without a recovered rationale for pairing US products with a global crude
benchmark. The construction remains unchanged pending a criterion—set before
either alternative is scored—for whether the component should measure US
refining margin or product stress against the crude benchmark exposed to the
Strait disruption.

**The gasoline coefficient remains unresolved.** The gasoline leg is scaled by
0.6 in the `product_cracks` formula, and the original rationale has not been
recovered. The recalibrated diesel and gasoline ladders also have different
reach under the stored corpus because the same distribution-based rule is
applied to differently shaped samples. Neither feature will be changed merely
to make the resulting scores look more even.

**The Strait component is at its scoring ceiling.** `hormuz_transit` reads
100.0 in the current snapshot. The component cannot distinguish further
deterioration while conditions remain beyond the highest rung of its present
ladder.

---

## 4. Outputs not produced

Across the two dashboards, the system does not produce:

- an LNG score, weight, threshold or alert;
- oil or gas price forecasts;
- days of cover, days to empty or projected depletion dates;
- projected end-of-season storage fill;
- regulatory-target stress or winter-adequacy judgements;
- attribution of molecule origin; or
- vessel-level cargo or barrel estimates.

The dashboards record physical and market state; they do not generate
forecasts. The STEO forecast shown on the LNG dashboard is external context,
explicitly labelled `FORECAST`, and excluded from both the European balance and
the oil score. No forecast-performance claim is made. Adding any of the outputs
above would require its own measurement contract.

---

## 5. External comparison

Later subscription research was compared with selected stored readings as an
external check on magnitude and direction. The comparator is not reproduced or
identified in this repository, so the comparison cannot be independently
verified here. Agreement did not change a component's provenance, admission
status, threshold or score.

The compared series also use different geographic universes and definitions.
Their alignment provides context, not like-for-like validation.
