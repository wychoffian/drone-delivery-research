# Two drones, three requests, and charging

This executable walkthrough extends the operational model to a small fleet. It connects first-in, first-out queueing with deterministic drone selection, simultaneous exposure reservations, mission battery use, and charging. It remains a controlled browser prototype with assumed inputs.

R1 arrives at minute 0 and is assigned to D1. R2 arrives at minute 2 while D1 is active and is assigned to D2. R3 arrives at minute 10 while D1 is charging and D2 is returning, so it waits until D1 completes charging.

Figure 1 presents the resulting event sequence.

Figure 1. Event sequence for three requests and two drones in the synthetic nine-neighborhood network. Orange identifies D1, blue identifies D2, circles show measured neighborhood exposure, and the request labels show queue and service states.

## Model assumptions

All requests have destination Z2. D1 and D2 start at the depot with 100 percent battery. The operator processes the queue in first-in, first-out order. Available drones are ranked by identifier, so D1 is selected before D2 when both are equally feasible.

A complete mission must retain at least 20 percent battery after return. Flight consumes 2 battery percentage points per distance unit. Charging restores 4 percentage points per minute until the battery reaches 100 percent. These values make the state transitions inspectable and are not vehicle specifications.

The central route has a return distance of 12 units. The upper and lower routes each have a return distance of 16 units. All neighborhoods have a current budget of 40 proxy exposure units. Starting exposure for N1 through N9 is [27, 26, 25, 24, 39.2, 24, 22, 23, 24].

Complete return-trip increments are [1.1, 0.8, 1.0, 1.0, 1.4, 1.1, 0.9, 1.0, 0.8] along the chosen corridor. Dispatch feasibility uses measured exposure plus all active reservations plus the proposed mission contribution. Each crossing moves half of the relevant mission contribution from reserved to measured exposure.

The review boundary is minute 35, after all missions and charging events in this controlled sequence.

## Event sequence

At minute 0, D1 receives R1. The central route fails because N5 would reach 40.6 units, so the operator selects the upper route and reserves its complete exposure contribution.

At minute 2, D1 is outbound and D2 is available. The operator checks R2 against the exposure already measured by R1 and R1's remaining reservation. The upper route still fits, so D2 receives R2. Both missions then hold reservations in the same neighborhood accounts.

D1 completes R1 at minute 9 with 68 percent battery and charges until minute 17. D2 completes R2 at minute 11 and charges until minute 19.

R3 arrives at minute 10. D1 is charging and D2 is returning, so R3 remains queued. When D1 finishes charging at minute 17, the operator checks the request again and dispatches it. The recorded wait is seven minutes. D1 completes R3 at minute 26 and finishes charging at minute 34.

The three missions add 3.3 units to N1, 2.4 to N2, and 3.0 to N3. Their final exposures are 30.3, 28.4, and 28.0 units. All active reservations return to zero. Both drones are available with full batteries when the regulator closes the period at minute 35.

## Verification result

The controlled fleet sequence passes the following checks:

- each request is assigned to at most one drone;
- each drone executes at most one mission at a time;
- R1 and R2 hold simultaneous reservations without exceeding a neighborhood budget;
- R3 remains queued while both drones are unavailable;
- D1 becomes dispatchable only after charging completes;
- queue order and the seven-minute wait are recorded;
- crossing events never make a reservation negative;
- outbound and return exposure are both measured;
- both batteries remain above the return reserve;
- final measured exposure plus active reservations stays within every budget.

These are verification findings. They do not establish fleet performance or policy effectiveness.

## Discussion

The small fleet creates two mechanisms absent from the allocation-only policy prototype. Missions can reserve exposure concurrently, and charging can delay service even when a route remains feasible. Individual drone state therefore affects when requests are served in this controlled case.

The walkthrough still uses one destination, identical drones, one charging rule, no charger-capacity limit, and no delivery deadline. Demand is scheduled rather than stochastic. The next development step should connect this event model to the 24-period policy controller. That integration will allow matched demand sequences to test whether fleet state changes policy comparisons before Phase C stochastic screening begins.
