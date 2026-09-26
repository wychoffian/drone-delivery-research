# Simulation sandbox protocol

## Adaptive neighborhood exposure regulation for urban drone delivery

Version 0.1, 26 September 2026

This protocol defines how the model will be used before empirical or field testing. It complements the [ODD model specification](odd.html), which describes what the model contains. This document specifies the permitted experiments, comparison rules, evidence standards, and conditions for moving to the next stage.

## Purpose

The simulation sandbox will test whether adaptive neighborhood exposure budgets behave as intended when delivery demand, population distribution, observation delay, and controller responsiveness vary.

The sandbox supports model development and policy learning. It does not establish a legal noise limit, predict commercial performance, or demonstrate health effects. Current exposure values are proxy units until an empirical acoustic method is selected and validated.

## Decision question

The sandbox addresses one primary question:

> Under what combinations of demand, population distribution, observation delay, and controller responsiveness do adaptive neighborhood exposure budgets reduce repeated local drone exposure without producing unacceptable service loss, oscillation, or burden transfer?

The sandbox will not declare a policy successful from exposure reduction alone. A policy must be evaluated across service, exposure, distribution, and stability outcomes.

## Sandbox boundary

Table 1 defines what is included in the first integrated sandbox and what remains outside it.

| Included | Excluded from the initial sandbox |
| --- | --- |
| Synthetic route network with three alternative corridors | Real airspace authorization and flight procedures |
| Individual delivery requests and arrival times | Customer adoption and willingness to pay |
| Drone availability, battery reserve, mission timing, and return flight | Failures, weather, collision avoidance, and emergency diversion |
| Neighborhood measured and reserved exposure accounts | Validated decibel, annoyance, or health-effect calculations |
| Fixed, proportional, and damped budget controllers | Legal enforcement powers and institutional behavior |
| Population distributions and population-weighted burden | Resident complaints, political response, and household behavior |
| Delivery, exposure, distribution, and stability outputs | Economic valuation and full life-cycle environmental effects |

Table 1. Boundary of the first integrated simulation sandbox. Excluded elements may be added only when they affect the research question and have a defensible rule or evidence source.

## Actors and permissions

The sandbox contains the following decision components:

- The operator may queue requests, assign an available drone, select a feasible route, reserve exposure, or defer a request.
- A drone may execute only its assigned outbound, delivery, return, and charging states.
- The regulator may update next-period budgets at a review boundary using the policy rule and available observation.
- Requests and neighborhoods are records. They do not make behavioral decisions.

The operator cannot authorize a route that fails battery, payload, or exposure feasibility. The regulator cannot alter authorization for a mission that has already departed.

## Experimental factors

Table 2 defines the four approved dials. The first two describe the operating scenario. The last two describe the adaptive controller and apply only to adaptive policies.

| Dial | Levels | Interpretation |
| --- | --- | --- |
| Demand | 18, 45, or 80 expected requests per review period | Low, medium, or high baseline demand before a 60 percent increase from period 9 |
| Population distribution | Dispersed or concentrated | Total population remains 900 while its distribution across neighborhoods changes |
| Information delay | 0, 1, or 3 review periods | Age of the exposure observation used to set the next budget |
| Controller responsiveness | Gain 0.15, 0.60, or 1.50 | Magnitude of the budget response to deviation from the exposure target |

Table 2. Approved sandbox factors and levels. Values are experimental settings rather than calibrated policy recommendations.

The population factor does not change buildings, the route network, or destination probabilities. It must therefore be described as population distribution rather than urban form.

## Policy regimes

Table 3 defines the core comparison. All policies receive the same requests for a given seed.

| Regime | Route rule | Budget rule |
| --- | --- | --- |
| P0: unrestricted shortest path | Select the shortest operationally feasible route | No neighborhood exposure budget |
| P1: noise-aware routing | Minimize distance plus population-weighted exposure cost | No neighborhood exposure budget |
| P2: fixed budget | Select the shortest route that fits every affected neighborhood account | Hold the budget fixed at 28, 34, or 40 proxy units |
| P3-U: proportional adaptive budget | Use the P2 feasibility and route rule | Apply the unsmoothed proportional update |
| P3-D: damped adaptive budget | Use the P2 feasibility and route rule | Apply the proportional update with damping factor `lambda = 0.50` |

Table 3. Policies in the first sandbox experiment. The three P2 levels prevent the adaptive policy from being compared with only one arbitrary fixed budget.

An asymmetric controller that tightens faster than it relaxes is reserved for a later sensitivity experiment. It will not be introduced until the symmetric proportional and damped rules pass verification.

## Controller rules

The unsmoothed adaptive rule is:

`B(i,t+1) = clip(B(i,t) - a * (E(i,t-d) - T(i)), Bmin, Bmax)`

The damped rule is:

`B(i,t+1) = clip(B(i,t) - lambda * a * (E(i,t-d) - T(i)), Bmin, Bmax)`

The initial demonstration uses `T = 28`, `Bmin = 6`, `Bmax = 70`, and `lambda = 0.50`. These values remain sandbox assumptions. Updates occur after period `t` closes and apply to period `t+1`.

## Experiment phases

The sandbox will proceed through the phases in Table 4. A later phase cannot begin until the preceding gate passes.

| Phase | Purpose | Runs or cases | Gate |
| --- | --- | --- | --- |
| A: rule verification | Test scheduling, conservation, bounds, and edge cases | Deterministic unit and event-sequence cases | Every required invariant passes |
| B: two-request integration | Connect queueing, drone availability, and active exposure reservations | One controlled event sequence with two overlapping requests | No double assignment, lost reservation, or incorrect exposure debit |
| C: stochastic screening | Compare policies across the approved factor grid | 30 matched seeds for each unique configuration | Results reproduce and uncertainty is reported |
| D: sensitivity analysis | Test fixed budgets, damping, exposure coefficients, and tie-breaking | Targeted extensions around influential settings | Conclusions are not dependent on one arbitrary parameter |
| E: empirical readiness | Replace selected synthetic inputs with evidenced values | Case-specific runs | Data provenance and validation targets are documented |

Table 4. Sandbox phases and progression gates. Phase C is not evidence of empirical validity because it still uses synthetic inputs.

## Core scenario matrix

The first stochastic screening uses six scenario combinations from three demand levels and two population patterns.

P0 and P1 each contain 6 unique configurations. P2 contains 18 because each scenario is tested with fixed budgets of 28, 34, and 40. P3-U contains 54 because each scenario is crossed with three delays and three gains. P3-D contains the same 54 configurations with damping fixed at 0.50.

The screening therefore contains 138 unique configurations per seed. With 30 matched seeds, the initial design contains 4,140 runs.

Thirty seeds are a screening minimum, not a universal sample-size rule. The analysis will report Monte Carlo uncertainty. Additional seeds will be added when key policy differences or tail outcomes remain unstable.

## Common random numbers

For each demand and population scenario, every policy receives the same request arrivals, destinations, payloads, and other stochastic inputs for a given seed. Controller behavior must not change the random-number stream used to generate demand.

The run record must include:

- model and protocol version;
- policy and parameter values;
- random seed and demand-stream identifier;
- network and input-data versions;
- execution timestamp;
- software environment identifier.

## Dispatch and reservation rules

Requests enter a first-in, first-out queue, with request identifier breaking timestamp ties. A later experiment may test priority or deadline rules, but the core comparison uses one queue discipline.

Before departure, a mission must pass the following checks:

1. A drone is available.
2. Payload and complete return-trip battery use are feasible.
3. The required battery reserve remains after return.
4. Under P2 or P3, measured exposure plus existing reservations plus the proposed complete mission contribution remains within every affected neighborhood budget.

An accepted mission immediately reserves its complete outbound and return exposure. Each neighborhood crossing transfers the relevant contribution from reserved to measured exposure. A request deferred because no drone is available must be distinguished from one blocked by exposure capacity.

## Required verification cases

Table 5 defines the checks required before stochastic results are interpreted.

| Case | Expected result |
| --- | --- |
| Zero demand | No mission, exposure, reservation, or queue growth |
| Same seed across policies | Identical request sequence |
| Zero responsiveness | Adaptive rule reproduces the corresponding fixed-budget behavior |
| No available drone | New request remains queued and the active drone is not assigned twice |
| Active reservation | A second mission cannot consume exposure already reserved by the first |
| Complete mission | Outbound and return exposure are both measured and its reservation returns to zero |
| Battery boundary | Mission is rejected when projected return battery falls below reserve |
| Exposure boundary | Mission at the exact budget is accepted; a mission above it is rejected |
| Delay timing | Update uses period `t-d` and affects only period `t+1` |
| Controller bounds | Every budget remains between `Bmin` and `Bmax` |
| Reproduction | Identical version, inputs, and seed produce identical outputs |

Table 5. Required verification cases for the integrated sandbox.

## Outcomes and comparison rules

Table 6 defines the outcome families used in every policy comparison.

| Family | Primary measures |
| --- | --- |
| Service | Delivery rate, rejected requests, deferred requests, mean delay, 95th-percentile delay, queue length |
| Operations | Route shares, distance, energy use, utilization, charging time |
| Exposure | Neighborhood exposure by period, maximum exposure, exceedances, cumulative exposure |
| Distribution | Population-weighted burden, neighborhood dispersion, group burden when data permit |
| Regulation | Budget path, adjustment size, time at bounds, observation used |
| Stability | Budget oscillation, route switching, exposure variance, persistent queue growth |

Table 6. Required sandbox outcomes. Results must retain the full neighborhood distribution rather than reporting only a network average.

Policy comparisons follow four rules:

1. Report exposure and delivery service together.
2. Compare adaptive policies with several fixed budgets, including comparisons at similar service levels.
3. Report uncertainty across matched seeds and do not rank policies from one run.
4. Examine whether reduced exposure in one neighborhood is transferred to another population.

## Failure conditions

A run is invalid if it contains a budget violation by an accepted mission, a negative reservation, a drone assigned to overlapping missions, a battery reserve violation, an incorrect delayed observation, or missing run metadata.

The controller is considered behaviorally problematic under a scenario when budgets repeatedly alternate direction with increasing or persistent amplitude, remain saturated while unmet demand grows, or cause frequent corridor switching without a material exposure benefit. Exact diagnostic thresholds will be set after Phase C distributions are inspected and documented before policy ranking.

## Evidence levels

Sandbox findings will use the following labels:

- Verification finding: the implementation follows a specified rule.
- Synthetic experiment finding: behavior observed under assumed inputs.
- Sensitivity finding: behavior that changes or persists across parameter ranges.
- Empirically anchored finding: behavior produced after the relevant inputs and validation targets are supported by data.

The current project may report the first three levels. It must not describe a synthetic experiment as an observed real-world effect.

## Progression to an empirical or field sandbox

The simulation may progress to an empirically anchored case when:

- the integrated queue, mission, reservation, exposure, and regulator processes pass verification;
- the experiment is reproducible from recorded versions and seeds;
- demand, vehicle performance, source noise, and population inputs have documented provenance;
- the exposure metric and review period are defined in compatible physical units;
- uncertainty and sensitivity results identify which assumptions matter;
- the relevant regulator or decision owner is identified.

A field sandbox requires a separate protocol covering aviation authorization, safety management, privacy, ethics, community engagement, acoustic instrumentation, data governance, incident response, and stopping rules. Approval of this simulation protocol does not authorize real flights.

## Phase B implementation result

Phase B is implemented as a controlled two-request event sequence. R2 arrives at minute 2 while D1 is outbound on R1. It remains in the first-in, first-out queue until D1 returns at minute 9, then receives new battery and exposure feasibility checks before dispatch. The sequence contains no double assignment, lost reservation, negative exposure debit, or battery reserve violation.

The small-fleet extension is now implemented with two drones, three requests, simultaneous reservations, and charging. R3 remains queued while D1 charges and D2 returns, then dispatches after a new feasibility check. Assignment uniqueness, battery reserve, reservation conservation, and charging transitions pass the controlled checks.

The event-based fleet is now connected to the multi-period policy controller. The integrated model reproduces the operational conservation rules and the policy timing checks across 360 factor-policy verification cases.

## Phase C implementation result

Phase C executed the approved matrix of 138 unique configurations across 30 matched seeds, for 4,140 runs. The run-level data, 138 configuration summaries, execution metadata, and 95 percent Monte Carlo intervals are published on the [Phase C results page](phase-c.html). The findings retain the synthetic experiment label and do not establish a preferred policy.

The next task is Phase D sensitivity analysis. It will test fixed-budget spacing, damping, exposure coefficients, destination probabilities, fleet size, charging capacity, and route tie-breaking before any policy ranking.
