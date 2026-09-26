# Research progress

Adaptive neighborhood exposure regulation for urban drone delivery.

<div class="release"><span>Current stage</span><span>Phase E10 rolling adaptive controllers published</span><span>Updated 26 September 2026</span></div>

## Ready to inspect

<div class="feature-grid"><a class="feature" href="mission.html"><span class="number">01</span><h3>Follow the small fleet</h3><p>Inspect two drones, three requests, concurrent reservations, queueing, and charging.</p><span class="link-label">Open fleet walkthrough</span></a><a class="feature" href="fleet-policy.html"><span class="number">02</span><h3>Run the integrated model</h3><p>Change the four dials and compare five policies with fleet capacity and queues.</p><span class="link-label">Open integrated experiment</span></a></div>

These demonstrations use assumed inputs and proxy exposure units. They test model logic; they do not establish empirical policy findings.

## Development milestones

The milestones below distinguish completed prototypes from research work that remains.

| Milestone | Status | Evidence or next step |
| --- | --- | --- |
| Research question and bounded model | Drafted | [Model and rules](model.html) |
| Literature evidence and candidate gap | First scoping pass | [Literature and gap](literature.html) |
| ODD model specification | Drafted | [ODD specification](odd.html) |
| Simulation sandbox protocol | Approved and documented | [Sandbox protocol](sandbox.html) |
| Route allocation and regulatory feedback | Demonstrated | [Four-dial experiment](policy.html) |
| One drone with mission state and timing | Demonstrated | [Delivery walkthrough](mission.html) |
| Two-request queue and drone availability | Demonstrated | [Queue and availability walkthrough](mission.html) |
| Small fleet and charging constraints | Demonstrated | [Fleet and charging walkthrough](mission.html) |
| Integrated fleet and policy periods | Demonstrated | [Integrated fleet and policy model](fleet-policy.html) |
| Phase C matched-seed screening | Complete | [Review 4,140 runs with uncertainty](phase-c.html) |
| Phase D targeted sensitivity | First module complete, gate open | [Review 6,780 sensitivity runs](phase-d.html) |
| Phase D joint sensitivity | Complete, gate open | [Review 19,440 joint runs](phase-d-joint.html) |
| Empirical dataset audit | Complete | [Review the calibration map and data gaps](empirical-calibration.html) |
| Phase E0 acoustic module | Implemented | [Inspect the calibrated calculation and validation boundary](phase-e0.html) |
| Phase E1 route-level acoustic exposure | Implemented | [Inspect the moving-source exposure calculation](phase-e1.html) |
| Phase E2 acoustic fleet-policy integration | Implemented | [Inspect physical reservations and scenario budgets](phase-e2.html) |
| Phase E3 acoustic sensitivity | Complete | [Review 3,900 matched-seed runs](phase-e3.html) |
| Phase E4 adaptive controller diagnosis | Complete | [Review 1,170 diagnostic runs](phase-e4.html) |
| Phase E5 long-horizon threshold analysis | Complete | [Review 2,100 long-horizon runs](phase-e5.html) |
| Phase E6 allocation benchmark | Complete | [Review 20,160 allocation results](phase-e6.html) |
| Phase E7 event-integrated planning | Complete | [Review 13,440 event runs](phase-e7.html) |
| Phase E8 workload-aware planning | Complete | [Review 13,440 time-aware runs](phase-e8.html) |
| Phase E9 online rolling dispatch | Complete | [Review 600 long-horizon runs](phase-e9.html) |
| Phase E10 rolling adaptive-policy comparison | Complete, pilot operating gate open | [Review 1,380 controller runs](phase-e10.html) |
| Pilot capacity envelope | Next | Find demand and fleet combinations with stable late-window queues |

Table 1. Development status for the current research prototype. "Demonstrated" means that the stated mechanism runs with illustrative inputs.

## Development record

### 26 September 2026: Phase E10 rolling adaptive controllers

Executed 1,380 runs over 120 periods under medium and high demand. P3-D 0.50 reduces late-window budget variation and retains a small service advantage near target 14 after approximate exposure matching. Both adaptive controllers saturate at target 18, and both demand scenarios remain overloaded after demand increases. [Review the Phase E10 results](phase-e10.html).

### 26 September 2026: Phase E9 online rolling dispatch

Executed 600 runs over 120 periods with carried queues and no future request knowledge. Rolling 3 removes the budget 14.05 allocation discontinuity, balances destination completion and corridor use, and matches Rolling 10 with lower computation. Sustained high demand remains structurally overloaded. [Review the Phase E9 results](phase-e9.html).

### 26 September 2026: Phase E8 workload-aware planning

Executed 13,440 event runs across 480 cases, seven budgets, and three route-workload limits. A 930-minute cap executes about 99.7 percent of planned missions at higher budgets, while a 960-minute cap raises throughput with lower strict plan completion. Both retain the allocation benefit near budget 14. [Review the Phase E8 results](phase-e8.html).

### 26 September 2026: Phase E7 event-integrated planned allocation

Executed 13,440 one-period event runs across 480 demand cases and 14 budgets. The planned dispatcher removes the allocation discontinuity around budget 14 and executes almost every optimized mission through budget 15. At higher budgets, route duration and arrival timing make the 46-mission cap optimistic. [Review the Phase E7 results](phase-e7.html).

### 26 September 2026: Phase E6 allocation benchmark

Compared FIFO-stop, FIFO-skip, and an integer optimization upper bound across 480 demand cases and 14 budgets, producing 20,160 case-method-budget rows. At budget 14.05, the greedy rule serves 23 missions and the benchmark serves 39. Skipping blocked requests does not close the gap. The benchmark restores monotonic service, so the next step is an optimization-guided dispatcher linked to the event model. [Review the Phase E6 results](phase-e6.html).

### 26 September 2026: Phase E5 long-horizon threshold analysis

Executed 70 configurations with 30 matched seeds over 120 periods, producing 2,100 runs. The strong 24-period result for slow damping does not persist. The analysis also finds non-monotonic fixed-budget service around 14, caused by the greedy route allocator, residual nine-receiver capacity, and the carried FIFO queue. Controller ranking is paused until the allocation rule is benchmarked. [Review the Phase E5 results](phase-e5.html).

### 26 September 2026: Phase E4 adaptive controller diagnosis

Executed 39 configurations with 30 matched seeds, producing 1,170 runs. P3-D with damping 0.50 is stable in the reference case, but its budget crosses a discrete route-feasibility threshold near the target of 14. First-in-line blocking is negligible. Slower damping preserves service during the 24-period horizon because it has not settled, so the next experiment must extend the horizon and map the threshold. [Review the Phase E4 diagnosis](phase-e4.html).

### 26 September 2026: Phase E3 acoustic sensitivity

Executed 130 configurations with 30 matched seeds, producing 3,900 runs. Physical mission duration makes fleet capacity binding, positive acoustic-error stress reduces regulated service, and speed affects both capacity and exposure. Phase E4 later traced the P3-D result to the target and discrete mission-feasibility thresholds. [Review the Phase E3 results](phase-e3.html).

### 26 September 2026: Phase E2 acoustic fleet-policy integration

Replaced proxy increments in a separate integration prototype with nine-receiver acoustic energy vectors. Dispatch now reserves the complete physical vector, completion transfers it to observed exposure, and regulated policies compare energy against declared reference-mission budgets. [Inspect the Phase E2 model](phase-e2.html).

### 26 September 2026: Phase E1 route acoustic integration

Assigned physical coordinates to the synthetic network and integrated each outbound and return flight in one-second maximum steps. Every mission now produces a nine-receiver energy vector, including cross-corridor exposure. Policy budgets remain separate. [Inspect the Phase E1 module](phase-e1.html).

### 26 September 2026: Phase E0 acoustic module

Implemented the calibrated received-level, directivity, event-exposure, and cumulative energy calculations. The module records the DroneNoise parameters and NASA validation results, exposes the geometry and duration assumptions, and leaves policy budgets unchanged. [Inspect the Phase E0 module](phase-e0.html).

### 26 September 2026: empirical dataset audit

Audited the supplied 1.8 GB research folder and mapped verified datasets to the current model. DroneNoise and NASA measurements can support the first acoustic calibration step. The Kawai et al. JASA dataset adds 2,340 laboratory annoyance ratings, including 1,440 drone ratings, for a later event-response model. Delivery demand, vehicle energy, charging, target-area population, observed complaints, and policy budgets still need separate evidence. [Review the empirical calibration plan](empirical-calibration.html).

### 26 September 2026: Phase D joint sensitivity

Executed 648 joint configurations with 30 matched seeds, producing 19,440 runs. The experiment combines destination patterns, fleet size, charger capacity, exposure scaling, demand, and four policy variants. Destination patterns have little effect on P2 and P3-D completion in the selected high-demand setting, while exposure scaling remains influential. [Review the joint sensitivity results](phase-d-joint.html).

### 26 September 2026: Phase D targeted sensitivity

Executed 226 configurations with 30 matched seeds, producing 6,780 runs. The analysis varies fleet size, charger capacity, fixed budgets, exposure scaling, controller damping, and route tie-breaking. Fleet capacity and exposure scaling materially change the results, so the Phase D progression gate remains open. [Review the Phase D sensitivity results](phase-d.html).

### 26 September 2026: Phase C matched-seed screening

Executed 138 unique configurations with 30 matched seeds, producing 4,140 run records. The published results report 95 percent Monte Carlo intervals for service, operations, exposure, distribution, and controller behavior. All findings remain synthetic. [Review the Phase C results](phase-c.html).

### 26 September 2026: integrated fleet and policy model

Connected within-period arrivals, two drones, charging, first-in-first-out queues, exposure reservations, and review-boundary regulation across 24 periods. The model now includes P0, P1, P2, P3-U, and P3-D. Verification covers 360 factor-policy combinations in addition to the allocation and mission checks. [Run the integrated model](fleet-policy.html).

### 26 September 2026: small fleet and charging

Added D2, R3, concurrent exposure reservations, deterministic drone selection, and charging. R3 arrives while D1 is charging and D2 is returning, waits seven minutes, and dispatches when D1 becomes available. Verification covers simultaneous reservations, assignment uniqueness, queue state, charging completion, battery reserve, and exposure conservation. [Inspect the fleet sequence](mission.html).

### 26 September 2026: two-request integration

Implemented the Phase B event sequence. R2 arrives while D1 is active, waits seven minutes in a first-in, first-out queue, and is dispatched only after D1 returns and passes new battery and exposure checks. Verification covers double assignment, queue order, complete reservations, exposure debits, and the return battery reserve. [Inspect the event sequence](mission.html).

### 26 September 2026: simulation sandbox protocol

Defined the sandbox boundary, approved factors, policy regimes, 4,140-run screening design, common-random-number rule, outcome measures, failure conditions, and progression gates. This protocol established the two-request Phase B gate that the current mission walkthrough now implements. [Read the sandbox protocol](sandbox.html).

### 22 September 2026: ODD model specification

Converted the conceptual model into the ODD 2020 structure. The specification separates implemented prototypes from the target integrated model and defines entities, scheduling, design concepts, submodels, inputs, outputs, and fitness-for-purpose checks. [Read the ODD specification](odd.html).

### 22 September 2026: literature scoping and gap revision

Reviewed representative work on agent-based logistics, drone acoustics, noise-aware routing, distributional equity, aviation noise quotas, and transport feedback control. Noise-aware routing, equitable allocation, operating quotas, and feedback controllers already exist. The candidate contribution is therefore narrowed to evaluating delayed neighborhood exposure budgets within individual drone operations under changing demand. [Read the evidence map](literature.html).

### 21 September 2026: traceable drone mission

Added mission timing and battery consumption to a single delivery. The operator reserves exposure for the return journey before departure. Checks cover chronological crossings, battery reserve, and the timing of the regulatory update. [Read the walkthrough](mission-notes.html).

### 20 September 2026: regulatory prototype

Implemented the four experimental factors and policy regimes P0 through P3. Verification covered budget enforcement and delayed updates across 384 policy runs, including zero-demand and zero-responsiveness cases. These were code checks, not research replications.

## Decisions for the next mentor meeting

- Which review window and policy-budget definition should govern the physical exposure account?
- Which fleet interactions are necessary to answer the regulatory question?

The next milestone is the pilot capacity envelope. The project should vary demand and fleet size with Rolling 3 and P3-D 0.50, then identify configurations where late-window queues stop growing. Policy budgets remain scenarios until their basis is agreed.

Progress entries are maintained with the project code. They change when a new version of the website is published.
