# Project overview

This study asks when adaptive neighborhood exposure regulation improves the distribution of drone-delivery burdens without excessive delivery loss or shifting exposure elsewhere.

## Research question

Under what demand, spatial, and regulatory conditions does adaptive neighborhood exposure regulation improve the equity of urban drone delivery?

The comparison includes a shortest-path baseline, population-weighted noise-aware routing, a fixed exposure budget, and an adaptive budget. The study uses a synthetic city so that individual mechanisms can be inspected before introducing empirical spatial inputs.

## Scope

The model contains one delivery operator and one regulator. The planned drone fleet carries parcels through a route network, with exposure accumulated by neighborhood. Population and socioeconomic attributes support distributional analysis.

The current integrated prototype connects 24 policy periods with within-period request arrivals, configurable fleets, constrained charging, queues, and simultaneous reservations. Phase C evaluates 138 configurations across 30 matched seeds. Two Phase D modules add 874 sensitivity configurations, including joint destination, fleet, charging, and exposure tests. The earlier allocation-only and controlled fleet demonstrations remain available for verification. An [empirical dataset audit](empirical-calibration.html) identifies the available evidence, the [Phase E0 module](phase-e0.html) implements the calibrated single-event calculation, and [Phase E1](phase-e1.html) integrates moving missions across nine receivers in a synthetic physical network. Operational demand, vehicle energy, charging, and case-area inputs remain missing.

## Intended evidence

The research will compare delivery completion with neighborhood exposure and its distribution. It will also examine whether delayed regulatory responses cause repeated tightening and relaxation.

Data sources named in the proposal require an accessibility and compatibility audit before their use. No current demonstration input is described as calibrated.

## Discussion

The current work establishes traceable rules and reports replicated synthetic comparisons. Its synthetic geometry and proxy exposure restrict what can be concluded. Phase D finds material dependence on fleet capacity and exposure scaling across both separate and joint tests. Its progression gate remains open until the exposure measure and budget relationship are calibrated.

[Inspect the model specification](specification.html)
