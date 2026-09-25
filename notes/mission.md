# Two requests, one available drone

This executable walkthrough implements the Phase B integration case. It connects a first-in, first-out request queue with individual drone availability, complete-mission battery checks, and exposure reservations. It is a browser prototype with assumed inputs rather than an empirically calibrated model.

Request R1 arrives at minute 0. Request R2 arrives at minute 2 while drone D1 is completing R1. The figure shows R2 waiting until D1 returns, then being reconsidered under the battery and exposure state that exists at dispatch time.

Figure 1. Event sequence for two overlapping delivery requests in the synthetic nine-neighborhood network. Circles show measured exposure, the diamond identifies D1, and squares identify the depot and customer. The request labels record queue, mission, delivery, and completion states.

## Model assumptions

Both requests have destination Z2. One drone starts at the depot with 100 percent battery. The queue follows first-in, first-out order. A request can leave the queue only when D1 is available and a complete outbound and return mission satisfies the battery reserve and neighborhood exposure budgets.

The central route has a return distance of 12 units. The upper and lower routes each have a return distance of 16 units. Flight speed is 2 distance units per minute. Battery use is 2 percentage points per distance unit, and the required return reserve is 20 percent. Delivery takes one minute.

All neighborhoods have a current exposure budget of 40 proxy units. Starting exposure for N1 through N9 is `[27, 26, 25, 24, 39.2, 24, 22, 23, 24]`. Complete return-trip increments are `[1.1, 0.8, 1.0, 1.0, 1.4, 1.1, 0.9, 1.0, 0.8]` along the chosen corridor. Each crossing transfers half of a neighborhood's mission contribution from reserved exposure to measured exposure.

The review period closes at minute 20 so both controlled missions finish before the regulatory update. This boundary is an illustrative verification setting rather than a proposed policy interval.

## Event sequence

The central route is unavailable to R1 because N5 would reach 40.6 units. The operator assigns the upper route and reserves its full exposure contribution before departure.

R2 arrives at minute 2. D1 is outbound on R1, so the operator records R2 as queued without assigning D1 a second mission. R1 reaches the customer at minute 4, completes delivery at minute 5, and returns at minute 9. Its reservation is then zero.

At minute 9, the operator reconsiders the first queued request. R2 has waited seven minutes. D1 has 68 percent battery, and the upper route remains feasible after accounting for R1's measured exposure. The operator reserves R2's complete mission contribution and dispatches it. R2 is delivered at minute 14 and D1 returns at minute 18 with 36 percent battery.

The two missions add 2.2 units to N1, 1.6 to N2, and 2.0 to N3. Final period exposures are 29.2, 27.6, and 27.0. All reservations are zero after the second return. At minute 20, the regulator uses the completed period exposure to calculate period 2 budgets with `next_budget = clip(current_budget - 0.6 * (exposure - 28), 6, 70)`.

## Verification result

The controlled sequence passes the Phase B gate:

- D1 is never assigned to overlapping missions.
- R2 remains queued while D1 is unavailable.
- The queue preserves request order and records a seven-minute wait.
- Each mission reserves its full outbound and return exposure before departure.
- Every crossing converts reserved exposure into measured exposure without making the reservation negative.
- Both return journeys are included, and all reservations finish at zero.
- The final battery remains above the required reserve.

These are implementation checks, not evidence about operational performance or policy effectiveness.

## Discussion

The prototype now shows that individual availability changes the timing of service: R2 waits even though an exposure-feasible route exists. This is the first mechanism that cannot be represented by the earlier allocation-only demonstration without adding an explicit capacity rule.

The case remains deterministic and uses one drone, one destination, no charging, and no delivery deadline. It does not test competition among several available drones or persistent queue growth. The next implementation should add a small fleet and charging state, then connect these operational events to the multi-period policy experiment. That comparison will show whether individual drone states materially change policy conclusions.
