# Literature and research gap

The first scoping review tests whether the proposed contribution is distinct from existing work. It is an evidence map, not yet a systematic review.

## Review question

What modelling approaches connect urban drone delivery operations with community noise exposure, distributional equity, and adaptive regulation, and what remains unresolved when demand and operations change over time?

## Evidence reviewed

The first pass covers model reporting, urban logistics, drone acoustics, noise-aware routing, and distributional equity. Table 1 records representative studies that most directly affect the model design.

| Study | Existing contribution | Consequence for this project |
| --- | --- | --- |
| [Grimm et al. (2020)](https://doi.org/10.18564/jasss.4259) | Updated ODD protocol for transparent simulation descriptions | Use ODD for the model specification and a separate protocol for experiments and evaluation. |
| [Wise et al. (2018)](https://doi.org/10.1145/3284038.3284039) | Agent-based representation of individual last-mile delivery operations | Call drones or operators agents only when their state and decisions influence outcomes. |
| [Tan et al. (2023)](https://doi.org/10.1016/j.trd.2023.103686) | Virtual flight and acoustic propagation model for residential drone noise | Replace proxy exposure with a physical measure informed by vehicle state and propagation. |
| [Hui et al. (2021)](https://doi.org/10.3390/ijerph18178893) | Psychoacoustic experiment relating UAV sound metrics to annoyance | Keep physical exposure, perceived annoyance, and health effects conceptually separate. |
| [Tan et al. (2024)](https://doi.org/10.1016/j.trd.2024.104306) | Noise-conscious drone-station placement and truck-drone operations | Do not claim that integrating noise and drone logistics is new. |
| [Farazi and Zou (2024)](https://doi.org/10.1016/j.tre.2024.103661) | Population-exposure measure and cost-noise optimization with fleet constraints | Report service and exposure outcomes together. |
| [Gao et al. (2024)](https://doi.org/10.1016/j.trc.2024.104740) | Noise-aware and equitable air-traffic optimization | Compare adaptive regulation with fixed and optimized baselines using explicit equity measures. |
| [Zhou and Brandao (2023)](https://kclpure.kcl.ac.uk/portal/en/publications/noise-and-environmental-justice-in-drone-fleet-delivery-paths-a-s/) | Simulation audit of unequal drone-fleet noise and fairer route allocation | Test whether rerouting transfers exposure to different neighborhoods or groups. |
| [Noise-aware urban drone delivery (2026)](https://doi.org/10.1016/j.urbmob.2026.100231) | Integrates demand, spatial noise, population weighting, and noise-aware routes | Distinguish the proposed delayed regulatory feedback and dynamic fleet consequences. |

Table 1. Representative evidence from the first scoping pass and its effect on the research design.

## What is already established

Existing studies address noise-aware routes, acoustic propagation, population exposure, environmental justice, equitable air-traffic allocation, and agent-based logistics. The project should not claim that drone noise, equity, population weighting, or agent-based modelling is new.

## Candidate research gap

The initial review has not identified a model that combines all of the following:

- individual drone availability, mission duration, battery state, and request queues;
- neighborhood exposure accounts that constrain route allocation;
- budgets revised through delayed feedback from observed exposure;
- demand disturbances and different population distributions;
- joint evaluation of service loss, burden distribution, and feedback stability.

Research on drone delivery has developed methods for noise-aware routing, infrastructure placement, spatial exposure assessment, and equitable traffic allocation. However, the studies identified in this initial review mainly treat noise controls as fixed objectives, penalties, thresholds, or centrally optimized constraints. Less attention has been given to a regulatory feedback system in which neighborhood exposure budgets change in response to delayed observations while demand, drone availability, and route choices evolve.

This is a provisional gap. It must be checked against adjacent research on adaptive environmental limits, congestion control, quotas, and dynamic resource allocation.

## Proposed contribution

The project will develop a transparent simulation sandbox for evaluating adaptive neighborhood exposure regulation in urban drone delivery. It will connect operational agents and request queues to spatial exposure accounts and a regulator with explicit observation delay and responsiveness. Matched demand sequences will support comparisons among unrestricted, noise-aware, fixed-budget, and adaptive-budget regimes.

The evaluation will report delivery service, spatial burden, equity, and feedback stability. Reduced exposure alone will not be treated as evidence that a policy is better.

## Evidence still needed

| Area | Question |
| --- | --- |
| Dynamic regulation | Which feedback, quota, or adaptive-cap mechanisms have already been tested in other environmental and transport systems? |
| Exposure metric | Which physical metric and observation period can validly accumulate repeated drone events? |
| Regulatory authority | Which institution could impose neighborhood-level operating constraints in the intended jurisdiction? |
| Operational data | Which sources can estimate arrivals, payloads, mission times, fleet size, and charging? |
| Equity | Which population groups and existing environmental burdens should be represented? |
| Validation | Which observed patterns should the completed model reproduce? |

Table 2. Questions that must be resolved before the gap and empirical model are finalized.

## Next review action

The next pass will search adjacent regulation and control literatures and trace citations from the closest noise, equity, and routing studies. Each retained source will then receive a quality and relevance assessment before the thematic literature review is written.
