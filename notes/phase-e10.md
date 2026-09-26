# Phase E10 rolling adaptive controllers

Phase E10 reconnects the Rolling 3 dispatcher to the fixed P2 policy and the adaptive P3-U and P3-D controllers. The experiment contains 1,380 runs from 46 configurations and 30 matched demand seeds. Each run covers 120 review periods and carries the queue between periods.

> Evidence label: Long-horizon controller comparison in a synthetic network. The acoustic route increments are calibrated, but demand, fleet size, controller targets, and policy budgets remain scenario assumptions.

## Experiment design

Table 1 defines the controller comparison.

| Element | Values |
| --- | --- |
| Demand | Medium 45 and high 80 requests per period before the period 9 increase |
| Fleet | Two drones and two chargers |
| Dispatcher | Rolling 3 fair |
| Fixed P2 budgets | 14.00 to 15.00 near the threshold, plus 18.00 |
| Adaptive targets | 14.00, 14.05, 14.10, 14.15, and 18.00 |
| P3-U | Gain 0.60, one-period delay, no damping |
| P3-D | Gain 0.60, one-period delay, damping 0.50 |
| Replication | 30 common seeds, 120 periods, final 24-period reporting window |

Table 1. Phase E10 experimental design.

The target is the desired receiver exposure in reference-mission equivalents. It is not a fixed operating budget. P3 changes the following period's budget when observed exposure differs from the target.

## Same-target comparison

Table 2 compares policies at the declared target or fixed budget of 14.05.

| Demand | Policy | Completion (%) | Mean population-weighted exposure | Final queue |
| ---: | --- | ---: | ---: | ---: |
| 45 | P2 fixed | 55.31 | 13.109 | 3,780 |
| 45 | P3-U | 59.71 | 14.129 | 3,408 |
| 45 | P3-D 0.50 | 60.04 | 14.207 | 3,380 |
| 80 | P2 fixed | 31.25 | 13.115 | 10,296 |
| 80 | P3-U | 33.72 | 14.129 | 9,926 |
| 80 | P3-D 0.50 | 33.91 | 14.207 | 9,898 |

Table 2. Full-horizon service and exposure at target or fixed budget 14.05.

At the same numeric setting, P3-D raises completion by 4.73 percentage points under medium demand and 2.66 points under high demand. It also raises mean population-weighted exposure by about 1.10 reference-mission equivalents. The service gain therefore cannot be interpreted independently of the additional exposure permitted by the controller.

## Nearest-exposure comparison

The fixed-budget response has plateaus because whole missions consume multi-receiver exposure vectors. Budgets from 14.15 through 15.00 produce the same aggregate result, so an exact exposure match is unavailable. Table 3 uses the closest fixed result, budget 14.15, for adaptive target 14.05.

| Demand | Adaptive policy | Adaptive completion (%) | Fixed completion (%) | Paired difference, percentage points | Exposure difference |
| ---: | --- | ---: | ---: | --- | ---: |
| 45 | P3-U | 59.71 | 59.52 | 0.19 [0.16, 0.22] | 0.022 |
| 45 | P3-D 0.50 | 60.04 | 59.52 | 0.53 [0.49, 0.56] | 0.101 |
| 80 | P3-U | 33.72 | 33.66 | 0.06 [0.06, 0.07] | 0.005 |
| 80 | P3-D 0.50 | 33.91 | 33.66 | 0.25 [0.25, 0.26] | 0.083 |

Table 3. Matched-seed completion differences against the nearest fixed-exposure configuration. Brackets contain 95 percent Monte Carlo intervals across the 30 paired seeds.

P3-D retains a small service advantage in the nearest-exposure comparison. P3-U is almost identical to the fixed policy under high demand. The differences are precise within these demand seeds, but they are small and the exposure match is approximate.

## Controller behavior

Table 4 reports late-window behavior at a reachable target and at target 18.

| Demand | Target | Policy | Late mean budget | Mean within-run budget SD | Late mean exposure | Completion (%) |
| ---: | ---: | --- | ---: | ---: | ---: | ---: |
| 45 | 14.05 | P3-U | 14.57 | 0.33 | 14.056 | 59.71 |
| 45 | 14.05 | P3-D 0.50 | 14.37 | 0.16 | 14.063 | 60.04 |
| 80 | 14.05 | P3-U | 14.56 | 0.33 | 14.055 | 33.72 |
| 80 | 14.05 | P3-D 0.50 | 14.37 | 0.16 | 14.063 | 33.91 |
| 45 | 18.00 | P3-U | 40.00 | 0.00 | 15.537 | 64.76 |
| 45 | 18.00 | P3-D 0.50 | 40.00 | 0.00 | 15.533 | 64.74 |
| 80 | 18.00 | P3-U | 40.00 | 0.00 | 15.526 | 36.70 |
| 80 | 18.00 | P3-D 0.50 | 40.00 | 0.00 | 15.544 | 36.70 |

Table 4. Late-window controller budgets and exposure with full-horizon completion.

Damping reduces late-window budget variation at target 14.05. At target 18, both adaptive controllers reach the maximum budget of 40 while exposure remains near 15.5. The two-drone fleet cannot generate the target exposure, so continued budget increases have no service value. This is a controller saturation condition.

Destination completion remains balanced. For P3-D at target 14.05, medium-demand completion is 60.09, 60.07, and 59.91 percent for north, central, and south destinations. Under high demand, the corresponding values are 33.85, 33.94, and 33.89 percent.

## Progression decision

Phase E10 passes the implementation gate: Rolling 3 works with fixed and adaptive budgets, the controllers use the correct delayed observations, and all accepted missions remain within their active budgets.

The policy-ranking gate remains open. P3-D 0.50 is the stronger adaptive candidate near target 14 because it has lower budget variation and a small service advantage in the nearest-exposure comparison. The evidence does not support target 18 because the controller saturates. Both demand scenarios also accumulate large queues after the demand increase.

The next development step is a capacity-envelope experiment. It should vary demand and fleet size with Rolling 3 and P3-D 0.50, then identify configurations where the late-window queue stops growing. Controller evaluation for a pilot should use those stable operating conditions.

## Discussion

The experiment uses a synthetic demand process, two drones, equal request priority, and one controller gain. Fixed budgets cannot exactly match every adaptive exposure outcome because missions are indivisible. Monte Carlo intervals describe variation across the selected demand seeds and do not include uncertainty in acoustic calibration, demand level, or vehicle performance.

The practical implication is that adaptive control cannot repair structural overload. Damping improves controller behavior near a reachable target, but fleet capacity must be established before the policy is interpreted. Future research should estimate a stable pilot demand range, repeat the controller comparison with observed operational inputs, and define the policy target with the relevant decision owner.

## Reproducibility files

- [Run-level results](results/phase_e10_controller_runs.csv)
- [Configuration summaries](results/phase_e10_controller_summary.csv)
- [Matched-seed effects](results/phase_e10_paired_effects.csv)
- [Period summaries](results/phase_e10_period_summary.csv)
- [Execution metadata](results/phase_e10_metadata.json)
