# Phase E9 online rolling dispatch

Phase E9 replaces advance period-level planning with an online dispatcher that uses only requests already in the queue. It replans whenever a drone becomes available, carries backlog across 120 periods, and balances exposure use, route workload, and queue order. The experiment contains 600 runs from 20 configurations and 30 matched demand seeds.

> Evidence label: Online-dispatch diagnostic in a synthetic network. The rule is executable without future request knowledge, but its weights and budgets remain experimental assumptions.

## Rolling rule

At each dispatch opportunity, the rolling method examines the first `k` arrived requests. For each feasible request-route combination it calculates:

`score = peak post-dispatch budget use + 0.30 * normalized route workload + fairness weight * queue position`

The dispatcher selects the lowest-scoring combination. Route workload includes mission duration and recharge time. A fairness weight of 1 retains queue order unless exposure and workload benefits justify a limited reordering.

Table 1 defines the tested methods.

| Method | Look-ahead | Fairness weight | Information available |
| --- | ---: | ---: | --- |
| Greedy FIFO | 1 | Strict FIFO | First queued request |
| Rolling 3 fair | 3 | 1 | First three arrived requests |
| Rolling 10 fair | 10 | 1 | First ten arrived requests |
| Rolling 30 throughput | 30 | 0 | First 30 arrived requests |

Table 1. Online operator methods compared over 120 review periods.

The experiment tests fixed budgets 14.00, 14.05, 14.10, 14.15, and 18.00. Demand uses the existing high-demand scenario and increases from period 9. The queue carries across all periods.

## Service and threshold recovery

Table 2 reports full-horizon completion and the service ratio during periods 97 through 120.

| Budget | Method | Completion (%) | Late-window service ratio (%) | Final queue |
| ---: | --- | ---: | ---: | ---: |
| 14.00 | Greedy FIFO | 31.2 | 30.4 | 10,298 |
| 14.00 | Rolling 3 fair | 31.3 | 30.4 | 10,296 |
| 14.05 | Greedy FIFO | 18.4 | 17.9 | 12,216 |
| 14.05 | Rolling 3 fair | 31.3 | 30.4 | 10,296 |
| 14.10 | Greedy FIFO | 26.4 | 25.7 | 11,016 |
| 14.10 | Rolling 3 fair | 32.9 | 32.0 | 10,056 |
| 14.15 | Greedy FIFO | 33.7 | 32.8 | 9,936 |
| 14.15 | Rolling 3 fair | 33.7 | 32.8 | 9,936 |
| 18.00 | Greedy FIFO | 36.6 | 35.6 | 9,499 |
| 18.00 | Rolling 3 fair | 36.7 | 35.8 | 9,477 |
| 18.00 | Rolling 30 throughput | 36.9 | 35.9 | 9,457 |

Table 2. Long-horizon service results for selected greedy and rolling configurations.

Rolling 3 removes the non-monotonic service loss at budgets 14.05 and 14.10 without using future demand. At budget 14.05, it increases full-horizon completion by 12.9 percentage points. At budget 18, the improvement is small because fleet workload is already the main constraint.

Rolling 10 produces the same reported service as Rolling 3 in every tested budget. Rolling 30 provides a small additional gain at budget 18, but it removes the queue-position penalty and examines ten times as many requests. Rolling 3 is therefore the parsimonious rule for the next pilot stage.

## Destination and route balance

Table 3 reports destination completion and route shares at the main discontinuity.

| Method at budget 14.05 | Destination completion north / central / south (%) | Route shares north / central / south (%) |
| --- | --- | --- |
| Greedy FIFO | 18.3 / 18.4 / 18.5 | 31.3 / 60.9 / 7.8 |
| Rolling 3 fair | 31.1 / 31.3 / 31.2 | 33.3 / 33.3 / 33.3 |

Table 3. Destination completion and corridor use at budget 14.05.

The rolling rule increases service without favoring one destination. It also prevents the central-corridor concentration that exhausts a receiver account under the greedy rule. At budget 18, Rolling 3 produces destination completion rates of 36.7 percent for each destination and near-balanced corridor use.

## Waiting time and overload

Table 4 reports late-window waiting outcomes. Values are minutes because each review period contains 480 operational minutes.

| Budget | Method | Late mean wait | Mean late-period 95th percentile | Oldest queued request at period 120 |
| ---: | --- | ---: | ---: | ---: |
| 14.05 | Greedy FIFO | 40,905 | 40,970 | 45,776 |
| 14.05 | Rolling 3 fair | 34,497 | 34,616 | 38,545 |
| 18.00 | Greedy FIFO | 31,834 | 31,970 | 35,538 |
| 18.00 | Rolling 3 fair | 31,762 | 31,897 | 35,459 |

Table 4. Waiting-time outcomes under sustained demand above system capacity.

Waiting times become extremely large because the high-demand input exceeds the service capacity in every method. Rolling dispatch reduces the backlog at the problematic threshold but does not make the system stable. These values should be interpreted as evidence of overload, not as a plausible service forecast.

## Computation

Table 5 reports mean local runtime for one 120-period run.

| Budget 18 method | Runtime (milliseconds) |
| --- | ---: |
| Greedy FIFO | 86.6 |
| Rolling 3 fair | 135.7 |
| Rolling 10 fair | 221.4 |
| Rolling 30 throughput | 419.3 |

Table 5. Mean execution time in the recorded local Node.js environment.

Rolling 3 adds modest computation and matches Rolling 10 on the measured service outcomes. Runtime is environment-specific and should be measured again after implementation in the final research platform.

## Progression decision

Phase E9 passes the online rolling-dispatch gate for the synthetic fixed-budget model. Rolling 3 uses only arrived requests, carries queues across periods, removes the allocation discontinuity, preserves destination balance, and has lower computation than the larger windows.

The next development step is to connect Rolling 3 to P3-U and P3-D, repeat the long-horizon controller experiment, and compare adaptive policies with fixed budgets at matched service and exposure. That experiment should include medium demand because the high-demand scenario is structurally overloaded.

## Discussion

The rolling score and fairness weight are design choices rather than empirically calibrated behavior. The rule considers at most three requests and does not solve an exact scheduling problem. It also treats all customer requests as equal except for queue position.

The implication is that the operator layer is now adequate for renewed controller testing, but the high-demand queue cannot support a practical pilot service target. Future research should test medium demand, delivery deadlines, priority rules, and solver alternatives. Empirical pilot readiness still requires observed demand, vehicle performance, target-area population, and a defensible policy target.

## Reproducibility files

- [Run-level results](results/phase_e9_rolling_runs.csv)
- [Configuration summaries](results/phase_e9_rolling_summary.csv)
- [Period summaries](results/phase_e9_period_summary.csv)
- [Execution metadata](results/phase_e9_metadata.json)
