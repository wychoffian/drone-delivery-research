# Phase E11 pilot capacity envelope

Phase E11 identifies a stable synthetic pilot configuration before the final policy comparison. It contains four sequential screens with 4,230 runs across 141 configurations. Every screen uses Rolling 3 dispatch, carried queues, matched demand seeds, and 120 review periods.

> Evidence label: Synthetic pilot-design experiment. The selected operating point is suitable for the final simulation pilot, but it is not a recommendation for a field fleet or legal exposure limit.

## Preregistered progression gates

Table 1 defines the gates used before inspecting the results.

| Outcome | Gate |
| --- | ---: |
| Completion | At least 99% |
| Late queue slope | At most 0.1 requests per period |
| Late mean waiting | At most 30 minutes |
| Oldest queued request | At most 480 minutes |
| Destination completion gap | At most 2 percentage points |
| Late controller budget SD | At most 1 reference mission |
| Late budget saturation | At most 25% of periods |
| Configuration acceptance | At least 95% of seeds pass every gate |

Table 1. Progression gates for selecting the final synthetic pilot configuration.

## Fleet and demand screen

The first screen tests eight demand levels and fleets from one to six drones at adaptive target 14.05 with damping 0.50. Table 2 reports the boundary configurations.

| Demand | Fleet | Stable seeds (%) | Completion (%) | Final queue | Late mean wait |
| ---: | ---: | ---: | ---: | ---: | ---: |
| 25 | 2 | 93 | 99.9 | 4.1 | 34.8 |
| 25 | 3 | 97 | 100.0 | 2.1 | 3.4 |
| 25 | 4 | 100 | 100.0 | 1.9 | 1.0 |
| 30 | 4 | 0 | 90.3 | 544.2 | 4,582.0 |
| 45 | 4 | 0 | 60.3 | 3,345.2 | 19,806.7 |

Table 2. Capacity boundary under target 14.05 and damping 0.50.

Adding drones cannot stabilize demand 30 at target 14.05 because exposure control, rather than fleet capacity, becomes binding. The next screen therefore varies the target without changing the progression gates.

## Target and responsiveness refinement

The target screens cover 14.05 through 17.00. They locate a transition near target 16, where service becomes stable but damping 0.50 still produces excessive seed-level budget variation. The final responsiveness screen tests damping from 0.20 to 0.50.

Table 3 reports the selected target with two damping settings.

| Demand | Fleet | Target | Damping | Seeds passing every gate (%) | Completion (%) | Late budget SD | Saturation (%) |
| ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| 30 | 4 | 16.3 | 0.50 | 37 | 99.97 | 1.16 | 6.3 |
| 30 | 4 | 16.3 | 0.20 | 97 | 99.97 | 0.50 | 0.0 |

Table 3. Responsiveness refinement at the selected demand, fleet, and target.

Damping 0.20 meets the screening gates without saturation. Targets 16.4 and 16.5 also pass with damping 0.20. The experiment selects 16.3 because it is the lowest passing target.

## Selected pilot configuration

Table 4 records the configuration advanced to the independent final run.

| Component | Frozen candidate value |
| --- | --- |
| Initial demand mean | 30 requests per period |
| Demand increase | 60% from period 9 |
| Fleet and charging | Four drones and four chargers |
| Adaptive policy | P3-D |
| Exposure target | 16.3 reference-mission equivalents |
| Gain and delay | 0.60 and one period |
| Damping | 0.20 for the screening candidate |
| Dispatcher | Rolling 3 fair |

Table 4. Configuration selected for the first independent final-pilot run.

The first 240-period run found that damping 0.20 passed every gate in only 75% of new seeds because its longer reporting window exposed additional budget variation. The model retained every preregistered threshold and reduced damping to 0.10. This single corrective change was then rerun across the same complete policy comparison. [Phase E12](phase-e12.html) records the accepted result and freeze.

## Discussion

The capacity boundary depends on the synthetic demand process, mission times, route geometry, and transferred acoustic model. The screens vary fleet size, target, and damping in stages rather than as one exhaustive factorial design.

The implication is that fleet expansion alone cannot resolve an exposure-constrained queue. A stable pilot requires a compatible demand level, exposure target, and controller response. Future research should repeat the envelope with observed demand, vehicle charging data, and a target defined with the relevant decision owner.

## Reproducibility files

- [Fleet and demand runs](results/phase_e11_capacity_runs.csv)
- [Fleet and demand summaries](results/phase_e11_capacity_summary.csv)
- [Fleet and demand period summaries](results/phase_e11_period_summary.csv)
- [Fleet and demand metadata](results/phase_e11_metadata.json)
- [Target-screen runs](results/phase_e11_target_runs.csv)
- [Target-screen summaries](results/phase_e11_target_summary.csv)
- [Fine-target runs](results/phase_e11_refinement_runs.csv)
- [Fine-target summaries](results/phase_e11_refinement_summary.csv)
- [Responsiveness runs](results/phase_e11_damping_runs.csv)
- [Responsiveness summaries](results/phase_e11_damping_summary.csv)
- [Fleet and demand runner](source/phase-e11.cjs)
- [Target-screen runner](source/phase-e11b.cjs)
- [Fine-target runner](source/phase-e11c.cjs)
- [Responsiveness runner](source/phase-e11d.cjs)
