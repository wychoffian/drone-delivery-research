# Experiments and evidence

The evidence includes implementation checks and a completed synthetic stochastic screening. Experiments with empirically anchored inputs have not yet been run.

## Reproducible demonstration

The policy prototype uses demand seed 421 and shares the same request sequence across policies. Its default scenario uses medium demand, concentrated population, one review period of delay, and a responsiveness gain of 0.60. Demand increases by 60% from period 9.

[Run the default scenario](policy.html)

The integrated prototype applies the same request stream to two drones, charging, carried queues, exposure reservations, and five policy regimes. Each review period contains 480 operational minutes. Dispatch stops during the final 10 minutes so accepted missions complete before regulation closes the period.

[Run the integrated scenario](fleet-policy.html)

## Verified behavior

- Exposure remains within enforced budgets in P2 and P3.
- Zero demand produces zero added exposure, and zero responsiveness reproduces the fixed-budget behavior.
- The regulator uses the correct historical observation and applies updates to the next period.
- The mission walkthrough accounts for both flight legs and preserves the required battery reserve.
- The integrated model conserves requests across dispatch and the final queue, closes every period with zero active exposure reservation, and reproduces fixed-budget behavior when responsiveness is zero.

The verification suite covers 384 allocation cases and 360 integrated fleet-policy cases. These are deterministic checks rather than a statistical sample supporting a policy ranking.

## Phase C comparison

Use the same demand realizations for each policy and report delivery loss alongside exposure distribution. Compare multiple fixed budgets with adaptive policies, including comparisons at similar service levels. Otherwise, an adaptive policy can appear better merely because it becomes stricter.

The batch runner executed the approved 4,140 matched-seed runs: 138 unique configurations with 30 seeds. Each saved run records the model version, parameter values, random seed, and demand-stream identifier. The metadata records the execution environment and input version.

[Review the Phase C results and download the data](phase-c.html)

## Discussion

Proxy units cannot establish physical noise or health effects. Monte Carlo intervals describe variation across the selected seeds, while Phase D sensitivity analysis will test dependence on model assumptions. Neither replaces acoustic validation. The research work must define the exposure measure and audit the proposed datasets.
