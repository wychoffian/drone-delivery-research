# Interactive pilot

Three complementary demonstrations make the proposed rules inspectable. All use a synthetic network and assumed exposure coefficients.

<div class="feature-grid"><a class="feature" href="mission.html"><span class="number">01</span><h3>Fleet and charging</h3><p>Follow three requests across two drones, concurrent reservations, queueing, and charging.</p><span class="link-label">Follow the fleet</span></a><a class="feature" href="policy.html"><span class="number">02</span><h3>Allocation and feedback</h3><p>Compare fixed and adaptive budgets without a fleet-capacity constraint.</p><span class="link-label">Explore the allocation model</span></a><a class="feature" href="fleet-policy.html"><span class="number">03</span><h3>Integrated model</h3><p>Connect two-drone operations with all five policy regimes over 24 review periods.</p><span class="link-label">Run the integrated model</span></a></div>

## What each demonstration can establish

The fleet walkthrough tests event ordering, deterministic drone selection, simultaneous reservations, queueing, charging, and return-trip accounting. R3 waits while D1 charges and D2 returns, then receives a new feasibility check before dispatch.

The policy demonstration tests how route allocation and delayed regulation interact over 24 periods. It has no fleet capacity constraint, so P0 and P1 serve every request by construction.

The integrated demonstration adds within-period arrivals, two drones, charging, carried queues, simultaneous reservations, and the damped P3-D controller. It provides the executable basis for Phase C, but its interactive seed is not a replicated experiment.

None of the demonstrations validates acoustics or demonstrates a real-world advantage of adaptive regulation. Their current role is to reveal missing rules and support model design discussions.
