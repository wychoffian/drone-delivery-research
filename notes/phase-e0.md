# Phase E0 acoustic module

Phase E0 replaces the arbitrary exposure scale with a physical single-event acoustic calculation. It keeps the current policy budgets in proxy units until the project defines their regulatory or community basis.

> Evidence label: Calibrated module prototype. The source and directivity parameters are fitted to DroneNoise measurements. NASA flyovers are held out for validation. The route-level integration remains a development step.

[Open the acoustic module](acoustic.html)

## Model boundary

The module predicts an A-weighted received level for a payload-capable multirotor reference case. It uses slant distance and the angle measured from the downward vertical:

```text
LpA(r, theta) = 88.4
                 - 20 log10(r / 1 m)
                 - 0.0015(r - 1 m)
                 - 3.3114(sec(theta) - 1)
```

The 88.4 dB(A) value is a received level at 1 m, not a sound-power level. The directivity correction is zero directly below the vehicle and is capped at the maximum validated off-axis angle of 82.2 degrees.

The interactive module converts a steady equivalent level over a selected duration into single-event A-weighted sound exposure level:

```text
LAE = LpA + 10 log10(duration / 1 second)
```

Multiple event exposures are summed in the linear energy domain. The result may then be reported as a cumulative LAE. Decibel values are never added directly.

## Calibration and validation

Table 1 separates fitted quantities from held-out evidence.

| Quantity | Value | Evidence role |
| --- | ---: | --- |
| Reference received level at 1 m | 88.4 dB(A) | Calibrated from DroneNoise Matrice 300 levels |
| Reference-level standard deviation | 1.0 dB | Event source variation scenario |
| Atmospheric absorption | 0.0015 dB/m | Declared propagation parameter |
| Directivity coefficient | 3.3114 dB | Fitted to five DroneNoise flyovers across nine microphones |
| Directivity fit RMSE | 0.66 dB | Calibration fit description |
| NASA validation observations | 56 from 14 flights and four microphones | Held-out external check |
| NASA validation bias | -0.12 dB | Held-out performance |
| NASA validation MAE | 1.77 dB | Held-out performance |
| NASA validation RMSE | 2.29 dB | Held-out performance |

Table 1. Acoustic parameters and held-out validation results used by the Phase E0 module.

The calibration target is the DJI Matrice 300. The held-out NASA aircraft is a Prioria Hex Flyer operating at 15.9 lb with its test payload. The validation supports comparative screening under the measured flyover conditions. It does not establish transfer to every drone, maneuver, weather condition, or urban sound environment.

## Integration contract

Table 2 defines how the acoustic module will connect to the agent-based model without mixing physical measurements and policy choices.

| Model component | Acoustic input | Returned value | Use |
| --- | --- | --- | --- |
| Drone agent | Position, altitude, operating state, time step | Reference source level or sampled event level | Defines the source event |
| Route segment | Drone and receiver geometry, segment duration | Received level and linear sound-exposure contribution | Builds a mission exposure trace |
| Neighborhood agent | Receiver location and accumulated segment contributions | Review-period exposure energy and reported LAE | Records physical exposure |
| Operator agent | Candidate routes and predicted exposure contributions | Feasible route exposure increment | Supports route comparison and reservations |
| Regulator agent | Observed neighborhood exposure account | Physical observation only | Updates a separately specified budget rule |

Table 2. Data passed between the acoustic calculation and the current model agents.

The operational model must reserve the complete predicted outbound and return exposure before dispatch. At mission completion, it will transfer the reserved energy to the observed account. This preserves the existing accounting rule while changing the unit from a proxy coefficient to physical sound exposure.

## Verification

Automated checks confirm that the implementation:

- reproduces the 10 m calibration target within 0.1 dB;
- decreases received level as distance increases;
- applies zero directivity correction on the centerline;
- caps directivity outside the validated angle range;
- converts duration to event exposure correctly;
- adds two equal events by 3.0103 dB;
- retains the existing fleet and policy invariants.

These checks establish numerical consistency. They do not repeat the raw-signal calibration or the NASA validation analysis.

## Discussion

The module gives the project a physical exposure unit and a traceable boundary between acoustics and governance. It also exposes a design problem that the proxy model concealed: a route is a moving source, so its exposure must be integrated over time rather than represented by one constant level.

The current demonstration assumes a steady equivalent level for a chosen duration. The next implementation step is to define physical coordinates for the synthetic network, discretize each outbound and return route, calculate segment exposure at every neighborhood receiver, and sum the contributions in the energy domain. Uncertainty in source level and validation error should be carried into a later sensitivity run.

The Kawai et al. annoyance data can be linked after this route integration. It should receive event exposure and operational descriptors as inputs. Policy budgets remain scenario values until their legal or participatory basis is documented.

## Reproducibility file

The compact [calibration and validation record](results/acoustic_calibration_v1.json) contains the implemented parameters, held-out metrics, scope, and quantities that remain uncalibrated. Raw acoustic measurements remain in the research data folder and are not republished by this website.
