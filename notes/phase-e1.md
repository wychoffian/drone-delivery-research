# Phase E1 route acoustic integration

Phase E1 applies the calibrated acoustic module to a moving outbound and return mission. It gives the nine neighborhood agents physical receiver coordinates and accumulates exposure from every route segment in the linear energy domain.

> Evidence label: Calibrated module in synthetic geometry. Acoustic parameters are evidence based, while the network coordinates, altitude, speed, and route layout remain declared scenarios.

[Run the route acoustic module](route-acoustic.html)

## Synthetic physical geometry

The geometry retains the three-corridor structure used in the policy model. Coordinates are measured in metres. The depot is at `(0, 0)`, the route entry is 500 m east of the depot, the main corridor extends to 5,500 m, and the three destinations are at 6,000 m. Corridor and destination northings are -400, 0, and 400 m.

Nine neighborhood receivers form a three-by-three grid. Their eastings are 1,500, 3,000, and 4,500 m, and their northings match the three corridor rows. Every mission contributes exposure to all nine receivers. A selected route therefore affects nearby corridors as well as the neighborhoods directly below it.

Table 1 records the current physical scenario values.

| Quantity | Current value | Status |
| --- | ---: | --- |
| Corridor length between entry and exit | 5,000 m | Synthetic geometry |
| Corridor spacing | 400 m | Synthetic geometry |
| Receiver spacing along corridor | 1,500 m | Synthetic geometry |
| Default altitude | 80 m | Adjustable scenario |
| Default speed | 15 m/s | Adjustable scenario |
| Maximum integration time step | 1 s | Numerical setting |
| Source reference level | 88.4 dB(A) at 1 m | DroneNoise calibrated |
| Off-axis directivity | `-3.3114(sec(theta) - 1)` dB | DroneNoise calibrated |

Table 1. Geometry, operation, numerical settings, and calibrated inputs used by the Phase E1 route module.

## Moving-source calculation

Each straight route segment is divided into time steps no longer than one second. The calculation evaluates the drone at the midpoint of each step. For every receiver, it calculates horizontal separation, slant distance, off-axis angle, and A-weighted received level. The step contributes:

```text
energy contribution = 10^(LpA / 10) x duration
```

The mission account sums all step contributions from the outbound and return paths. Reporting converts the final energy to LAE:

```text
mission LAE = 10 log10(sum of energy contributions)
```

The integration uses shorter final steps where a segment duration is not an exact multiple of one second. This prevents the numerical routine from adding excess flight time.

## Agent mapping

Table 2 shows how this module changes the agent-based model.

| Agent | Phase D representation | Phase E1 representation |
| --- | --- | --- |
| Drone | Route identifier and proxy exposure coefficient | Position along a physical path, altitude, speed, and calibrated source level |
| Neighborhood | One proxy account associated with one corridor | Fixed receiver coordinate with energy received from every mission |
| Operator | Reserves three fixed coefficients | Predicts and reserves a nine-receiver mission exposure vector |
| Regulator | Compares proxy account with proxy budget | Receives a physical exposure vector, while the budget basis remains separate |

Table 2. Change from proxy route accounting to moving-source acoustic accounting.

## Verification

Automated checks cover the following properties:

- each mission returns nine finite positive exposure values;
- the reported route distance and flight time match the physical path and speed;
- outbound and reversed return paths contribute equal energy;
- a round trip adds 3.0103 dB to the identical outbound event;
- increasing altitude reduces the peak receiver exposure in the reference case;
- reducing speed increases exposure because the source remains audible longer;
- the central route produces symmetric exposure in the north and south receiver rows;
- all previous mission, policy, and Phase E0 checks continue to pass.

## Progression gate

Phase E1 establishes a traceable route exposure vector. [Phase E2](phase-e2.html) now connects that vector to the fleet-policy controller using a declared review window and reference-mission budget scenarios. The controller compares linear exposure energy with linear energy budgets. LAE is used for reporting because subtracting or adding dB values inside the reservation ledger would violate energy conservation.

## Discussion

The moving-source calculation is a material change to the system map. Neighborhood exposure is no longer assigned only to the selected corridor. Every flight affects all receivers, with the contribution determined by geometry and directivity. This creates spatial spillovers that may alter route ranking and distributional outcomes.

The current coordinates are synthetic and the source level does not change between cruise, turns, takeoff, or landing. Buildings, ground effects, weather, background sound, and shielding are omitted. These limits mean the module supports mechanism testing rather than a site prediction.

The nine-receiver vector is now connected to mission reservation and completion events in Phase E2. The next experiment should test acoustic and policy uncertainty across matched demand seeds. A later case study should replace the synthetic coordinates with a named area's route constraints and population data.

## Reproducibility file

The [route geometry record](results/route_acoustic_geometry_v1.json) documents the coordinates, default operating values, integration setting, and remaining exclusions.
