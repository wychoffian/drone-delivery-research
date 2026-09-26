# Phase D joint sensitivity analysis

The second Phase D module tests destination probabilities, fleet size, charger capacity, and exposure scaling together. It contains 19,440 runs from 648 configurations, with 30 matched seeds per configuration.

> Evidence label: Sensitivity finding. These are synthetic model results. They do not specify a real fleet, exposure limit, or operational requirement.

## Design

The design crosses medium and high demand with three destination patterns, fleet sizes of 2, 4, and 8 drones, charger capacities of 1, 2, and unlimited, and exposure scales of 0.75, 1.00, and 1.25. Four policies are included: P0, P1, P2 with budget 34, and P3-D with delay 1, gain 0.60, and damping 0.50. Population remains concentrated so the experiment can isolate the joint operational effects.

Values below are means with 95 percent Monte Carlo intervals across seeds 421 through 450.

## Fleet and destination interaction

Table 1 reports high-demand P3-D completion with two chargers and the reference exposure scale.

| Destination pattern | 2 drones | 4 drones | 8 drones |
| --- | ---: | ---: | ---: |
| centered | 55.6 (55.1 to 56.1) | 69.3 (68.6 to 69.9) | 69.3 (68.6 to 69.9) |
| balanced | 56.2 (55.7 to 56.7) | 69.3 (68.6 to 69.9) | 69.3 (68.6 to 69.9) |
| edge-weighted | 56.2 (55.7 to 56.8) | 69.3 (68.6 to 69.9) | 69.3 (68.6 to 69.9) |

Table 1. Completion rate in percent for the joint fleet-size and destination-pattern sensitivity.

For centered destinations, increasing the fleet from two to four drones changes completion from 55.6 to 69.3 percent. The other destination patterns show whether this capacity effect persists when requests are redistributed.

## Exposure scale and destination interaction

Table 2 reports high-demand P3-D completion for four drones and two chargers.

| Destination pattern | Scale 0.75 | Scale 1.00 | Scale 1.25 |
| --- | ---: | ---: | ---: |
| centered | 89.3 (88.6 to 90.0) | 69.3 (68.6 to 69.9) | 55.3 (54.8 to 55.9) |
| balanced | 89.7 (89.0 to 90.5) | 69.3 (68.6 to 69.9) | 55.3 (54.8 to 55.9) |
| edge-weighted | 89.7 (89.0 to 90.5) | 69.3 (68.6 to 69.9) | 55.3 (54.8 to 55.9) |

Table 2. Completion rate in percent for the joint exposure-scale and destination-pattern sensitivity.

For centered destinations, completion changes from 89.3 percent at scale 0.75 to 55.3 percent at scale 1.25. This tests the exposure dependency while holding fleet and charger capacity fixed.

## Policy comparison across destination patterns

Table 3 compares policies under high demand with four drones, two chargers, and the reference exposure scale.

| Policy | Centered | Balanced | Edge-weighted |
| --- | ---: | ---: | ---: |
| P0 | 99.6 (99.4 to 99.8) | 99.0 (98.6 to 99.4) | 98.6 (98.1 to 99.0) |
| P1 | 92.2 (91.5 to 92.9) | 94.5 (93.8 to 95.1) | 95.8 (95.1 to 96.5) |
| P2-B34 | 76.0 (75.4 to 76.6) | 76.0 (75.4 to 76.6) | 76.0 (75.4 to 76.6) |
| P3-D | 69.3 (68.6 to 69.9) | 69.3 (68.6 to 69.9) | 69.3 (68.6 to 69.9) |

Table 3. Completion rate in percent across policies and destination patterns.

The table is a structural comparison. Service performance must still be interpreted with peak exposure, population-weighted burden, queues, and waits contained in the downloadable summary.

## Fleet and charger interaction

Table 4 reports high-demand P3-D completion for centered destinations at the reference exposure scale.

| Charger capacity | 2 drones | 4 drones | 8 drones |
| --- | ---: | ---: | ---: |
| 1 | 54.4 (53.9 to 54.9) | 59.4 (58.8 to 59.9) | 59.5 (59.0 to 60.1) |
| 2 | 55.6 (55.1 to 56.1) | 69.3 (68.6 to 69.9) | 69.3 (68.6 to 69.9) |
| unlimited | 55.6 (55.1 to 56.1) | 69.3 (68.6 to 69.9) | 69.3 (68.6 to 69.9) |

Table 4. Completion rate in percent for the joint fleet-size and charger-capacity sensitivity.

One charger limits the benefit from adding drones. Two chargers produce the same completion rate as unlimited parallel charging in these configurations, although the summary data show charging waits for larger fleets.

## Reproducibility files

- [Run-level results](results/phase_d_joint_runs.csv)
- [Configuration summaries](results/phase_d_joint_summary.csv)
- [Execution metadata](results/phase_d_joint_metadata.json)

## Discussion

This module tests interactions that the one-at-a-time analysis could not identify. It remains limited to one network, concentrated population, assumed demand, and proxy exposure. The Phase D gate can close only if the policy interpretation remains coherent across the joint settings and the remaining influential quantities are tied to evidence.

Empirical readiness still requires a physical exposure measure, defensible policy budgets, vehicle and charging data, and documented destination demand. Joint sensitivity does not replace those inputs.
