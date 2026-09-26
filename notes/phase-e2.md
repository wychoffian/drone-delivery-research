# Phase E2 acoustic fleet-policy integration

Phase E2 connects the Phase E1 mission exposure vectors to drone dispatch, exposure reservation, mission completion, queues, charging, and regulatory review periods. The Phase D proxy model remains unchanged as a reproducible baseline.

> Evidence label: Integrated calibrated mechanism in a synthetic scenario. Acoustic route increments use calibrated parameters. Network geometry, demand, operations, and policy budgets remain declared scenarios.

[Run the acoustic fleet-policy model](acoustic-fleet.html)

## Integration decision

The ledger stores normalized linear sound-exposure energy. It does not store or add dB values. A candidate mission returns nine energy increments, one for each neighborhood receiver. Before dispatch, the operator checks and reserves the complete outbound and return vector. Mission completion removes the reservation and adds the same vector to observed exposure.

LAE is calculated only when results are displayed:

```text
LAE = 10 log10(accumulated normalized sound-exposure energy)
```

This design preserves the reservation and completion rules tested in Phases B through D while replacing the arbitrary exposure coefficients.

## Budget scenario

No legal or community exposure limit has been established. Phase E2 therefore uses a declared reference-mission equivalent for model experiments.

For each receiver, one reference mission is a round trip on that receiver's own corridor at 80 m altitude and 15 m/s. A budget of 20 reference missions means 20 times that receiver-specific linear energy. The budget is stored as energy and may be reported as an equivalent mission count or as LAE.

Table 1 records the scenario parameters.

| Parameter | Value or range | Status |
| --- | --- | --- |
| Initial budget | 10, 20, or 30 reference missions | User-selected scenario |
| Adaptive target | 14 reference missions per review period | Declared controller target |
| Adaptive bounds | 5 to 40 reference missions | Declared controller bounds |
| P3-D damping | 0.50 | Phase D design choice |
| Review period | 480 operational minutes | Phase D design choice |
| Fleet | Two drones and two chargers | Controlled prototype |
| Altitude and speed | 80 m and 15 m/s | Synthetic operating scenario |

Table 1. Policy and operating scenarios used by the Phase E2 integration prototype.

These equivalent counts make the experiment interpretable without suggesting that one mission has the same exposure at every receiver. Each receiver has its own physical reference energy. A later policy process must replace this normalization with a defensible exposure budget.

## Policy regimes

Table 2 shows how the earlier regimes are translated into physical exposure accounting.

| Policy | Phase E2 rule |
| --- | --- |
| P0 | Select the shortest feasible operational route and record acoustic exposure without a budget |
| P1 | Add population-weighted physical exposure to route cost and record exposure without a budget |
| P2 | Select the shortest route whose nine-value energy vector fits the fixed budget |
| P3-U | Apply the adaptive controller to reference-mission equivalent budgets without damping |
| P3-D | Apply the same controller with damping factor 0.50 |

Table 2. Policy regimes after replacing proxy route coefficients with acoustic energy vectors.

P1 normalizes each route increment by the corresponding receiver reference energy, weights it by population, and adds the resulting burden to distance. The current weight is a declared route-choice parameter and requires sensitivity analysis.

## Event sequence

The event order remains completion, charging completion, review boundary, arrival, and dispatch. The dispatch cutoff is calculated from the longest physical mission rather than fixed from the old schematic route length. This ensures that an accepted mission completes before its review boundary.

The regulator observes completed mission exposure only. Reserved energy is unavailable to other dispatch decisions but is not counted as observed exposure until the mission completes.

## Verification

Automated checks confirm that:

- demand streams remain identical across policies;
- every request is dispatched or retained in the final queue;
- every mission reserves nine positive energy increments;
- P2, P3-U, and P3-D remain within all nine energy budgets;
- no reservation remains at a review boundary;
- zero demand produces zero exposure;
- zero responsiveness makes both adaptive policies reproduce P2;
- exposure display uses energy-to-LAE conversion;
- Phase B through E1 checks still pass.

## Discussion

Phase E2 closes the software connection between measured acoustics and the regulatory agent. It also changes the interpretation of route feasibility. A route can fail because its contribution would exceed any one of nine neighborhood energy budgets, including a receiver outside the selected corridor.

The module does not establish that the reference-mission budget is socially acceptable or legally meaningful. Its purpose is to test the integrated mechanism in coherent physical units. Demand, fleet performance, route geometry, background sound, and population remain synthetic.

The next experiment should vary the declared budget, source-level uncertainty, altitude, speed, acoustic route-choice weight, and validation error across matched demand seeds. The Kawai et al. data can then add a separate short-term annoyance outcome without changing the physical energy ledger.

## Reproducibility file

The [Phase E2 design record](results/phase_e2_design_v1.json) documents the accounting rule, controller scenarios, and current exclusions.
