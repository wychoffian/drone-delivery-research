# Experiments and evidence

The current evidence concerns implementation checks. Research experiments with empirically anchored inputs have not yet been run.

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

The batch runner will execute the approved 4,140 matched-seed configurations. Each saved run must record the model version, parameter values, random seed, demand-stream identifier, execution environment, and input versions. Results must remain linked to that version when the model changes.

## Discussion

Proxy units cannot establish physical noise or health effects. Sensitivity analysis can characterize uncertainty in the assumptions, but cannot replace acoustic validation. The next development task is the Phase C batch runner. In parallel, the research work must define the exposure measure and audit the proposed datasets.
