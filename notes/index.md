# Research progress

Adaptive neighborhood exposure regulation for urban drone delivery.

<div class="release"><span>Current stage</span><span>Phase C stochastic screening complete</span><span>Updated 26 September 2026</span></div>

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
| Empirical acoustic exposure | Planned | Audit sources and define the exposure measure |
| Replicated policy comparisons | Planned | Compare fixed budgets at matched service levels |

Table 1. Development status for the current research prototype. "Demonstrated" means that the stated mechanism runs with illustrative inputs.

## Development record

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

- Which acoustic measure and observation window should replace the exposure proxy?
- Which fleet interactions are necessary to answer the regulatory question?

The next development milestone is Phase D sensitivity analysis. It will test whether the Phase C comparisons depend on fixed-budget spacing, damping, exposure coefficients, destination probabilities, fleet size, charging capacity, or route tie-breaking.

Progress entries are maintained with the project code. They change when a new version of the website is published.
