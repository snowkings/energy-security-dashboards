# The Strait of Hormuz: One Chokepoint, Two Measurement Problems

This page explains why the project maintains two separate dashboards for a
disruption that originates at a single geographic point.

---

## 1. Why the Strait matters

The Strait of Hormuz is the outlet for seaborne exports from Saudi Arabia,
Iraq, the UAE, Kuwait, Qatar and Iran. In normal conditions roughly 20 million
barrels per day of crude and refined products transit it — close to a fifth of
global liquids consumption — and it is the sole maritime route for Qatari LNG,
one of the largest LNG export complexes in the world.

There is no substitute route with comparable capacity. Bypass infrastructure
exists — Saudi Red Sea loadings, the UAE's Fujairah line, the Iraqi–Türkiye
route via Ceyhan, Iran's Jask terminal — but its combined throughput is a
fraction of normal Strait volumes, and its realised utilisation is not the
same quantity as its nameplate capacity.

For gas, there is effectively no bypass at all.

---

## 2. Why one vessel count cannot measure both

A crossing count is a count of hulls. It is not a count of energy.

That limitation applies to oil, but it applies far more severely to gas,
because the two systems fail in different places:

**Crude and products have shock absorbers.** Commercial and strategic
inventories can be drawn. Alternative grades can be substituted. Refineries
can re-optimise their slates. Pipelines provide partial route substitution.
The disruption is absorbed for a period and then appears downstream as
inventory draws, crack-spread widening and refinery-run stress.

**LNG has almost none.** The chain runs liquefaction plant → specialised
carrier → regasification terminal → destination pipeline network, and every
link is capacity-constrained and asset-specific. There is no equivalent of
"buy a different grade." A cargo that cannot load cannot be replaced by
rerouting a different cargo unless that cargo physically exists elsewhere and
a receiving terminal has both slot and capacity. Storage substitution happens
in underground caverns hundreds of kilometres from the tidewater, on a
seasonal rather than a weekly clock.

So the same 30% reduction in Strait transits produces a slow, absorbable,
inventory-mediated response in petroleum and a fast, structural, seasonally
compounding response in European gas. Measuring them with one number would
average away the entire signal.

---

## 3. Where the effects actually show up

| System | Immediate observable | Downstream confirmation |
| --- | --- | --- |
| Crude / products | Strait transits, Gulf export aggregates, bypass utilisation | Commercial and strategic inventories, refinery runs and utilisation, product cracks, term structure |
| LNG / gas | Loadings at the export complex, cargo destinations | European LNG terminal inventory and send-out, external pipeline supply, underground storage level and filling trajectory |

The two columns are measured by different institutions on different clocks.
Petroleum inventory data are weekly and government-published. European gas
storage and terminal data are daily and published by transmission operators
through a transparency regime. Cargo-level flow data are commercial and
vessel-tracking derived.

That difference in clock and lineage is why the subsystems are versioned
independently.

---

## 4. Why vessel tracking alone is not enough

Automatic Identification System data are the fastest available evidence and
the weakest.

- **Dark crossings.** Transponders can be switched off. Visible traffic then
  understates actual flow, and the understatement is largest exactly when
  political incentives to conceal movement are highest — that is, during a
  disruption.
- **AIS restoration distance.** Vessels may re-appear well outside the Gulf,
  attributing a movement to the wrong window or the wrong origin.
- **Ship-to-ship transfers.** Cargo can change hulls at sea, so one cargo can
  appear as two vessels or as none.
- **Hulls are not barrels.** Without IMO/MMSI identity, vessel class, deadweight,
  and laden-versus-ballast state, a crossing count cannot be converted into a
  volume. In this project that is a data-acquisition gap, not a modelling gap.
- **Lineage collapse.** Several apparently independent published estimates
  ultimately trace to the same vessel-tracking vendor. They are one evidence
  family and cannot corroborate each other.

The project's response is not to discard AIS evidence but to refuse to let it
stand alone. Strait traffic is scored against an independent downstream
evidence family — government petroleum inventory data — that has no
vessel-tracking parentage.

---

## 5. A reopening announcement is not normalisation

The project's governing objective is explicitly not to accept any government's
characterisation of whether the Strait is open.

An announcement is a statement about intent. Normalisation is a physical
condition with observable signatures: transit counts recovering toward
baseline, loadings recovering at the export complex, inventories ceasing to
draw, cracks compressing, terminal send-out and storage injection returning to
seasonally ordinary levels. Those signatures arrive at different speeds, and
the gas signatures arrive last because storage refilling is bounded by
injection capacity and by the calendar.

A dashboard that reacts to headlines measures sentiment. This one is built to
measure execution.

---

## 6. Normalisation baselines and their honest status

Every stress score is a ratio, and a ratio is only as defensible as its
denominator. This project's Hormuz baselines are documented with their actual
epistemic status rather than presented as settled constants:

- **Normal Strait oil flow (~21 mb/d configured).** The exact configured
  provenance is not recoverable from preserved project history. Its magnitude
  is approximately corroborated by the published ~20 mb/d benchmark, so it is
  retained as an approximate normalisation anchor and labelled as one.
- **Normal daily crossings (28/day configured).** The construction has been
  recovered — crude plus product crossings, matching the scope of the logged
  numerator — but the empirical magnitude has not been independently
  validated against an external vessel-count series.
- **Pre-disruption Gulf exports.** Formerly held a derived, wrong-vintage
  value. That value was invalidated and set null, which is why the
  `gulf_exports` component now abstains rather than producing an export-loss
  score against an unsound denominator.
- **Normal LNG crossings.** Not configured. A single seed observation exists
  in the record and was deliberately not promoted into a calibration constant.
  LNG traffic is therefore excluded from the oil score entirely and carried as
  diagnostic context only.

Publishing these baselines with their defects visible is the point. A
normalisation constant whose origin cannot be reconstructed is a liability
whether or not anyone writes it down.
