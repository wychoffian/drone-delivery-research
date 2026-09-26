# Phase E7 event-integrated planned allocation

Phase E7 tests whether the Phase E6 optimized route allocations can be executed with within-period arrivals, two drones, charging, mission duration, and acoustic reservations. The experiment contains 13,440 event-model runs from 480 demand cases, 14 budgets, and two operator methods.

> Evidence label: Operational-integration diagnostic in a synthetic network. The planner has advance knowledge of all requests within a review period, so it is not yet a deployable rolling-horizon policy.

## Design

For each demand case and budget, Phase E6 supplies integer route counts that maximize missions under the nine-receiver exposure constraints and a 46-mission cap. Phase E7 assigns those routes to the earliest requests for each destination, then executes them in the event model. The greedy comparison uses the same requests, arrival offsets, fleet, charging rules, and budget.

Table 1 defines the comparison.

| Component | Greedy event | Planned event |
| --- | --- | --- |
| Request information | Requests become available at arrival | Planner sees the period's requests in advance; execution still waits for arrival |
| Route choice | Shortest currently feasible route | Route assigned by the Phase E6 integer plan |
| Queue selection | First request | Earliest arrived request with a planned assignment |
| Fleet and charging | Two drones and two chargers | Same |
| Exposure accounting | Full mission reserved before dispatch | Same |
| Dispatch cutoff | Mission must complete before the review boundary | Same |

Table 1. Greedy and planned operator methods in the event integration.

## Execution around the allocation discontinuity

Table 2 reports mean missions per case and planned-route execution rates across 480 cases for each budget.

| Budget | Greedy dispatch | Planned missions | Executed planned missions | Plan execution (%) | Cases completing the full plan (%) |
| ---: | ---: | ---: | ---: | ---: | ---: |
| 14.00 | 39.0 | 39.0 | 39.0 | 100.0 | 99.6 |
| 14.05 | 23.0 | 39.0 | 39.0 | 100.0 | 99.2 |
| 14.10 | 33.0 | 41.0 | 41.0 | 100.0 | 98.3 |
| 14.15 | 42.0 | 42.0 | 42.0 | 100.0 | 98.3 |
| 16.00 | 44.5 | 45.0 | 43.3 | 96.3 | 3.8 |
| 17.00 | 44.8 | 46.0 | 43.6 | 94.8 | 12.3 |
| 18.00 | 44.9 | 46.0 | 44.0 | 95.6 | 12.9 |

Table 2. Greedy dispatch, optimized plan size, and event-model execution by budget.

The planned dispatcher removes the Phase E5 discontinuity. At budget 14.05, it executes 38.99 missions on average compared with 23 for the greedy rule. At 14.10, it executes 40.98 compared with 33. Nearly every optimized mission is operationally executable through budget 15.

At budgets 16 through 18, the 45- or 46-mission exposure plan exceeds what some exact arrival and mission sequences can finish before the boundary. The planned dispatcher then completes about 43 to 44 missions, slightly fewer than the greedy event model. This confirms that the 46-mission cap is too coarse to represent route-specific duration and arrival timing.

## Waiting time and route balance

Table 3 compares mean waiting time and central-route share in selected cases.

| Budget | Method | Mean wait (minutes) | Central-route share (%) |
| ---: | --- | ---: | ---: |
| 14.05 | Greedy | 68.9 | 60.9 |
| 14.05 | Planned | 93.7 | 33.3 |
| 14.10 | Greedy | 102.2 | 42.4 |
| 14.10 | Planned | 96.3 | 31.7 |
| 18.00 | Greedy | 141.6 | 37.9 |
| 18.00 | Planned | 68.3 | 31.4 |

Table 3. Waiting time and route balance for selected event-model comparisons.

At budget 14.05, higher planned throughput increases the average wait among dispatched missions because the planner continues serving requests that the greedy method leaves in the queue. At budget 18, the planned set has a lower mean wait but executes slightly fewer missions. Waiting time must therefore remain a separate objective rather than being inferred from throughput.

## Progression decision

Phase E7 passes the exposure-allocation integration test in the problematic budget range. The optimized route mix can be executed with the event model and removes the non-monotonic service loss around budget 14. The fixed 46-mission capacity approximation does not pass at higher budgets.

[Phase E8](phase-e8.html) adds route-specific mission and recharge workload to the optimizer. It substantially reduces the higher-budget execution gap and identifies a reliability-throughput tradeoff between conservative and full-capacity plans.

## Discussion

The planned method uses advance knowledge of all requests in a period and may select a later request before an earlier unplanned request. These assumptions make it a diagnostic policy rather than a deployable rule. The experiment covers one period at a time, so it does not include carried backlog or adaptive budget updates.

The implication is that exposure allocation and operational scheduling must be solved together when the exposure constraint is loose enough for fleet timing to bind. Future research should state the operator objective, include a fairness or waiting-time rule, and measure computation time. The empirical pilot milestone remains dependent on this time-aware scheduler and on empirical demand, vehicle, population, and policy inputs.

## Reproducibility files

- [Event cases and route vectors](results/phase_e7_event_input.json)
- [Request-level optimized plans](results/phase_e7_plans.json)
- [Event-run results](results/phase_e7_event_runs.csv)
- [Budget-method summaries](results/phase_e7_event_summary.csv)
- [Execution metadata](results/phase_e7_metadata.json)
