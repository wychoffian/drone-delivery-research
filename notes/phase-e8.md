# Phase E8 time-aware workload planning

Phase E8 adds route-specific flight, service, and recharge workload to the allocation optimizer. It tests whether a workload-constrained plan can retain the Phase E7 exposure-allocation benefit while reducing unfinished plans at higher budgets. The experiment contains 13,440 event-model runs from 480 demand cases, seven budgets, and four operator methods.

> Evidence label: Time-capacity diagnostic in a synthetic network. The planner still sees all requests within a review period in advance and is not yet a rolling-horizon dispatch policy.

## Design

For each route, workload is mission duration plus the time required to restore the battery used by that mission. The optimizer first maximizes mission count under destination availability, nine receiver budgets, and a two-drone workload cap. It then minimizes total workload while holding the mission count fixed.

Table 1 defines the tested methods.

| Method | Workload treatment |
| --- | --- |
| Greedy event | No period plan; dispatch uses the shortest route currently feasible. |
| Time plan 900 | Optimized routes must use at most 900 total drone-minutes. |
| Time plan 930 | Optimized routes must use at most 930 total drone-minutes. |
| Time plan 960 | Optimized routes may use the full nominal two-drone period capacity. |

Table 1. Greedy comparison and three time-aware workload limits.

The 900-minute limit is conservative. The 960-minute limit equals two drones multiplied by the 480-minute review period. It does not reserve time for late request releases or the requirement that accepted missions finish before the boundary. The 930-minute setting provides an intermediate sensitivity case.

## Executed missions

Table 2 reports mean dispatched missions per case.

| Budget | Greedy event | Time plan 900 | Time plan 930 | Time plan 960 |
| ---: | ---: | ---: | ---: | ---: |
| 14.00 | 39.0 | 39.0 | 39.0 | 39.0 |
| 14.05 | 23.0 | 39.0 | 39.0 | 39.0 |
| 14.10 | 33.0 | 41.0 | 41.0 | 41.0 |
| 14.15 | 42.0 | 42.0 | 42.0 | 42.0 |
| 16.00 | 44.5 | 43.0 | 44.9 | 44.9 |
| 17.00 | 44.8 | 43.0 | 44.9 | 45.6 |
| 18.00 | 44.9 | 43.0 | 44.9 | 45.6 |

Table 2. Mean missions dispatched by the event model after workload-aware planning.

Every time-aware setting removes the allocation discontinuity at budgets 14.05 and 14.10. The conservative 900-minute cap sacrifices service when the exposure budget is loose. The 930-minute cap approximately matches or slightly exceeds greedy dispatch at budgets 16 through 18. The 960-minute cap produces the highest mean dispatch at budgets 17 and 18.

## Plan execution reliability

Table 3 reports the share of planned missions executed and the percentage of cases that complete every planned mission.

| Budget | Workload cap | Planned missions | Plan execution (%) | Full-plan cases (%) |
| ---: | ---: | ---: | ---: | ---: |
| 14.05 | 900, 930, or 960 | 39.0 | 100.0 | 99.2 |
| 16.00 | 900 | 43.0 | 100.0 | 98.1 |
| 16.00 | 930 | 45.0 | 99.7 | 87.1 |
| 17.00 | 930 | 45.0 | 99.7 | 87.9 |
| 17.00 | 960 | 46.0 | 99.0 | 67.1 |
| 18.00 | 930 | 45.0 | 99.7 | 87.5 |
| 18.00 | 960 | 46.0 | 99.1 | 68.1 |

Table 3. Mission-level plan execution and strict full-plan completion.

The 930-minute plan leaves about one mission of nominal capacity unused but completes the entire plan in about 87 percent of higher-budget cases. The 960-minute plan dispatches about 0.7 more missions at budgets 17 and 18, while only about two thirds of cases execute every planned mission. Both execute more than 99 percent of planned missions on average.

## Waiting time

Table 4 compares waiting time for selected methods.

| Budget | Method | Mean wait (minutes) |
| ---: | --- | ---: |
| 14.05 | Greedy | 68.9 |
| 14.05 | Time plan 930 | 93.7 |
| 17.00 | Greedy | 141.5 |
| 17.00 | Time plan 930 | 113.3 |
| 18.00 | Greedy | 141.6 |
| 18.00 | Time plan 960 | 116.5 |

Table 4. Mean waiting time among dispatched missions in selected cases.

At budget 14.05, the planner serves 16 more missions and the additional missions raise the mean wait. When the budget is loose, the shorter workload mix reduces waiting time relative to the greedy rule. These outcomes confirm that throughput and waiting time must be reported together.

## Progression decision

Phase E8 passes the route-workload integration gate. Adding route-specific operational workload preserves the allocation benefit near budget 14 and substantially reduces the high-budget schedule gap. The experiment also identifies a clear tradeoff between the 930- and 960-minute limits.

The next development step is a rolling-horizon version that uses only arrived requests, replans when a drone becomes available, carries the queue across periods, and works with adaptive budgets. It should compare a reliability-oriented workload reserve with a throughput-oriented setting before the controller experiment is repeated.

## Discussion

The workload constraint aggregates two drones into one capacity account. It does not assign missions to individual drones or model exact release-time scheduling inside the optimizer. Event execution supplies those checks after planning. Advance knowledge of period demand remains a strong assumption.

The practical implication is that a single mission-count cap is insufficient, while a route-workload constraint is a useful intermediate approximation. Future research should implement receding-horizon decisions, preserve a documented fairness rule, measure solver time, and test carried backlog. Empirical pilot readiness still requires this integration and empirical demand, vehicle, population, and policy inputs.

## Reproducibility files

- [Time-aware request-route plans](results/phase_e8_time_plans.json)
- [Event-run results](results/phase_e8_time_runs.csv)
- [Budget-method summaries](results/phase_e8_time_summary.csv)
- [Execution metadata](results/phase_e8_metadata.json)
