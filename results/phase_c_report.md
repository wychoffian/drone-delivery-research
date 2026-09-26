# Phase C matched-seed screening

Phase C is complete for the approved synthetic experiment. The batch contains 4,140 runs: 138 unique configurations evaluated with 30 matched seeds from 421 through 450.

> Evidence label: Synthetic experiment finding. These results describe model behavior under assumed inputs. They are not measurements of physical noise, health effects, public acceptance, or commercial operations.

## Method

Each run covers 24 review periods of 480 operational minutes. Policies within a demand setting and seed receive the same request stream. The experiment crosses three demand levels with two population distributions. P2 uses fixed budgets of 28, 34, and 40 proxy units. P3-U and P3-D cross delays of 0, 1, and 3 periods with gains of 0.15, 0.60, and 1.50. P3-D applies a damping factor of 0.50.

Reported intervals are 95 percent Monte Carlo confidence intervals across the 30 seeds, calculated as the sample mean plus or minus the t critical value for 29 degrees of freedom. They quantify simulation uncertainty for this seed sample. They do not include parameter, measurement, or model-structure uncertainty.

## Selected comparison

Table 1 presents the preselected medium-demand, concentrated-population comparison. Values are means with 95 percent Monte Carlo intervals. The adaptive settings are delay 1 and gain 0.60, and the fixed-budget comparator uses 40 proxy units.

| Regime | Completion rate, % | Mean wait, min | Peak exposure, units | Population-weighted burden | Final queue |
| --- | ---: | ---: | ---: | ---: | ---: |
| P0 | 96.2 (95.5 to 97.0) | 136.3 (110.2 to 162.5) | 86.1 (85.0 to 87.3) | 1030.3 (1023.6 to 1037.1) | 57.3 (45.2 to 69.5) |
| P1 | 86.2 (85.3 to 87.1) | 477.5 (444.3 to 510.8) | 52.1 (51.5 to 52.7) | 157.9 (157.1 to 158.8) | 208.6 (193.7 to 223.5) |
| P2, budget 40 | 91.6 (90.7 to 92.5) | 287.5 (253.7 to 321.2) | 39.2 (39.2 to 39.2) | 664.0 (662.7 to 665.2) | 127.5 (112.7 to 142.3) |
| P3-U, delay 1, gain 0.60 | 89.7 (88.8 to 90.6) | 345.3 (311.4 to 379.1) | 41.4 (40.8 to 41.9) | 530.9 (530.0 to 531.7) | 155.8 (140.8 to 170.8) |
| P3-D, delay 1, gain 0.60 | 89.8 (88.9 to 90.7) | 342.6 (308.9 to 376.3) | 41.6 (41.1 to 42.1) | 540.9 (540.1 to 541.7) | 153.9 (139.1 to 168.8) |

Table 1. Selected medium-demand comparison across 30 matched seeds. Exposure is measured in proxy units.

The default damped adaptive setting completed 89.8 percent of requests, with a mean final queue of 153.9. P0 completed 96.2 percent. This difference must be interpreted together with the exposure and distribution measures rather than as a standalone policy ranking.

## High-demand stress comparison

Table 2 applies the same policy settings under high demand and concentrated population. This scenario tests queue growth when demand rises by 60 percent from period 9.

| Regime | Completion rate, % | Mean wait, min | Peak exposure, units | Population-weighted burden | Final queue |
| --- | ---: | ---: | ---: | ---: | ---: |
| P0 | 61.5 (61.0 to 62.1) | 1498.9 (1458.8 to 1538.9) | 87.2 (86.3 to 88.1) | 1176.0 (1170.5 to 1181.5) | 1036.6 (1011.3 to 1061.9) |
| P1 | 52.5 (52.0 to 53.0) | 1991.8 (1951.7 to 2031.8) | 52.5 (52.1 to 52.8) | 171.9 (171.8 to 172.1) | 1280.2 (1254.5 to 1305.9) |
| P2, budget 40 | 57.1 (56.5 to 57.6) | 1730.7 (1691.4 to 1770.1) | 39.2 (39.2 to 39.3) | 685.0 (684.8 to 685.2) | 1156.4 (1130.9 to 1181.8) |
| P3-U, delay 1, gain 0.60 | 55.4 (54.9 to 56.0) | 1806.9 (1767.4 to 1846.4) | 39.3 (39.2 to 39.3) | 548.0 (547.8 to 548.1) | 1200.3 (1174.8 to 1225.7) |
| P3-D, delay 1, gain 0.60 | 55.6 (55.1 to 56.1) | 1789.2 (1749.6 to 1828.7) | 39.2 (39.2 to 39.3) | 558.1 (557.9 to 558.3) | 1196.7 (1171.5 to 1221.8) |

Table 2. Selected high-demand stress comparison across 30 matched seeds. Exposure is measured in proxy units.

Under the default damped adaptive setting, the high-demand scenario completed 55.6 percent of requests and ended with a mean queue of 1196.7. The result is a capacity and policy interaction within the current two-drone model.

## Reproducibility files

- [Run-level results](results/phase_c_runs.csv)
- [Configuration summaries](results/phase_c_summary.csv)
- [Execution metadata](results/phase_c_metadata.json)

Every run records the policy, factor settings, seed, demand-stream identifier, model version, and protocol version. The metadata file records the runtime environment and full seed list.

## Discussion

The screening tests whether the implementation produces stable and interpretable contrasts across the approved factor grid. It does not justify a preferred regulatory policy. Comparisons at similar service levels are needed because tighter exposure budgets can reduce deliveries and queue performance.

The model uses proxy exposure coefficients, a synthetic three-route network, identical drones, one charging rule, and no weather, failures, delivery deadlines, or socioeconomic group attributes. The population-weighted burden measure changes route cost and summarizes exposure distribution, but it is not a validated health or annoyance measure.

Phase D should test the assumptions that can change the comparison: fixed-budget spacing, damping, exposure coefficients, destination probabilities, fleet size, charging capacity, and route tie-breaking. The research work should define a physical exposure measure and document data provenance before any empirical policy claim.
