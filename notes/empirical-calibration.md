# Empirical calibration planning

The supplied `Drone Simulation 1` folder provides a usable acoustic calibration package and two optional behavioral datasets. It does not yet provide the operational demand, battery, charging, population, or complaint observations needed to calibrate the full agent-based model.

> Evidence label: Dataset audit. This page records verified file contents and their proposed use. No Phase E simulation has been run with these data.

## Audit scope

The folder contains about 1.8 GB across 478 files. It includes 71 CSV files, 62 MATLAB files, 54 WAV recordings, four Excel workbooks, calibration scripts, reports, simulation outputs, and manuscript versions. The audit checked the original tables and measurement files where possible. Reports from the earlier project were treated as references, not as instructions for this project.

The raw datasets remain outside the public repository. This keeps the website small and avoids redistributing files before every licence and attribution record is checked. The public record contains dataset names, uses, limitations, and file hashes for traceability.

## Calibration map

Table 1 maps each verified source to the part of the model it can support.

| Source | Verified contents | Proposed model use | Readiness | Main limitation |
| --- | --- | --- | --- | --- |
| DroneNoise Matrice 300 | 54 audio recordings and a flyover workbook with vehicle, height, speed, payload, direction, and microphone metrics | Estimate source level, propagation, directionality, and route-segment sound exposure | Ready for acoustic method specification | One aircraft family and controlled measurement conditions |
| NASA small UAS flyovers | 62 MATLAB files with microphone pressure, microphone position, aircraft position, weather, and flight metadata | Independent validation of predicted levels by range and angle | Ready as held-out validation | Different aircraft and site; acquisition and positioning caveats are documented in the archive |
| Berke delivery-choice survey | 36,297 long-format alternatives from 3,625 respondents | Optional customer choice extension using cost, time, privacy, and free delivery | Ready only if customer choice enters the research scope | Does not identify request arrival rates, a no-order option, or complaint behavior |
| Wasatch UAS perception survey | 200 respondent records and derived resident sensitivity scores | Optional heterogeneity in resident sensitivity | Ready for distribution shape only | Does not identify an acoustic response coefficient or complaint probability |
| Jafarov last-mile survey | 53 respondent records | Descriptive context | Not suitable for coefficient calibration | Sample and design do not support the required behavioral model |
| Complaint and annoyance literature files | Laboratory response and stated complaint-intention material | Inform a later response model and its sensitivity bounds | Context only | No observed complaints linked to measured neighborhood exposure |

Table 1. Verified datasets and their proposed role in the current model.

## Strongest evidence package

The DroneNoise measurements and NASA flyovers should be used first. The prior calibration material in the folder reports a Matrice 300 received level of 88.4 dB(A) at 1 m and the following propagation form:

```text
LpA(r, theta) = LpA,1m - 20 log10(r / 1 m)
                 - 0.0015(r - 1)
                 - 3.3114(sec(theta) - 1)
```

The directivity correction was fitted to DroneNoise and then checked on 56 held-out NASA observations from 14 flights and four microphones. The existing report gives a bias of -0.12 dB, a mean absolute error of 1.77 dB, and an RMSE of 2.29 dB. These values are promising, but this project must reproduce the calculation and document every processing choice before using them as validation results.

The acoustic account must conserve energy. The model should calculate a sound-exposure quantity for every route segment, sum exposure in the linear energy domain over the review period, and convert to a logarithmic level only for reporting. It must not add decibel values directly.

## Source integrity

Table 2 records identifiers that can be checked when the calibration pipeline is created.

| File or dataset | Integrity or provenance record |
| --- | --- |
| DroneNoise flyover workbook | SHA-256 `6bf1a18f40b3b3da26f886815bbfc809f13efd371281e997d564e2c8da18e105` |
| Berke long-format choice file | 36,297 rows; no duplicate rows or missing cells; SHA-256 `97d966585fb4231b2243e90f4ccb9d0589777274e9b8a1665f0ba8312c632390` |
| Wasatch survey archive | 200 respondent rows after two metadata rows; SHA-256 `b14a2b9804db31dbd3301b921885922c59aae33e9efbe9dae8fa924c59ee7cc3` |
| Jafarov survey workbook | SHA-256 `b958666faf83ad3f4e95a5c95871b2e6eab369bcc0e9a241b9445edfe80691e9` |
| NASA archive | Flight metadata workbook plus 62 MATLAB measurement files; original data-description document retained locally |

Table 2. File checks and provenance records retained for a reproducible calibration workflow.

The local notes identify DroneNoise and Wasatch as CC BY 4.0 and the Berke repository contains an MIT licence. These records must be checked against the original distribution pages before any raw file is republished.

## What the folder cannot calibrate

Table 3 identifies the remaining empirical gaps and the model decision each gap affects.

| Missing evidence | Model quantity affected | Required next source |
| --- | --- | --- |
| Delivery request timestamps and destinations | Arrival process, demand level, temporal peaks, and spatial demand | Operator records or a documented synthetic demand design |
| Vehicle energy and charging observations | Energy per distance, reserve rule, charging duration, and charger queues | Flight logs, manufacturer curves checked against tests, or controlled measurements |
| Target-area population and geography | Neighborhood agents, route geometry, and population-weighted burden | Census population grid, boundaries, buildings, and route constraints for a named case area |
| Measured background sound | Detectability and total environmental exposure | Monitoring data or a declared background scenario |
| Complaints linked to exposure | Resident response and reporting behavior | Local complaint records with time and location, or a separate elicitation study |
| Defensible exposure budgets | Fixed and adaptive policy limits | Regulatory interpretation and an explicit policy design process |

Table 3. Data gaps that prevent full empirical calibration of the current model.

## Phase E0 sequence

The first empirical step is to replace the arbitrary exposure scale. It does not require adding all available datasets to the model at once.

1. Freeze the current synthetic model and keep its results as a verification baseline.
2. Reproduce the DroneNoise preprocessing and estimate the acoustic parameters with recorded units and uncertainty.
3. Validate predictions against the held-out NASA flyovers by flight and microphone.
4. Define route geometry, segment duration, slant distance, off-axis angle, and the review-period sound-exposure account.
5. Replace the proxy route coefficients in a calibration branch and rerun the existing invariant checks.
6. Run a limited Phase E acoustic sensitivity experiment. Retain policy budgets as declared scenarios until a policy basis is agreed.
7. Add demand, vehicle, charging, and population inputs only when their provenance and case definition are documented.

The Berke and Wasatch datasets should remain outside the core experiment for now. They are useful if the study later introduces customer choice or resident-response agents. Adding them before those mechanisms are part of the research question would expand the model without resolving the current exposure problem.

## Discussion

The dataset folder changes the immediate plan because acoustic calibration can now begin. The strongest defensible claim is narrower than full model validation: the data can support an empirically based route-exposure calculation and an independent acoustic check for a heavier multirotor reference case.

The main limitation is transfer. Aircraft type, operating state, weather, background sound, and site geometry affect received levels. NASA and DroneNoise cannot establish a universal neighborhood budget, and neither source validates service demand or charging behavior. The model should carry parameter uncertainty into Phase E and keep the policy budgets separate from the physical exposure calculation.

For the next mentor review, the project can present the acoustic equation, unit conversions, validation split, and proposed review-period account. Future work should then obtain a named case area and operational logs before interpreting results as a real deployment pilot.
