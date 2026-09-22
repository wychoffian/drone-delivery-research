# Research progress

Adaptive neighborhood exposure regulation for urban drone delivery.

<div class="release"><span>Current stage</span><span>Literature scoping and executable prototype</span><span>Updated 22 September 2026</span></div>

## Ready to inspect

<div class="feature-grid"><a class="feature" href="mission.html"><span class="number">01</span><h3>Follow one delivery</h3><p>Inspect route choice, battery use, and the exposure recorded before a regulatory update.</p><span class="link-label">Open mission walkthrough</span></a><a class="feature" href="policy.html"><span class="number">02</span><h3>Explore the four dials</h3><p>Change demand and population concentration, then inspect the effect of regulatory delay and responsiveness.</p><span class="link-label">Open policy experiment</span></a></div>

These demonstrations use assumed inputs and proxy exposure units. They test model logic; they do not establish empirical policy findings.

## Development milestones

The milestones below distinguish completed prototypes from research work that remains.

| Milestone | Status | Evidence or next step |
| --- | --- | --- |
| Research question and bounded model | Drafted | [Model and rules](model.html) |
| Literature evidence and candidate gap | First scoping pass | [Literature and gap](literature.html) |
| Route allocation and regulatory feedback | Demonstrated | [Four-dial experiment](policy.html) |
| One drone with mission state and timing | Demonstrated | [Delivery walkthrough](mission.html) |
| Multiple requests and fleet constraints | Planned | Test dispatch while a drone is busy |
| Empirical acoustic exposure | Planned | Audit sources and define the exposure measure |
| Replicated policy comparisons | Planned | Compare fixed budgets at matched service levels |

Table 1. Development status for the current research prototype. "Demonstrated" means that the stated mechanism runs with illustrative inputs.

## Development record

### 22 September 2026: literature scoping and gap revision

Reviewed representative work on agent-based logistics, drone acoustics, noise-aware routing, distributional equity, aviation noise quotas, and transport feedback control. Noise-aware routing, equitable allocation, operating quotas, and feedback controllers already exist. The candidate contribution is therefore narrowed to evaluating delayed neighborhood exposure budgets within individual drone operations under changing demand. [Read the evidence map](literature.html).

### 21 September 2026: traceable drone mission

Added mission timing and battery consumption to a single delivery. The operator reserves exposure for the return journey before departure. Checks cover chronological crossings, battery reserve, and the timing of the regulatory update. [Read the walkthrough](mission-notes.html).

### 20 September 2026: regulatory prototype

Implemented the four experimental factors and policy regimes P0 through P3. Verification covered budget enforcement and delayed updates across 384 policy runs, including zero-demand and zero-responsiveness cases. These were code checks, not research replications.

## Decisions for the next mentor meeting

- Which acoustic measure and observation window should replace the exposure proxy?
- Which fleet interactions are necessary to answer the regulatory question?

The next development milestone is a second request arriving during an active mission. Its purpose is to test availability and exposure reservations before adding more drones.

Progress entries are maintained with the project code. They change when a new version of the website is published.
