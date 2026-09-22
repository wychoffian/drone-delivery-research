# Experiments and evidence

The current evidence concerns implementation checks. Research experiments with empirically anchored inputs have not yet been run.

## Reproducible demonstration

The policy prototype uses demand seed 421 and shares the same request sequence across policies. Its default scenario uses medium demand, concentrated population, one review period of delay, and a responsiveness gain of 0.60. Demand increases by 60% from period 9.

[Run the default scenario](policy.html)

## Verified behavior

- Exposure remains within enforced budgets in P2 and P3.
- Zero demand produces zero added exposure, and zero responsiveness reproduces the fixed-budget behavior.
- The regulator uses the correct historical observation and applies updates to the next period.
- The mission walkthrough accounts for both flight legs and preserves the required battery reserve.

The 384 policy runs used for checks span boundary cases and factor combinations. They are not a statistical sample supporting a policy ranking.

## Planned comparison

Use the same demand realizations for each policy and report delivery loss alongside exposure distribution. Compare multiple fixed budgets with adaptive policies, including comparisons at similar service levels. Otherwise, an adaptive policy can appear better merely because it becomes stricter.

Each saved research run should record the model version, parameter values, random seed, and data provenance. Results should remain linked to that version when the model changes.

## Discussion

Proxy units cannot establish physical noise or health effects. Sensitivity analysis can characterize uncertainty in the assumptions, but cannot replace acoustic validation. The next research task is to define the exposure measure and audit the proposed datasets.
