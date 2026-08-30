# Limitations

This page states what the system cannot currently establish. It is maintained
as a first-class document rather than a footnote, because a monitoring system
that cannot say where its inference stops is not a monitoring system.

Items are grouped by whether they are measurement gaps, methodological
constraints, or known defects.

---

## 1. Measurement gaps

**Vessel counts are not volumes.** The Strait traffic record contains
aggregate daily crossings by broad commodity class. It does not contain IMO or
MMSI identifiers, vessel class, deadweight, laden-versus-ballast state, or
per-vessel origin, destination or cargo estimate. Vessel-by-vessel barrel
reconstruction is therefore not available, and no capacity assumption has been
introduced to manufacture one.

**AIS misses dark crossings and ship-to-ship transfers.** Visible traffic is a
lower bound on actual movement, and the gap widens precisely during periods of
disruption.

**Strait context is not LNG-carrier-specific.** The traffic record is an
all-vessel proxy. It does not identify LNG carriers or LNG volumes, and it is
not used to claim anything about gas throughput. LNG crossings are carried as
diagnostic context with no score impact and no configured baseline.

**Qatari LNG loadings are not directly measured.** The gas subsystem observes
the European receiving end of the chain — terminal inventory, send-out,
pipeline supply, underground storage — but not the Gulf loading end. The link
between Strait disruption and European gas state is therefore inferred from
downstream evidence, not observed end to end.

**No current admissible Gulf-export observation exists.** A structured source
search was conducted against criteria frozen before any source was consulted:
comparable scope, post-period vintage, non-vessel-tracking lineage,
reproducible cadence, and independence from the traffic component. No source
satisfied all five. `gulf_exports` ships abstaining as a result.

**No validated term structure quote.** `term_structure` abstains for the same
class of reason: no typed, validated M1–M2 series has been admitted.

**European gas storage is an EU aggregate.** It is not the North-West European
subset used by several published market analyses. Comparing the two directly
produces an apparent discrepancy that is a geographical-universe difference,
not a disagreement about physical conditions.

**No price confirmation layer for gas.** The gas subsystem carries no TTF or
JKM series, so it cannot observe price-driven demand response or confirm
physical tightness against the forward curve.

---

## 2. Methodological constraints

**Two components abstain, so coverage is 72%.** The composite score is
computed over the admissible components and renormalised. The coverage figure
is displayed with every score. A score at 72% coverage is not equivalent to
the same score at full coverage, and the dashboard does not present it as
such.

**The gas subsystem produces no score at all.** By design. See the README for
why a partially observed physical chain should not be collapsed into a
composite.

**Local historical files are latest-value histories, not vintage archives.**
Seasonal ranks and percentiles describe the history currently stored, not
necessarily the data an analyst would have seen on each historical
publication date. Source revisions are absorbed into the stored history rather
than preserved as immutable point-in-time snapshots. Building a full vintage
archive is an open item.

**Estimated-quality observations are admitted with disclosure.** European gas
transparency data carry confirmed, estimated and unavailable quality states.
Excluding estimated observations would remove roughly 63% of the storage
history, and estimated values do not reliably age into confirmed, so
exclusion would not function as a wait-for-confirmation policy. Confirmed and
estimated are both admitted, and every statistic discloses the quality
composition of its reference sample. Unavailable contributes nothing and is
never treated as zero.

**Seasonal samples are small and are labelled as such.** Same-calendar-day
comparisons draw on five prior calendar years within a narrow day window. The
observation count is displayed with every percentile. A percentile computed
over a handful of observations is reported as exactly that.

**Denominators move.** Storage fullness is a ratio to working gas volume;
terminal fill is a ratio to declared maximum inventory. Both denominators
change over time and across the reference years. Absolute level and fill ratio
are reported separately, and where their seasonal positions disagree the
denominator change is described rather than resolved by picking whichever
reading looks more stressed.

**Several stored values are marked UNVERIFIED FROM PRESERVED PROVENANCE.**
For these, the arithmetic that reproduces the value is known but the direction
of derivation is not — in at least one case because the value and its
apparent inputs entered the repository in the same commit, making the
derivation order unrecoverable. Recovering a construction is not the same as
validating it, and the record says so.

---

## 3. Known defects and open items

- Part of the configured oil weight sits in a saturated component. A scoring
  ladder whose breaking endpoint is exceeded by current conditions carries no
  resolution in the present regime — it reports maximum stress and stops
  discriminating. This is recorded as a calibration-resolution defect, not
  treated as a validity failure.
- A known reachability inversion exists in the product-crack rungs, where one
  commodity's critical rung sits above the calibration corpus maximum and
  another's breaking rung sits below the stored high percentile.
- Two historical methodology-break scores are absent from the score history
  because of a date-keyed upsert that overwrote same-day rows. The gap is
  documented rather than backfilled.
- Some external pipeline aggregation and deduplication questions are
  unresolved: distinct published identifiers can carry near-identical flow
  with no declared relationship between them, so no summation rule,
  representative point, or regional aggregate has been authorised.
- One market-data ticker fails silently on refresh and is pending replacement
  or removal.

---

## 4. What this system does not attempt

The following are outside scope and are not produced, in either subsystem:

```text
price forecasts
days of cover / days to empty
projected end-of-season storage fill
regulatory-target stress scores
winter adequacy judgements
attribution of molecule origin
vessel-level cargo estimates
```

Each would require its own measurement contract. None has one.

---

## 5. Forecast accuracy has not been established

The system is a state-measurement instrument, not a predictive model. It has
not been backtested, and no forecast-accuracy claim is made anywhere in this
repository. External research has been used to check the magnitude of
constructed estimates after the fact — never to reverse-engineer a
construction toward a preferred answer.

Where external comparison has been performed, it has mostly established
directional agreement rather than like-for-like numerical agreement, because
the published series and the local series measure different universes on
different definitions. That distinction is preserved rather than smoothed
over.
