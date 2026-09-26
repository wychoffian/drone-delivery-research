# Experiments and evidence

The evidence includes implementation checks, synthetic stochastic screening, and a Phase E matched-seed sensitivity experiment using calibrated acoustic route increments in synthetic geometry.

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

## Phase D sensitivity

The first Phase D module contains 6,780 runs from 226 configurations. It tests fleet sizes from two to eight drones, one to unlimited chargers, fixed budgets from 22 to 46 proxy units, three exposure scales, three damping values, and two route tie-breaking rules in the medium- and high-demand concentrated-population scenarios.

[Review the Phase D sensitivity results and download the data](phase-d.html)

The second Phase D module adds 19,440 joint-sensitivity runs. It crosses three destination patterns with fleet size, charger capacity, exposure scaling, demand, and four policy variants. This tests interactions that the first one-at-a-time module could not estimate.

[Review the Phase D joint analysis and download the data](phase-d-joint.html)

## Phase E acoustic sensitivity

Phase E3 contains 3,900 runs from 130 configurations. It uses calibrated acoustic route increments in the synthetic physical network and varies reference-mission budgets, source level, held-out validation error, altitude, speed, and acoustic route weighting. Thirty common demand seeds support paired comparisons.

[Review the Phase E3 sensitivity results and download the data](phase-e3.html)

## Discussion

The Phase D results remain useful as proxy-model baselines. Phase E replaces the proxy increments with calibrated physical exposure calculations, but the network, demand, operations, and budgets remain scenarios. Monte Carlo intervals describe variation across the selected seeds. They do not include all empirical parameter uncertainty. Phase E3 shows that P3-D needs a separate stability diagnosis before policy ranking.
