# Phase E3 acoustic sensitivity results

Phase E3 tests whether the Phase E2 policy comparison is stable when the declared budget and influential acoustic or operational assumptions change. The experiment contains 3,900 runs from 130 configurations, with 30 matched demand seeds per configuration.

> Evidence label: Sensitivity finding from a calibrated acoustic mechanism in a synthetic network. The results do not establish a real fleet requirement, community response, or legal exposure limit.

## Design

The experiment uses one-at-a-time changes around the Phase E2 reference setting. Every setting is run under medium and high demand for P0, P1, P2, P3-U, and P3-D. Seeds 501 through 530 provide common request sequences across policies and sensitivity settings.

Table 1 defines the thirteen settings.

| Factor | Levels |
| --- | --- |
| Reference | Budget 20 missions, 88.4 dB(A), no validation adjustment, 80 m, 15 m/s, route weight 0.35 km |
| Initial budget | 10 and 30 reference missions |
| Source reference level | -1 and +1 dB from the calibrated mean |
| Validation error stress | -2.29 and +2.29 dB, equal to one held-out NASA RMSE |
| Altitude | 60 and 120 m |
| Speed | 10 and 20 m/s |
| Acoustic route-choice weight | 0 and 0.70 km |

Table 1. One-at-a-time Phase E3 settings around the reference acoustic fleet model.

The physical budgets remain anchored to the nominal reference mission at 80 m, 15 m/s, and 88.4 dB(A). Varied source levels, altitude, and speed therefore change exposure relative to a fixed budget instead of redefining the budget.

## Reference policy comparison

Table 2 reports means and 95 percent Monte Carlo intervals across the 30 seeds.

| Demand | Policy | Completion (%) | Final queue | Peak period LAE (dB) |
| --- | --- | ---: | ---: | ---: |
| Medium | P0 | 71.1 (70.6 to 71.6) | 440.2 (429.3 to 451.1) | 79.9 (79.9 to 79.9) |
| Medium | P1 | 69.3 (68.8 to 69.8) | 468.4 (457.2 to 479.6) | 79.4 (79.3 to 79.4) |
| Medium | P2 | 71.0 (70.5 to 71.5) | 442.1 (431.2 to 453.1) | 76.1 (76.1 to 76.1) |
| Medium | P3-U | 67.2 (66.7 to 67.7) | 499.7 (488.2 to 511.3) | 76.5 (76.4 to 76.6) |
| Medium | P3-D | 49.4 (49.0 to 49.8) | 771.0 (758.8 to 783.2) | 76.1 (76.1 to 76.2) |
| High | P0 | 41.2 (41.0 to 41.5) | 1,573.4 (1,556.1 to 1,590.7) | 79.9 (79.9 to 79.9) |
| High | P1 | 39.9 (39.7 to 40.2) | 1,608.3 (1,591.0 to 1,625.6) | 79.3 (79.3 to 79.4) |
| High | P2 | 41.2 (40.9 to 41.4) | 1,575.5 (1,558.2 to 1,592.8) | 76.1 (76.1 to 76.1) |
| High | P3-U | 38.4 (38.2 to 38.7) | 1,648.7 (1,631.3 to 1,666.1) | 76.5 (76.5 to 76.6) |
| High | P3-D | 28.1 (27.9 to 28.3) | 1,924.9 (1,907.7 to 1,942.1) | 76.1 (76.1 to 76.1) |

Table 2. Reference Phase E2 outcomes under medium and high demand.

Physical mission duration makes fleet capacity binding. P0 completes about 71 percent of medium-demand requests and 41 percent of high-demand requests, even though it has no exposure budget. This means policy comparisons must separate acoustic constraints from the two-drone capacity limit.

P2 reduces peak LAE by about 3.8 dB relative to P0 without a material completion loss in the reference 20-mission setting. P3-U reduces service, while P3-D performs substantially worse than P3-U. Damping does not stabilize service in this configuration.

## Fixed-budget response

Table 3 shows the high-demand P2 response to the three initial fixed budgets.

| Budget | Completion (%) | Final queue | Peak period LAE (dB) |
| --- | ---: | ---: | ---: |
| 10 missions | 24.1 (23.9 to 24.3) | 2,032.0 (2,014.4 to 2,049.6) | 73.0 (72.9 to 73.0) |
| 20 missions | 41.2 (40.9 to 41.4) | 1,575.5 (1,558.2 to 1,592.8) | 76.1 (76.1 to 76.1) |
| 30 missions | 41.2 (41.0 to 41.5) | 1,573.7 (1,556.4 to 1,591.0) | 77.9 (77.9 to 77.9) |

Table 3. High-demand P2 outcomes across fixed reference-mission budgets.

The 10-mission budget is strongly restrictive. Raising the budget from 20 to 30 missions does not improve completion because fleet capacity has already become the binding constraint. It increases allowable peak exposure by about 1.8 dB.

## Acoustic and operational sensitivity

Table 4 reports paired changes in high-demand completion relative to each policy's reference setting. The confidence intervals use within-seed differences.

| Change | P2 difference (percentage points) | P3-U difference | P3-D difference |
| --- | ---: | ---: | ---: |
| Source level -1 dB | +0.1 | +2.3 | +11.7 |
| Source level +1 dB | -0.8 | -7.9 | +3.1 |
| Validation stress -2.29 dB | +0.1 | +1.4 | +10.2 |
| Validation stress +2.29 dB | -11.6 | -15.5 | -4.9 |
| Altitude 60 m | -3.5 | -9.8 | +1.3 |
| Altitude 120 m | +0.1 | -4.1 | +4.4 |
| Speed 10 m/s | -10.7 | -12.9 | -2.5 |
| Speed 20 m/s | +7.3 | +8.7 | +16.4 |

Table 4. Paired high-demand completion-rate differences from the reference setting.

Speed affects both mission capacity and sound-exposure duration, so it is the strongest operational sensitivity in this design. A positive 2.29 dB validation-error stress also causes a material reduction for every regulated policy.

P3-D is non-monotonic. Both lower and higher initial budgets improve completion relative to its 20-mission reference, and both -1 and +1 dB source changes also improve it. This indicates feedback dynamics and queue interactions around the reference setting. P3-D should not advance as a preferred controller until its trajectories, stability, and target rule are diagnosed.

## Population-weighted route choice

Table 5 isolates the high-demand P1 route-weight setting.

| Route weight | Completion (%) | Population-weighted exposure equivalent | Route shares north / central / south (%) |
| --- | ---: | ---: | ---: |
| 0 km | 41.2 (41.0 to 41.5) | 810.5 (810.2 to 810.8) | 0.0 / 100.0 / 0.0 |
| 0.35 km | 39.9 (39.7 to 40.2) | 146.5 (146.4 to 146.5) | 79.9 / 0.0 / 20.1 |
| 0.70 km | 39.9 (39.7 to 40.2) | 146.5 (146.4 to 146.5) | 79.9 / 0.0 / 20.1 |

Table 5. High-demand P1 outcomes across acoustic route-choice weights.

Introducing the acoustic term changes route choice discontinuously between weights 0 and 0.35 km. The population-weighted exposure measure falls by about 82 percent, while completion falls by 1.3 percentage points because the selected routes take longer. Increasing the weight to 0.70 km causes no further change in this network, so later work should test values around the route-switching threshold.

## Progression decision

Phase E3 passes the software and reproducibility gate, but it does not support selecting P3-D. The next development step should diagnose its budget trajectories and feedback stability before adding the Kawai annoyance response.

The experiment remains limited to one synthetic network, concentrated population, two drones, two chargers, fixed demand structure, and one-at-a-time sensitivities. The validation-error levels are deterministic screening stresses equal to plus or minus one RMSE. They are not prediction intervals. The Monte Carlo intervals measure demand-seed variation and do not include empirical parameter-estimation uncertainty.

## Reproducibility files

- [Run-level results](results/phase_e_runs.csv)
- [Configuration summaries](results/phase_e_summary.csv)
- [Matched-seed paired effects](results/phase_e_paired_effects.csv)
- [Execution metadata](results/phase_e_metadata.json)
