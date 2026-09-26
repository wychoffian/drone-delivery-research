# Phase D targeted sensitivity analysis

Phase D tests whether selected Phase C findings depend on fleet capacity, charging capacity, fixed-budget choice, exposure coefficients, controller damping, or route tie-breaking. The analysis contains 6,780 runs from 226 unique configurations, each evaluated with the same 30 seeds used in Phase C.

> Evidence label: Sensitivity finding. These results describe changes within a synthetic model. They do not validate the exposure proxy or establish a real-world fleet or policy requirement.

## Design

The targeted design uses the medium- and high-demand concentrated-population scenarios because Phase C identified capacity pressure there. It varies one assumption group at a time around the reference model. The policy comparison includes P0, P1, P2 budgets 28, 34, and 40, P3-U, and P3-D. A separate fixed-budget module extends P2 to budgets 22 and 46.

Reported values are means with 95 percent Monte Carlo intervals across 30 matched seeds. One-at-a-time tests reveal dependence on each selected assumption. They do not estimate interactions among every sensitivity factor.

## Fleet capacity

Table 1 shows the high-demand P3-D result as fleet size increases. This test directly examines the capacity constraint found in Phase C.

| Drones | Completion rate, % | Mean wait, min | Final queue | Peak exposure, units |
| ---: | ---: | ---: | ---: | ---: |
| 2 | 55.6 (55.1 to 56.1) | 1789.2 (1749.6 to 1828.7) | 1196.7 (1171.5 to 1221.8) | 39.2 (39.2 to 39.3) |
| 4 | 69.3 (68.6 to 69.9) | 880.3 (849.6 to 911.0) | 828.2 (802.6 to 853.8) | 48.6 (46.4 to 50.7) |
| 6 | 69.3 (68.6 to 69.9) | 850.0 (819.3 to 880.8) | 828.3 (802.7 to 853.8) | 48.5 (46.4 to 50.6) |
| 8 | 69.3 (68.6 to 69.9) | 835.8 (805.3 to 866.4) | 828.2 (802.7 to 853.7) | 48.5 (46.3 to 50.6) |

Table 1. High-demand P3-D sensitivity to fleet size. Exposure is measured in proxy units.

Increasing the fleet from two to four drones changes mean completion from 55.6 to 69.3 percent. With eight drones it reaches 69.3 percent. This confirms that the Phase C high-demand queue is materially dependent on the assumed fleet size.

## Charging capacity

Table 2 tests whether charger access constrains an eight-drone fleet under high demand and P3-D.

| Charger capacity | Completion rate, % | Mean charging wait, min | Peak charger queue | Final request queue |
| --- | ---: | ---: | ---: | ---: |
| 1 | 59.5 (59.0 to 60.1) | 41.5 (41.3 to 41.7) | 7.0 (7.0 to 7.0) | 1090.2 (1064.9 to 1115.6) |
| 2 | 69.3 (68.6 to 69.9) | 9.2 (9.1 to 9.3) | 6.0 (6.0 to 6.0) | 828.2 (802.7 to 853.7) |
| unlimited | 69.3 (68.6 to 69.9) | 0.0 (0.0 to 0.0) | 0.0 (0.0 to 0.0) | 828.2 (802.7 to 853.7) |

Table 2. High-demand P3-D sensitivity to charger capacity with eight drones.

One charger produces a mean charging wait of 41.5 minutes, compared with 0.0 minutes under unlimited parallel charging. Charger capacity must therefore be stated whenever fleet size changes.

## Fixed exposure budgets

Table 3 extends the fixed-budget comparison for a four-drone fleet under high demand.

| Fixed budget | Completion rate, % | Mean wait, min | Peak exposure, units | Population-weighted burden |
| ---: | ---: | ---: | ---: | ---: |
| 22 | 50.8 (50.4 to 51.3) | 1950.1 (1910.4 to 1989.9) | 22.0 (22.0 to 22.0) | 433.2 (433.2 to 433.2) |
| 28 | 65.0 (64.5 to 65.6) | 1209.3 (1172.1 to 1246.6) | 28.0 (28.0 to 28.0) | 568.4 (568.2 to 568.6) |
| 34 | 76.0 (75.4 to 76.6) | 789.4 (767.6 to 811.3) | 34.0 (34.0 to 34.0) | 675.5 (674.4 to 676.6) |
| 40 | 85.6 (84.9 to 86.2) | 499.2 (473.3 to 525.1) | 40.0 (40.0 to 40.0) | 778.2 (777.1 to 779.4) |
| 46 | 94.2 (93.5 to 94.9) | 220.5 (192.0 to 249.0) | 46.0 (46.0 to 46.0) | 877.7 (876.4 to 879.0) |

Table 3. High-demand P2 sensitivity to the fixed neighborhood budget. Exposure is measured in proxy units.

The table provides a service-exposure frontier rather than a single preferred budget. A budget choice requires an empirically defined exposure measure and an acceptable service criterion.

## Controller damping

Table 4 tests P3-D damping with a four-drone fleet under high demand. The delay remains one period and the gain remains 0.60.

| Damping | Completion rate, % | Peak exposure, units | Budget bound share, % | Mean oscillations |
| ---: | ---: | ---: | ---: | ---: |
| 0.25 | 72.7 (72.0 to 73.4) | 47.3 (46.0 to 48.7) | 0.0 (0.0 to 0.0) | 12.4 (11.1 to 13.7) |
| 0.50 | 69.3 (68.6 to 69.9) | 48.6 (46.4 to 50.7) | 6.5 (6.5 to 6.5) | 18.9 (17.8 to 19.9) |
| 0.75 | 68.0 (67.4 to 68.7) | 48.9 (46.7 to 51.0) | 17.6 (17.6 to 17.7) | 19.7 (18.7 to 20.7) |

Table 4. High-demand P3-D sensitivity to controller damping.

## Exposure coefficient scale

Table 5 tests whether P3-D results depend on the magnitude assigned to every proxy exposure coefficient. The fleet contains four drones and charging is unlimited.

| Exposure scale | Completion rate, % | Mean wait, min | Final queue | Peak exposure, units |
| ---: | ---: | ---: | ---: | ---: |
| 0.75 | 89.3 (88.6 to 90.0) | 259.4 (232.3 to 286.4) | 288.9 (266.8 to 311.0) | 45.5 (45.3 to 45.7) |
| 1.00 | 69.3 (68.6 to 69.9) | 880.3 (849.6 to 911.0) | 828.2 (802.6 to 853.8) | 48.6 (46.4 to 50.7) |
| 1.25 | 55.3 (54.8 to 55.9) | 1482.4 (1446.3 to 1518.4) | 1203.0 (1177.4 to 1228.6) | 40.1 (39.9 to 40.4) |

Table 5. High-demand P3-D sensitivity to proportional scaling of all exposure coefficients.

Completion changes from 89.3 percent at scale 0.75 to 55.3 percent at scale 1.25. This is a material dependency. The exposure measure and its relationship to the budget must be calibrated before the Phase D progression gate can pass.

The route tie-breaking test produced completion rates of 69.3 and 69.2 percent for the lower- and higher-index rules. Peak exposure changed from 48.6 to 50.2 proxy units. This selected scenario is much less sensitive to tie-breaking than to fleet capacity or exposure scaling.

## Reproducibility files

- [Run-level results](results/phase_d_runs.csv)
- [Configuration summaries](results/phase_d_summary.csv)
- [Execution metadata](results/phase_d_metadata.json)

## Discussion

Phase D determines whether the Phase C conclusions survive selected alternative assumptions. It does not calibrate the model. Fleet and charger sensitivities are operational implications because capacity changes service outcomes. Exposure-scale and controller sensitivities show where empirical evidence and controller design are needed.

The design varies factors one group at a time in two concentrated-population scenarios. It does not cover all factor interactions, alternative network geometries, weather, failures, delivery deadlines, or measured acoustics. The [second Phase D module](phase-d-joint.html) tests destination probabilities and joint interactions among fleet size, charging, and exposure scaling. The progression gate remains open pending empirical definition of the exposure measure and its policy budgets.
