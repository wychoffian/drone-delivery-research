# ODD model specification

## Adaptive neighborhood exposure regulation for urban drone delivery

Version 0.1, 22 September 2026

This model description follows the Overview, Design concepts, and Details protocol described by [Grimm et al. (2020)](https://doi.org/10.18564/jasss.4259). It specifies the target integrated research model. The current browser prototypes implement parts of this specification but do not yet constitute the complete agent-based model.

## Model status

Table 1 distinguishes executable behavior from planned model components.

| Component | Status | Evidence |
| --- | --- | --- |
| Demand generation and route allocation | Implemented | Four-policy browser prototype with 24 review periods |
| Fixed and proportional exposure budgets | Implemented | Policy prototype and 384 deterministic checks across factor combinations |
| One drone mission, battery, timing, and return exposure | Implemented separately | Twelve-event mission walkthrough |
| Integrated request queue and drone availability | Planned | First target for model version 0.3 |
| Multiple drones, charging, and fleet interaction | Planned | Added after the two-request availability test |
| Damped adaptive controller | Specified, not implemented | Added following the regulation and control literature review |
| Empirical acoustic exposure | Planned | Current exposure values are proxy units |
| Socioeconomic equity attributes | Planned | Current population patterns are synthetic |

Table 1. Implementation status of the ODD components. "Implemented separately" means that the behavior runs in a prototype but has not been integrated with the multi-period policy model.

# Overview

## Purpose and patterns

The model will examine when adaptive neighborhood exposure budgets can control repeated local exposure from urban drone deliveries while maintaining delivery service. It is designed to answer the following question:

> Under what combinations of demand, population distribution, observation delay, and controller responsiveness do adaptive neighborhood exposure budgets reduce repeated local drone exposure without producing unacceptable service loss, oscillation, or burden transfer?

The model is explanatory and comparative. It is not intended to forecast commercial drone activity or recommend a legal exposure threshold in its current form.

The target patterns are outcomes that the model should be able to generate and explain rather than results that are assumed in advance:

- concentration of flights and exposure on short routes under an unrestricted policy;
- rerouting or rejected requests when local budgets become binding;
- transfer of exposure from constrained neighborhoods to alternatives;
- delayed or oscillating budget responses under high gain or long observation delay;
- queue growth or service loss when exposure and fleet constraints interact;
- differences between physical exposure, population-weighted burden, and distributional equity.

The model will compare four policy regimes. P0 uses shortest feasible paths without exposure regulation. P1 adds a population-weighted exposure term to route cost. P2 enforces fixed neighborhood exposure budgets. P3 updates those budgets using delayed exposure observations. A damped P3 variant will be included to test whether smoothing reduces oscillation.

## Entities, state variables, and scales

The model contains agents, passive entities, an environment, and an institutional controller. Table 2 defines their minimum state.

| Entity | Type | State variables | Main action |
| --- | --- | --- | --- |
| Delivery operator | Decision-making agent | Pending requests, available drones, candidate routes, route costs, current neighborhood budgets, exposure reservations | Select a request, drone, and feasible route or defer the request |
| Drone | Individual agent | Identifier, location, battery, payload state, mission state, assigned request, route, route position, availability time | Execute outbound, delivery, return, and charging events |
| Regulator | Institutional controller | Policy regime, neighborhood budgets, targets, observation history, delay, gain, damping, bounds, review clock | Calculate budgets for the next review period |
| Request | Passive task entity | Arrival time, origin, destination, payload, deadline, status, assigned drone, completion time | Enter a queue and record service outcomes |
| Neighborhood | Spatial record | Geometry or network association, population, baseline sound, socioeconomic attributes, measured exposure, reserved exposure, current budget | Accumulate exposure and provide attributes for routing and evaluation |
| Route network | Environment | Nodes, edges, distances, permitted directions, corridor labels, neighborhood intersections | Supply candidate paths and movement distances |

Table 2. Entities, state variables, and actions in the target model. Residents are represented through neighborhood attributes unless behavioral responses are added later.

The synthetic environment contains one depot, three delivery zones, three alternative corridors, and nine exposed neighborhoods. Each corridor crosses three neighborhoods. The research version may replace this network with an empirical spatial graph, but the synthetic network remains the verification case.

The model uses two time scales. Operational time advances through request arrivals, drone movement, delivery, return, and charging events. Regulatory time advances at review boundaries. Operational time is represented in minutes. The duration of a review period, `H_review`, remains a calibration parameter. The ten-minute boundary in the single-mission walkthrough is an illustrative test value, not a proposed policy interval.

The current synthetic network uses abstract distance units. The mission walkthrough uses flight speed of two distance units per minute, battery consumption of two percentage points per distance unit, and a 20 percent battery reserve. These values are verification assumptions that will be replaced or bounded using evidence.

## Process overview and scheduling

Figure 1 gives the target schedule. Processes at the same timestamp are executed in the listed order to prevent inconsistent reservations or updates.

```text
Initialize scenario, policy, network, neighborhoods, and drones
For each operational event time:
  1. complete scheduled drone movements and record received exposure
  2. complete deliveries, returns, and charging transitions
  3. release or reconcile exposure reservations
  4. add newly arrived requests to the queue
  5. dispatch queued requests in the documented queue order
  6. reserve complete outbound and return exposure for accepted missions
At each regulatory review boundary:
  7. close the exposure account for the completed period
  8. record service, exposure, burden, and stability outputs
  9. read the exposure observation specified by the information delay
  10. calculate budgets for the next period
  11. reset current-period measured exposure while retaining history
Stop after the specified number of review periods
```

Figure 1. Process order for the integrated model. Dispatch occurs after state completion at the same timestamp so a returned drone can become available before the next queued request is considered.

Requests arriving at the same timestamp are ordered by a reproducible rule, initially request identifier. Candidate routes are ordered by policy cost and then corridor identifier. These tie-breaking rules are recorded because they can affect exposure distribution.

# Design concepts

## Basic principles

The model combines four ideas. Delivery requests create demand for a limited fleet. Route decisions create spatial externalities. Neighborhood budgets impose local quantity constraints. A regulator changes future budgets using delayed information. The combined system can produce feedback, displacement, saturation, and service trade-offs that are difficult to infer from a static route optimization.

Conventional aviation noise quotas show that operations can consume noise-weighted allowances. Transport-control studies show that feedback signals can change route choices and network stability. The model tests a different mechanism: received-exposure accounts for multiple neighborhoods along a drone route, revised at discrete review boundaries.

## Emergence

Route concentration, neighborhood burden, unmet demand, budget oscillation, and queue length emerge from individual request and mission events. They are not assigned as model targets. Spatial equity emerges from the interaction of population attributes, network structure, route choices, and budget constraints.

## Adaptation

The operator adapts route and dispatch decisions to current fleet availability and remaining neighborhood budgets. The P3 regulator adapts future budgets to observed exposure. Drones do not learn; they follow mission rules. Requests and neighborhoods have no behavioral adaptation.

## Objectives

The operator minimizes a policy-specific route cost subject to battery, payload, fleet, and exposure feasibility. P0, P2, and P3 initially rank feasible routes by distance. P1 ranks routes by distance plus a population-weighted exposure term.

The regulator seeks to keep exposure near a target without directly optimizing a global welfare function. Its proportional rule is a bounded controller, not an optimal policy. Evaluation therefore compares its outcomes with fixed-budget baselines and, if feasible, an optimized benchmark.

## Learning

No behavioral learning is included in the bounded model. Budget updating is adaptation through a specified controller rather than machine learning or belief revision. Later work should not introduce operator or resident learning without a behavioral theory and supporting evidence.

## Prediction

The operator evaluates the full outbound and return mission before departure. It predicts distance, battery use, and exposure debit using known route coefficients. It does not predict future requests. The regulator uses recorded exposure from period `t-d` and does not forecast future demand in the initial model.

## Sensing

The operator observes queued requests, drone state, route alternatives, current budgets, measured exposure, and active reservations. The regulator observes period exposure after the stated delay. Perfect sensing is assumed initially. Measurement error and missing observations are later uncertainty scenarios.

## Interaction

Drones interact indirectly by competing for operator attention, charging capacity, and exposure capacity. Missions interact through shared neighborhood accounts. The regulator influences later operator choices through budgets. Population affects P1 route costs and evaluation, but residents do not directly interact with drones.

## Stochasticity

Request counts are currently generated by a Poisson process, and destinations are sampled from documented probabilities. All policies receive matched request sequences for a given seed. Route selection and controller updates are otherwise deterministic. Empirical work may replace the Poisson assumption if arrivals are overdispersed, time-varying, or spatially correlated.

## Collectives

The drone fleet is a collection of individual drone agents managed by one operator. Neighborhoods may later be grouped by deprivation or baseline environmental burden for analysis. Such groups are analytical categories and do not act as collective agents.

## Observation

The model records operational, environmental, distributional, and stability outcomes. Table 3 defines the minimum output set.

| Outcome family | Measures |
| --- | --- |
| Service | Requests received, delivered, deferred, rejected, mean and percentile delay, queue length |
| Operations | Flights, distance, energy use, drone utilization, charging time, route shares |
| Exposure | Period exposure by neighborhood, maximum exposure, exceedance count, cumulative received exposure |
| Distribution | Population-weighted burden, between-neighborhood dispersion, group burden where data permit |
| Regulation | Budget by neighborhood and period, time at bounds, adjustment magnitude, delayed observation used |
| Stability | Budget oscillation, route switching frequency, exposure variance, persistent queue growth |

Table 3. Minimum outputs for evaluating service, exposure, distribution, and feedback behavior. No single indicator is sufficient to rank policies.

# Details

## Initialization

Each run records the model version, policy, random seed, network version, parameters, and input-data versions. The synthetic verification case uses seed 421, 24 review periods, and a 60 percent demand increase beginning in period 9.

The current policy prototype initializes neighborhood budgets at 40 proxy units, target exposure at 28, lower and upper budget bounds at 6 and 70, and gains of 0.15, 0.60, or 1.50. Information delay is 0, 1, or 3 review periods. These values are experimental settings rather than measured regulatory values.

The dispersed population pattern assigns 100 residents to each of nine neighborhoods. The concentrated pattern assigns 220 residents to each central-corridor neighborhood and 40 residents to each remaining neighborhood, keeping the total at 900.

Every drone begins at the depot with a documented battery state and is available unless the scenario specifies an active mission. Requests begin empty and arrive according to the demand submodel. Exposure and reservations begin at zero for a standard run. Verification cases may initialize exposure near a budget to test route rejection.

## Input data

Table 4 separates current synthetic inputs from evidence needed for an empirical case.

| Input | Current source | Required empirical replacement or justification |
| --- | --- | --- |
| Network and neighborhoods | Synthetic three-corridor graph | Permitted air routes, buildings, sensitive locations, and neighborhood boundaries |
| Demand | Poisson counts and fixed destination probabilities | Time-stamped orders or a calibrated demand model |
| Drone performance | Illustrative speed, battery rate, and reserve | Vehicle specifications and operational tests |
| Source noise | Route-specific proxy coefficients | Measurements following a documented level-flight and hover procedure |
| Propagation | Embedded in proxy coefficients | Acoustic propagation appropriate to altitude, buildings, ground, and weather |
| Population | Two synthetic patterns | Population grid and selected socioeconomic variables |
| Baseline sound | Omitted | Observed or modelled ambient sound by location and time |
| Regulatory target | Illustrative value of 28 | Explicit policy rationale and compatible exposure units |

Table 4. Input-data status. Proxy exposure cannot be interpreted as decibels, annoyance, or health risk.

## Submodels

### Demand generation

For review period `t`, the prototype draws request count `N_t` from a Poisson distribution with mean `mu_t`. Before period 9, `mu_t` is 18, 45, or 80. From period 9, it increases by 60 percent. Destination probabilities are 0.20, 0.60, and 0.20 for zones Z1 to Z3.

The integrated model will assign operational arrival times within each period. A homogeneous Poisson process is the initial assumption. The implementation must permit replacement with empirical or time-varying arrivals.

### Candidate routes and cost

For corridor `r` and destination `z`, the synthetic return-trip distance is:

`L(r,z) = 2 * (6 + abs(r - 1) + abs(r - z))`

P0, P2, and P3 rank candidates by `L`. P1 uses:

`C(r,z) = L(r,z) + w * sum(population(i) * c(i) for i on r) / 100`

where `w = 0.75` in the demonstration and `c(i)` is the route contribution for neighborhood `i`. The scaling and weight require sensitivity analysis.

### Dispatch and feasibility

A request is dispatchable when at least one drone is available and at least one route passes payload, battery, and policy checks. Required battery includes the complete outbound and return distance plus reserve. Under P2 and P3, a route is exposure-feasible only when:

`measured(i,t) + reserved(i,t) + mission_contribution(i) <= budget(i,t)`

for every affected neighborhood `i`.

Once assigned, the complete mission contribution is added to reserved exposure. If no route is feasible, the request is deferred until its deadline or recorded as rejected according to the scenario rule. The model must distinguish fleet deferral from exposure rejection.

### Mission execution

A drone transitions through `available`, `outbound`, `delivering`, `returning`, `charging`, and back to `available`. Movement consumes battery and advances route position. Delivery changes the request status, but the drone remains occupied until it returns. Recharging time is included only after empirical or bounded assumptions are defined.

When the drone crosses an exposed neighborhood, that portion of the mission contribution moves from reserved to measured exposure. At mission completion, its reservation must equal zero. A failed or diverted mission requires an explicit reservation-reconciliation rule before such failures are introduced.

### Exposure accounting

The synthetic complete return-trip contributions for N1 to N9 are:

`c = [1.1, 0.8, 1.0, 1.0, 1.4, 1.1, 0.9, 1.0, 0.8]`

Only neighborhoods on the selected corridor receive contributions. The mission walkthrough assigns half at the outbound crossing and half at the return crossing. These values are additive proxy units. Decibel values must not be added directly when the acoustic model replaces the proxy.

### Regulatory update

P2 keeps every budget fixed. The unsmoothed P3 rule is:

`B(i,t+1) = clip(B(i,t) - a * (E(i,t-d) - T(i)), Bmin, Bmax)`

The damped variant is:

`B(i,t+1) = clip(B(i,t) - lambda * a * (E(i,t-d) - T(i)), Bmin, Bmax)`

where `a` is controller gain, `d` is observation delay, and `lambda` is a smoothing factor between zero and one. Updates occur after period `t` closes and affect period `t+1`. Delay zero therefore means that exposure from the completed current period informs the next budget. It never changes authorization for a completed mission.

The symmetric rule relaxes budgets when exposure is below target. An asymmetric variant will test slower relaxation than tightening. Saturation at the lower or upper bound is recorded as an output.

### Data collection

Event-level logs record request, dispatch, drone transition, neighborhood crossing, reservation, exposure, and regulatory-update events. Period summaries are derived from these logs rather than maintained independently. Every output row includes run identifier, seed, policy, scenario, and time.

# Evaluation and fitness for purpose

The model is fit for its initial purpose if another researcher can reproduce the synthetic runs and if the implementation passes the checks in Table 5.

| Check | Expected result |
| --- | --- |
| Zero demand | No missions or added exposure |
| Same seed across policies | Identical request sequence |
| Zero responsiveness | Adaptive rule reproduces the corresponding fixed budget |
| Budget feasibility | No accepted mission exceeds a current budget after including reservations |
| Return accounting | Every accepted mission reserves and records both flight legs |
| Battery feasibility | No mission returns below the specified reserve |
| Delay timing | The recorded historical period matches `t-d` and updates only `t+1` |
| Reservation conservation | Reserved exposure returns to zero when all missions finish |
| Controller bounds | Every budget remains within `Bmin` and `Bmax` |
| Replication | Same version, inputs, and seed produce identical outputs |

Table 5. Verification checks required before model outputs are interpreted.

Empirical validation is separate. It requires evidence for arrival patterns, mission performance, source noise, propagation, baseline sound, population exposure, and regulatory interpretation. Sensitivity analysis cannot substitute for those data.

## Model rationale and alternatives

An agent-based model is justified only if individual drone availability, queues, reservations, and asynchronous missions change policy outcomes. If the integrated implementation produces the same conclusions as the allocation-only model, a discrete-event or optimization model may be sufficient and easier to validate.

The operator and regulator are retained as explicit decision components for traceability. The regulator is an institutional controller rather than a human-like agent. Neighborhoods and requests remain passive records. Resident agents should be added only if behavior such as complaints, adoption, or political response becomes part of the research question.

## Limitations, implications, and next development

The synthetic graph predetermines a small set of exposure-transfer options. Proxy units omit physical acoustics, ambient sound, and perception. The controller target and bounds are experimental. Legal authority for neighborhood budgets has not been established. These limitations prevent policy recommendations from the current prototype.

The specification makes the next implementation test precise: introduce a second request while the first drone remains on mission. The second request must encounter drone unavailability and active exposure reservations. Passing that test will connect operational time with the regulatory model before the fleet is expanded.

The result will determine whether individual agents add explanatory value. Later versions can then add multiple drones, charging, empirical exposure, socioeconomic attributes, and replicated policy experiments.

[Read the simulation sandbox protocol](sandbox.html)
