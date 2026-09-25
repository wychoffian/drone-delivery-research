# Interactive pilot

Two complementary demonstrations make the proposed rules inspectable. Both use a synthetic network and assumed exposure coefficients.

<div class="feature-grid"><a class="feature" href="mission.html"><span class="number">01</span><h3>Queue and availability</h3><p>Follow two requests as the second waits for the only drone to return.</p><span class="link-label">Follow the missions</span></a><a class="feature" href="policy.html"><span class="number">02</span><h3>Policy feedback</h3><p>Compare fixed and adaptive exposure budgets under changing demand and delayed observations.</p><span class="link-label">Explore the experiment</span></a></div>

## What each demonstration can establish

The mission walkthrough tests the sequence of events, queueing, drone availability, and return-trip accounting. R2 waits while D1 completes R1, then receives a new feasibility check before dispatch.

The policy demonstration tests how route allocation and delayed regulation interact over 24 periods. It has no fleet capacity constraint, so P0 and P1 serve every request by construction.

Neither demonstration validates acoustics or demonstrates a real-world advantage of adaptive regulation. Their current role is to reveal missing rules and support model design discussions.
