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
| [Kawai et al. short-term annoyance dataset](https://doi.org/10.5281/zenodo.22045972) | 2,340 repeated ratings from 36 participants, including 1,440 drone ratings and acoustic and psychoacoustic metrics | Estimate an event-level short-term annoyance or highly-annoyed response after physical exposure is calculated | Ready for a separate response submodel | Laboratory responses to single auralized events do not identify cumulative community annoyance or complaints |
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

## Short-term annoyance response

The new Kawai et al. dataset is highly relevant to a resident-response extension, but it does not replace the acoustic calibration. Its 36 participants each completed 65 trials. The 1,440 drone observations cover two drone sizes, flyby, takeoff, and landing maneuvers, fast and slow operation, and lateral distances of 10 and 50 m. The drone stimuli span approximately 54 to 79 dB LAE. The files also include a 0 to 10 annoyance rating, a highly-annoyed indicator, participant noise sensitivity, demographics, context, and several psychoacoustic measures.

The most defensible use is a repeated-measures response model in which event annoyance, or the probability of a highly-annoyed response, depends on LAE, maneuver, drone type, selected sound-quality metrics, and participant noise sensitivity. Participant-specific intercepts should account for repeated ratings. Stimulus or background effects may also need random or fixed terms. The model must be validated by holding out participants, not individual rows, because ratings from the same participant are correlated.

This dataset should enter after the physical route-exposure calculation is implemented. It can translate a modeled event into a short-term response distribution for synthetic residents. It cannot establish a legal exposure budget, predict complaint frequency, or quantify long-term community effects. The associated article describes the controlled laboratory design and reports that drones were more annoying than the comparison transport sources at the same sound exposure level, while takeoffs and landings were more annoying than flybys. [Read the JASA article](https://doi.org/10.1121/10.0032386).

## Source integrity

Table 2 records identifiers that can be checked when the calibration pipeline is created.

| File or dataset | Integrity or provenance record |
| --- | --- |
| DroneNoise flyover workbook | SHA-256 `6bf1a18f40b3b3da26f886815bbfc809f13efd371281e997d564e2c8da18e105` |
| Berke long-format choice file | 36,297 rows; no duplicate rows or missing cells; SHA-256 `97d966585fb4231b2243e90f4ccb9d0589777274e9b8a1665f0ba8312c632390` |
| Wasatch survey archive | 200 respondent rows after two metadata rows; SHA-256 `b14a2b9804db31dbd3301b921885922c59aae33e9efbe9dae8fa924c59ee7cc3` |
| Jafarov survey workbook | SHA-256 `b958666faf83ad3f4e95a5c95871b2e6eab369bcc0e9a241b9445edfe80691e9` |
| NASA archive | Flight metadata workbook plus 62 MATLAB measurement files; original data-description document retained locally |
| Kawai et al. JASA response data | Version DOI `10.5281/zenodo.22045972`; dataset CSV SHA-256 `54876cd04bef71b431de45b6517a19539c56a347bd94754576b41bf3441a7cc3`; legend CSV SHA-256 `6e31b6ed6d33a8d3e198709560e7fa4ac4404582715f30a04d2ff1aedebf5d76` |

Table 2. File checks and provenance records retained for a reproducible calibration workflow.

The local notes identify DroneNoise and Wasatch as CC BY 4.0 and the Berke repository contains an MIT licence. Zenodo publishes the Kawai et al. dataset under CC BY 4.0. These records must be checked against the original distribution pages before any raw file is republished.

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

The Kawai et al. data should be the preferred source if the study introduces an event-level annoyance response. The Wasatch dataset can then supply a broader sensitivity distribution, but the two sensitivity scales must not be treated as interchangeable without a mapping study. Berke should remain outside the core experiment unless customer choice becomes part of the research question.

## Discussion

The available datasets now support two linked layers: an empirically based route-exposure calculation and a laboratory-based short-term response model. The strongest defensible claim remains narrower than full model validation because the response data do not observe residents in an operating delivery network.

The main limitation is transfer. Aircraft type, operating state, weather, background sound, site geometry, and laboratory presentation affect the result. NASA, DroneNoise, and the Kawai et al. data cannot establish a universal neighborhood budget, and they do not validate service demand or charging behavior. The model should carry parameter and response uncertainty into Phase E and keep the policy budgets separate from both the physical exposure calculation and the annoyance response.

For the next mentor review, the project can present the acoustic equation, unit conversions, validation split, proposed review-period account, and a separate event-response model specification. Future work should then obtain a named case area and operational logs before interpreting results as a real deployment pilot.
