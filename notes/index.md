# Research progress

Adaptive neighborhood exposure regulation for urban drone delivery.

<div class="release"><span>Current stage</span><span>Phase B queue integration complete</span><span>Updated 26 September 2026</span></div>

## Ready to inspect

<div class="feature-grid"><a class="feature" href="mission.html"><span class="number">01</span><h3>Follow two requests</h3><p>Inspect queueing, drone availability, battery use, and exposure reservations.</p><span class="link-label">Open mission walkthrough</span></a><a class="feature" href="policy.html"><span class="number">02</span><h3>Explore the four dials</h3><p>Change demand and population concentration, then inspect the effect of regulatory delay and responsiveness.</p><span class="link-label">Open policy experiment</span></a></div>

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
| Small fleet and charging constraints | Planned | Add two drones and explicit charging state |
| Empirical acoustic exposure | Planned | Audit sources and define the exposure measure |
| Replicated policy comparisons | Planned | Compare fixed budgets at matched service levels |

Table 1. Development status for the current research prototype. "Demonstrated" means that the stated mechanism runs with illustrative inputs.

## Development record

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

The next development milestone is a small fleet with explicit charging. Its purpose is to test drone selection, simultaneous reservations, utilization, and queue growth before the operational model is connected to the multi-period policy experiment.

Progress entries are maintained with the project code. They change when a new version of the website is published.
