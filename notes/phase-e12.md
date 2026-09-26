# Phase E12 final synthetic pilot

Phase E12 runs the selected pilot configuration for 240 review periods across 100 matched seeds. It compares all proposal regimes and evaluates the preregistered service, queue, exposure, destination-balance, and controller-stability gates.

> Evidence label: Frozen synthetic simulation pilot. It supports conclusions about model behavior under declared assumptions. It does not establish field readiness or an empirical policy recommendation.

## Frozen configuration

Table 1 records the final pilot configuration.

| Component | Value |
| --- | --- |
| Demand | Mean 30 requests per period, increasing by 60% from period 9 |
| Fleet | Four drones and four chargers |
| Horizon | 240 periods with a 48-period final evaluation window |
| Replication | 100 matched seeds, 901 through 1000 |
| Dispatcher | Rolling 3 fair |
| Adaptive target | 16.3 reference-mission equivalents |
| Information delay | One period |
| Gain | 0.60 |
| Selected damping | 0.10 |

Table 1. Frozen Phase E12 synthetic pilot configuration.

## Progression gates

Table 2 reports the selected P3-D 0.10 result against every progression gate.

| Gate | Threshold | Observed mean | Passing seeds | Result |
| --- | ---: | ---: | ---: | --- |
| Completion | At least 99% | 99.98% | 100 / 100 | Pass |
| Late queue slope | At most 0.1 | -0.002 | 100 / 100 | Pass |
| Late mean waiting | At most 30 minutes | 1.70 | 100 / 100 | Pass |
| Oldest queued request | At most 480 minutes | 10.4 | 100 / 100 | Pass |
| Destination completion gap | At most 2 percentage points | 0.034 points | 100 / 100 | Pass |
| Budget violations | Zero | 0 | 100 / 100 | Pass |
| Late budget SD | At most 1 | 0.404 | 98 / 100 | Pass |
| Late budget saturation | At most 25% | 0% | 100 / 100 | Pass |
| All gates | At least 95% of seeds | 98% | 98 / 100 | Pass |

Table 2. Final progression-gate evaluation for P3-D 0.10.

The final queue is small and has a declining late-window trend. The controller remains below its maximum budget and produces no accepted mission that exceeds an active receiver budget.

## Policy comparison

Table 3 compares service and exposure across the five regimes.

| Policy | Completion (%) | Late mean wait | Population-weighted exposure | Maximum mean receiver exposure | Receiver-exposure Gini |
| --- | ---: | ---: | ---: | ---: | ---: |
| P0 shortest path | 99.98 | 1.55 | 34.85 | 47.44 | 0.657 |
| P1 noise aware | 99.98 | 1.71 | 6.50 | 37.94 | 0.527 |
| P2 fixed budget | 99.21 | 742.58 | 15.84 | 15.86 | 0.001 |
| P3-U | 99.95 | 31.20 | 15.96 | 15.98 | 0.002 |
| P3-D 0.10 | 99.98 | 1.70 | 15.93 | 15.94 | 0.000 |

Table 3. Full-horizon service and exposure outcomes across the proposal policy regimes.

P0 concentrates every route on the central corridor. P1 sends about 80% of routes through the north corridor and 20% through the south corridor. Its low population-weighted exposure therefore hides substantial transfer from the concentrated central population to the outer receivers. The mean outer-to-central receiver-exposure ratio is 100.02 under P1.

P2, P3-U, and P3-D distribute corridor use almost equally. P2 produces a growing queue and fails the waiting-time gate. P3-U misses the waiting and controller-variation gates. P3-D 0.10 combines balanced receiver exposure with stable service and controller behavior.

## Freeze decision

The synthetic pilot passes its progression gate and is frozen as `phase-e12-synthetic-pilot-v1`. The freeze manifest records SHA-256 hashes for the model, runner, results, gate evaluation, and metadata. Changes to those files create a new model version rather than modifying this result in place.

This freeze applies only to the synthetic simulation pilot. Empirical calibration and field-pilot authorization remain separate stages.

## Discussion

The result depends on synthetic demand, synthetic geometry, assumed population concentration, equal request priority, and one charger per drone. Acoustic route increments use transferred calibration data, while the operational and socioeconomic inputs remain uncalibrated. P1's apparent population-weighted benefit demonstrates why average burden cannot replace receiver-level distribution measures.

The simulation implication is specific: under these assumptions, P3-D with damping 0.10 passes the declared pilot gates while the fixed and undamped regulated alternatives do not. Future research should replace the synthetic demand, fleet, population, and urban-form inputs before treating this configuration as evidence for a location or field operation.

## Frozen reproducibility files

- [Run-level results](results/phase_e12_pilot_runs.csv)
- [Policy summaries](results/phase_e12_pilot_summary.csv)
- [Period summaries](results/phase_e12_period_summary.csv)
- [Gate evaluation](results/phase_e12_gate_evaluation.csv)
- [Execution metadata](results/phase_e12_metadata.json)
- [Freeze manifest](results/phase_e12_freeze_manifest.json)
- [Frozen model source](source/acoustic-fleet.html)
- [Frozen final-pilot runner](source/phase-e12.cjs)
