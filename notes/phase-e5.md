# Phase E5 long-horizon threshold analysis

Phase E5 tests whether the 24-period controller comparisons persist over a longer horizon and maps the target region around the route-feasibility threshold. The experiment contains 2,100 high-demand runs from 70 configurations, with 30 matched demand seeds and 120 review periods per run.

> Evidence label: Long-horizon diagnostic finding from a calibrated acoustic mechanism in a synthetic network. The experiment identifies model behavior that must be resolved before policy selection.

## Design

The analysis holds demand, population, observation delay, and controller gain constant. It varies fixed budgets and adaptive targets from 13 to 18, with dense coverage from 14.00 to 14.25. Every P3-D target is tested with damping 0.25, 0.50, and 0.75. P3-U and P2 provide adaptive and fixed comparisons.

Table 1 summarizes the experiment.

| Component | Setting |
| --- | --- |
| Horizon | 120 review periods |
| Demand | High demand, with the existing increase from period 9 |
| Targets and fixed budgets | 13, 13.5, 13.75, 14, 14.05, 14.10, 14.15, 14.20, 14.25, 14.5, 15, 16, 17, and 18 |
| Adaptive policies | P3-U and P3-D |
| P3-D damping | 0.25, 0.50, and 0.75 |
| Observation delay and gain | Delay 1 and gain 0.60 |
| Replication | Seeds 501 through 530 |
| Late window | Periods 97 through 120 |

Table 1. Phase E5 long-horizon threshold design.

The analysis reports completion over the full horizon, completion during periods 1 through 24, the late-window service ratio, budget variation, route shares, and final queue composition. The late window describes the end of the simulated horizon. It is not assumed to be a mathematical steady state.

## The 24-period result does not persist

Table 2 compares the first and final 24 periods for selected target 14 and target 18 controllers.

| Configuration | First 24 completion (%) | Late-window service ratio (%) | Late dispatch per period | Late central-route share (%) |
| --- | ---: | ---: | ---: | ---: |
| P3-U, target 14 | 38.4 | 32.6 | 41.8 | 33.1 |
| P3-D 0.25, target 14 | 40.0 | 10.9 | 14.0 | 100.0 |
| P3-D 0.50, target 14 | 28.1 | 12.4 | 15.9 | 90.9 |
| P3-D 0.75, target 14 | 38.3 | 31.5 | 40.4 | 34.2 |
| P3-U, target 18 | 40.5 | 35.1 | 45.0 | 39.8 |
| P3-D 0.25, target 18 | 41.1 | 14.0 | 18.0 | 100.0 |
| P3-D 0.50, target 18 | 36.1 | 14.0 | 18.0 | 100.0 |
| P3-D 0.75, target 18 | 39.9 | 34.2 | 43.8 | 40.9 |

Table 2. Early and late service outcomes for selected adaptive controllers across 120 periods.

The apparent advantage of damping 0.25 in Phase E4 is transient. At target 18, it falls from 41.1 percent completion during the first 24 periods to a late-window service ratio of 14.0 percent. It dispatches exactly 18 missions per late period, all through the central corridor. Damping 0.50 reaches the same route lock-in earlier.

The 0.75 controller and P3-U avoid permanent lock-in in these cases, but their budgets and dispatch counts continue to vary. Their higher late service does not establish superiority because the route-allocation mechanism itself fails a monotonicity check.

## Fixed-budget monotonicity failure

Table 3 maps the narrow fixed-budget region where a small budget increase changes the greedy allocation sequence.

| P2 budget | First 24 completion (%) | Late-window service ratio (%) | Late dispatch per period | Central-route share (%) |
| ---: | ---: | ---: | ---: | ---: |
| 14.00 | 35.0 | 30.4 | 39.0 | 33.4 |
| 14.05 | 20.6 | 18.0 | 23.0 | 60.9 |
| 14.10 | 29.6 | 25.7 | 33.0 | 42.4 |
| 14.15 | 37.7 | 32.8 | 42.0 | 33.3 |
| 14.20 | 37.7 | 32.8 | 42.0 | 33.3 |
| 18.00 | 40.9 | 35.6 | 45.6 | 37.3 |

Table 3. Fixed-budget service and route allocation around the target 14 threshold.

Increasing the fixed budget from 14.00 to 14.05 reduces late service from 30.4 to 18.0 percent. This is not a plausible general response to a relaxed constraint. It occurs because the operator uses a first-in, first-out queue and selects the shortest route that is feasible at each dispatch. A small budget change alters early route choices, leaving a different pattern of residual exposure capacity. When the first queued request no longer fits any route, later requests wait even if a different earlier allocation could have served more of the queue.

The result is a property of the current greedy heuristic and continuous nine-receiver exposure vectors. It prevents a clean interpretation of target or damping effects.

## Route lock-in

At target 14 with damping 0.25, the late central budget is 14.004 reference missions and the controller dispatches 14 missions per period, all through the central corridor. At target 18, the same controller converges to 18.004 and dispatches 18 central-corridor missions. Outer neighborhood budgets relax toward their upper bounds because they receive little exposure, but alternative routes still add some energy to central receivers after their accounts are nearly exhausted.

The queue remains dominated by central-destination requests because the demand distribution assigns 60 percent of requests to that destination. However, queue composition alone does not explain the loss. The decisive mechanism is the combination of greedy route order, residual capacity across all nine receivers, and a carried first-in, first-out backlog.

## Progression decision

Phase E5 closes the question raised by the 24-period horizon: slow damping does not preserve service after convergence. It also opens a more fundamental allocation gate. Controller ranking must pause until the operator rule passes a monotonicity and order-sensitivity audit.

[Phase E6](phase-e6.html) compares the current greedy dispatcher with a mixed-integer optimization that maximizes served missions under a 46-mission cap and nine-receiver exposure constraints. It measures a 16-mission gap at budget 14.05 and shows that skipping blocked requests does not close the gap.

## Discussion

The 120-period horizon is five times longer than the earlier experiment, but it does not prove convergence for controllers that continue to vary. The network has only three corridors, demand is synthetic, and the operator uses one deterministic route-order rule. The result identifies an implementation-dependent mechanism rather than a real policy response.

The implication is methodological. Feedback control cannot be evaluated independently of the decision rule that converts a budget into authorized missions. Future research should validate that allocation layer first, then repeat the controller experiment at matched exposure and service. Empirical annoyance calibration can proceed as a separate outcome-model task, but it should not be used to justify a controller whose operational allocation rule remains unresolved.

## Reproducibility files

- [Run-level results](results/phase_e5_long_horizon_runs.csv)
- [Configuration summaries](results/phase_e5_long_horizon_summary.csv)
- [Period summaries](results/phase_e5_period_summary.csv)
- [Execution metadata](results/phase_e5_metadata.json)
