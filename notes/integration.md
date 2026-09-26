# Integrated fleet and policy model

The integrated prototype connects request arrivals, two-drone operations, charging, queues, neighborhood exposure accounts, and the five policy regimes over 24 review periods. It uses the same four experimental dials as the allocation-only prototype.

[Run the integrated prototype](fleet-policy.html)

## Integration boundary

Each review period contains 480 operational minutes. Requests receive reproducible arrival times within each period. Demand increases by 60 percent from period 9. The two drones begin with full batteries and use the same flight, reserve, and charging rules as the controlled fleet walkthrough.

Dispatch stops during the final 10 minutes of a period. The longest candidate mission takes 10 minutes, so every accepted mission completes before the review boundary. Requests and charging states may carry into the next period. This rule prevents one mission's exposure from being split across two regulatory accounts.

Table 1 defines the event order used at equal timestamps.

| Order | Event |
| --- | --- |
| 1 | Complete missions and transfer reservations to measured exposure |
| 2 | Complete charging and make drones available |
| 3 | Close the review period and calculate the next budgets |
| 4 | Add request arrivals |
| 5 | Dispatch queued requests |

Table 1. Event priority in the integrated prototype. Period closure occurs after mission completion at the same timestamp and before new-period dispatch.

## Fleet rules

The operator processes requests in first-in, first-out order. It selects the available drone with the lowest identifier and then selects the lowest-cost feasible route. A request remains queued when both drones are unavailable, the dispatch cutoff has begun, or its first-in-line route options fail the current policy budget.

Every accepted mission reserves its complete outbound and return exposure. Mission completion transfers that reservation to measured exposure. The drone then charges at 4 battery percentage points per minute and becomes available only at 100 percent.

P0 selects the shortest route. P1 adds the population-weighted exposure term. P2 enforces fixed budgets of 40 proxy units. P3-U uses the proportional adaptive rule. P3-D applies the same rule with damping factor 0.50.

## Common demand streams

The generator uses seed 421 for the interactive demonstration. Every policy receives identical request counts, destinations, and within-period arrival times for a selected demand setting. Policy behavior does not alter the random-number stream.

The interactive run is one reproducible example. The [Phase C screening](phase-c.html) repeats the approved configurations across matched seeds.

## Outputs

The integrated page reports the following measures for each period:

- requests received, missions completed, and queue length;
- mean dispatch wait;
- route shares, distance, energy, and fleet utilization;
- neighborhood exposure and peak exposure;
- current and next budgets;
- delayed observation used by an adaptive controller.

Figure 1 presents the operational state for a selected period, while Figure 2 compares end-of-period queues across the five regimes.

Figure 1. Integrated route, mission, and neighborhood exposure state for the selected policy and review period.

Figure 2. End-of-period request queues under matched demand for P0, P1, P2, P3-U, and P3-D.

## Verification result

The integrated model has been checked across demand levels, population patterns, information delays, responsiveness settings, and all five regimes. The checks cover:

- identical demand streams across policies;
- request conservation between dispatch and the final queue;
- zero demand;
- exposure-budget enforcement;
- zero active reservations at review boundaries;
- nonnegative waits and queues;
- utilization bounds;
- observation delay;
- controller bounds;
- zero responsiveness reproducing fixed-budget behavior.

These checks establish rule consistency. They do not show that one policy performs better.

## Discussion

The integrated model now makes fleet capacity endogenous. P0 and P1 can accumulate queues under high demand, whereas the allocation-only model completed every request under those regimes. Exposure budgets can create further waiting because first-in-line requests remain queued when no route fits.

The prototype still uses two identical drones, unlimited charging access, one synthetic network, proxy exposure coefficients, and one interactive seed. The dispatch cutoff is a modeling choice that should be tested in sensitivity analysis. Its main implication is that regulatory accounting remains clear at the cost of delaying late-period requests.

Phase C executed the approved 4,140 matched-seed runs. The first [Phase D sensitivity analysis](phase-d.html) adds configurable fleet size, charging capacity, exposure scaling, controller damping, and route tie-breaking. The [joint Phase D analysis](phase-d-joint.html) adds destination patterns and crosses the influential operational assumptions.

The separate [Phase E2 integration](phase-e2.html) retains this proxy model as a baseline and replaces its route coefficients with calibrated moving-source energy vectors. It uses reference-mission budget scenarios because a legal or community budget has not been established.
