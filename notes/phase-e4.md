# Phase E4 adaptive controller diagnosis

Phase E4 diagnoses why the damped adaptive policy performed poorly in the Phase E3 reference case. The experiment contains 1,170 high-demand runs from 39 configurations, with 30 matched demand seeds per configuration.

> Evidence label: Controller-diagnosis finding from a calibrated acoustic mechanism in a synthetic network. Targets and budgets are experimental settings, not legal limits or policy recommendations.

## Design

The diagnosis separates four explanations: an overly restrictive exposure target, information delay, damping strength, and first-in-line queue blocking. P2 provides fixed-budget comparisons at 14, 18, and 20 reference missions. P3-U and P3-D start at 20 and use targets of 10, 14, or 18, delays of 0, 1, or 3 periods, and a gain of 0.60. P3-D uses damping values of 0.25, 0.50, or 0.75.

Table 1 summarizes the diagnostic design.

| Component | Levels |
| --- | --- |
| Demand | High demand, 80 expected requests before the period 9 increase |
| Fixed comparison | P2 budgets 14, 18, and 20 |
| Adaptive policies | P3-U and P3-D |
| Target | 10, 14, and 18 reference missions |
| Observation delay | 0, 1, and 3 periods |
| P3-D damping | 0.25, 0.50, and 0.75 |
| Replication | Seeds 501 through 530, shared across configurations |

Table 1. Phase E4 controller-diagnosis design.

Each run records completion, final queue, blocked dispatch attempts, central-neighborhood budget and use, period-to-period direction changes, and warm-period variation. A first-in-line event is counted only when the first request has no feasible route while at least one later queued request has a feasible route.

## Reference diagnosis

Table 2 compares the original target 14 and delay 1 configurations. Means and 95 percent Monte Carlo intervals use the same 30 demand seeds.

| Policy | Completion (%) | Final queue | Warm budget mean | Warm dispatch mean | Warm dispatch SD | Direction changes |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| P3-U | 38.4 (38.2 to 38.7) | 1,648.7 | 14.44 | 42.98 | 4.65 | 4 |
| P3-D, damping 0.25 | 40.0 (39.7 to 40.2) | 1,606.7 | 14.89 | 45.19 | 0.74 | 0 |
| P3-D, damping 0.50 | 28.1 (27.9 to 28.3) | 1,924.9 | 14.09 | 25.77 | 14.03 | 0 |
| P3-D, damping 0.75 | 38.3 (38.0 to 38.6) | 1,650.7 | 14.34 | 41.50 | 6.87 | 4 |

Table 2. High-demand results at target 14 and delay 1 across adaptive controller variants.

The original P3-D rule with damping 0.50 is not unstable in the sense of an oscillating budget. Its central budget falls monotonically from 20 toward 14 and records no direction changes. The service loss begins when the budget crosses a discrete route-feasibility threshold near 14.16 reference missions. Dispatch then falls from about 45 missions per period to 14 by period 18, while the queue continues to grow.

The damping 0.25 configuration retains high service because it approaches the target more slowly and remains above the threshold during the 24-period horizon. This is a transient benefit, not evidence that 0.25 is a superior steady-state controller. Damping 0.75 crosses the threshold earlier, then its delayed updates produce four direction changes and a partial recovery.

## Target and fixed-budget comparison

Table 3 places the adaptive result beside fixed P2 budgets and the best-performing tested adaptive configuration over the current horizon.

| Configuration | Completion (%) | Final queue | Blocked attempts | Warm dispatch mean |
| --- | ---: | ---: | ---: | ---: |
| P2, budget 14 | 35.0 (34.8 to 35.2) | 1,740.6 | 410.1 | 39.00 |
| P2, budget 18 | 40.9 (40.7 to 41.2) | 1,581.7 | 0.0 | 45.67 |
| P2, budget 20 | 41.2 (40.9 to 41.4) | 1,575.5 | 0.0 | 45.96 |
| P3-D, target 14, delay 1, damping 0.50 | 28.1 (27.9 to 28.3) | 1,924.9 | 919.4 | 25.77 |
| P3-D, target 18, delay 1, damping 0.25 | 41.1 (40.8 to 41.4) | 1,576.9 | 0.0 | 45.87 |

Table 3. Fixed-budget benchmarks and selected P3-D configurations under high demand.

The target is the main substantive control. A target of 18 preserves almost all fleet-limited service over 24 periods, while target 14 lies close to the model's route-feasibility threshold. P2 at budget 14 still completes 35.0 percent because it stays exactly at 14. P3-D with target 14 approaches the target from above and crosses several continuous budget values that change the number and sequence of feasible missions. The adaptive trajectory therefore cannot be inferred from a fixed policy with the same nominal number.

## Queue diagnosis

First-in-line blocking does not explain the poor reference result. P3-D with damping 0.50 records a mean of 919.4 blocked dispatch attempts but only 1.0 first-in-line event per run. Across the other main comparisons the first-in-line count is zero or near zero. When a queued request is blocked, later destinations almost never have a feasible route under the same neighborhood accounts.

The blocking mechanism is therefore system-wide exposure feasibility at the current budget vector. Changing the queue discipline would not remove the main loss observed here.

## Progression decision

Phase E4 closes the controller-stability diagnosis but does not identify a preferred policy. The earlier description of P3-D as non-monotonic around the reference setting is replaced by a more specific explanation: continuous budget updates cross discrete mission-feasibility thresholds, and the 24-period horizon can make a slowly converging controller appear better because it has not reached its target.

[Phase E5](phase-e5.html) extends the horizon to 120 periods and tests target values densely around the route thresholds. It shows that the apparent advantage of slow damping does not persist and identifies a non-monotonicity in the greedy route allocator. The annoyance-response module should remain separate until the policy target has an empirical or explicitly normative basis.

## Discussion

The experiment uses one synthetic network, two drones, a fixed 24-period horizon, concentrated population, and one demand profile. Its deterministic controller parameters are not estimated from field data. The apparent advantage of damping 0.25 at targets 14 and 18 depends on slow convergence during the chosen horizon. A longer run may remove that advantage.

The practical implication is that controller comparison requires trajectory evidence. End-of-run completion alone confounds the target, convergence speed, route thresholds, and experiment duration. Future research should map threshold locations, extend runs until budgets settle, compare policies at matched exposure and service, and then test any empirically supported annoyance response as a separate outcome model.

## Reproducibility files

- [Run-level results](results/phase_e4_controller_runs.csv)
- [Configuration summaries](results/phase_e4_controller_summary.csv)
- [Period summaries](results/phase_e4_period_summary.csv)
- [Execution metadata](results/phase_e4_metadata.json)
