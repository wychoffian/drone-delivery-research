# Research progress

Adaptive neighborhood exposure regulation for urban drone delivery.

<div class="release"><span>Current stage</span><span>Phase E1 route acoustics implemented</span><span>Updated 26 September 2026</span></div>

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
| Acoustic fleet-policy integration | Next | Replace proxy route coefficients with reserved energy vectors |
| Replicated policy comparisons | Planned | Compare fixed budgets at matched service levels |

Table 1. Development status for the current research prototype. "Demonstrated" means that the stated mechanism runs with illustrative inputs.

## Development record

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

The next milestone is acoustic fleet-policy integration. The project will reserve each mission's nine-receiver energy vector before dispatch and transfer it to the observed account at completion. Policy budgets remain declared scenarios until their basis is agreed.

Progress entries are maintained with the project code. They change when a new version of the website is published.
