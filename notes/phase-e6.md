# Phase E6 allocation benchmark

Phase E6 tests whether the non-monotonic service found in Phase E5 comes from the exposure budget or from the operator's greedy route-allocation rule. It compares the current first-in, first-out dispatcher with a skip-blocked variant and an integer optimization benchmark across 480 high-demand cases.

> Evidence label: Allocation-method diagnostic using calibrated acoustic route vectors in a synthetic network. The optimization is a period-level upper bound, not a replacement operational scheduler.

## Design

Each case uses the request destinations generated for periods 9 through 24 from seeds 501 through 530. The benchmark applies the same nine-receiver acoustic vectors and the same 14 budget values used in Phase E5. A 46-mission cap reflects the approximate two-drone period capacity observed in the event model.

Table 1 defines the three allocation methods.

| Method | Rule |
| --- | --- |
| FIFO-stop | Process requests in arrival order, choose the shortest feasible route, and stop when the first request has no feasible route. |
| FIFO-skip | Use the same greedy route rule, but skip an infeasible request and continue scanning later requests. |
| MILP upper bound | Choose integer mission counts for all nine destination-route combinations to maximize served missions under destination availability, exposure budgets, and the 46-mission cap. |

Table 1. Allocation methods compared in Phase E6.

The mixed-integer linear program uses one integer decision variable for each destination-route combination. It maximizes the sum of assigned missions subject to:

`sum(x[d,r] * exposure[d,r,i]) <= budget[i]` for every receiver `i`

`sum(x[d,r] over r) <= available requests[d]` for every destination `d`

`sum(x[d,r]) <= 46`

The optimization omits arrival times, charging, and mission scheduling. Its result is therefore an exposure-allocation upper bound for the current fleet capacity, not a directly executable dispatch plan.

## Allocation gap around budget 14

Table 2 reports mean missions served per case. Each budget-method cell contains 480 cases.

| Budget | FIFO-stop | FIFO-skip | MILP upper bound | FIFO gap |
| ---: | ---: | ---: | ---: | ---: |
| 14.00 | 39.0 | 39.0 | 39.0 | 0.1 |
| 14.05 | 23.0 | 23.0 | 39.0 | 16.0 |
| 14.10 | 33.0 | 33.0 | 41.0 | 8.0 |
| 14.15 | 42.0 | 42.0 | 42.0 | 0.0 |
| 14.20 | 42.0 | 42.0 | 42.0 | 0.0 |
| 18.00 | 46.0 | 46.0 | 46.0 | 0.0 |

Table 2. Mean missions served by the two greedy rules and the optimization upper bound.

At budget 14.05, the greedy dispatcher serves 23 missions while the benchmark serves 39. The greedy result is 41 percent below the upper bound. At budget 14.10, the gap is eight missions. The benchmark result is non-decreasing as the budget rises, while the greedy result falls sharply between 14.00 and 14.05.

FIFO-skip produces the same mean as FIFO-stop at every tested budget. The isolated cases therefore do not support the hypothesis that stopping at the first blocked request causes the non-monotonicity. The decisive loss occurs earlier, when shortest-feasible route choices consume a combination of receiver capacity that prevents later allocations.

## Route-balance evidence

Table 3 shows how route shares change in the problematic range.

| Budget and method | North (%) | Central (%) | South (%) | Peak budget use |
| --- | ---: | ---: | ---: | ---: |
| 14.00 FIFO-stop | 33.3 | 33.5 | 33.2 | 0.938 |
| 14.05 FIFO-stop | 31.4 | 60.9 | 7.7 | 1.000 |
| 14.05 MILP | 33.3 | 33.3 | 33.3 | 0.935 |
| 14.10 FIFO-stop | 41.6 | 42.4 | 16.0 | 1.000 |
| 14.15 FIFO-stop | 33.3 | 33.3 | 33.3 | 0.999 |

Table 3. Route allocation and peak receiver-budget use around the discontinuity.

The budget 14.05 greedy allocation concentrates 60.9 percent of missions on the central corridor and exhausts at least one receiver account. The optimization allocates 39 missions without exhausting the same combination of accounts. This confirms that the service loss is an allocation inefficiency rather than an unavoidable consequence of the budget value.

## Progression decision

Phase E6 closes the allocation diagnosis. The current greedy dispatcher is adequate at many tested budgets but can be substantially inefficient near discrete route thresholds. It should not be used for controller ranking without an allocation robustness check.

The next development step is to add an optimization-guided period allocator to the research model while retaining the existing event simulator for mission timing, batteries, and charging. The implementation should define how a period plan responds to within-period arrivals and whether FIFO waiting time is an objective or a constraint. Controller experiments can then compare greedy and planned allocation as separate operator policies.

## Discussion

The benchmark is deliberately narrower than the event model. It assumes that all requests for a period are visible together and uses a fixed 46-mission cap. It does not prove that 39 optimized missions can be scheduled with the exact arrival, battery, and charging sequence. It measures the exposure-allocation gap under a common capacity ceiling.

The implication is that the operator needs its own explicit decision model. Future research should test rolling-horizon allocation, include mission durations and drone availability, and report computation time alongside service, exposure, and waiting-time outcomes. The empirical pilot milestone remains dependent on that operational validation and on documented real-world inputs.

## Reproducibility files

- [Benchmark cases and route vectors](results/phase_e6_benchmark_input.json)
- [Case-level results](results/phase_e6_allocation_runs.csv)
- [Budget-method summaries](results/phase_e6_allocation_summary.csv)
- [Execution metadata](results/phase_e6_metadata.json)
