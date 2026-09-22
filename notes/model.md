# Model and rules

The operator chooses routes. Drone missions generate exposure, and the regulator uses delayed observations to change the next period's budgets.

## System map

Figure 1 shows the operational sequence and its feedback loop.

<figure class="system-map"><ol class="flow"><li>Delivery requests</li><li>Operator agent<small>Choose a feasible route</small></li><li>Drone agents<small>Execute outbound and return missions</small></li><li>Neighborhood exposure<small>Accumulate flight contributions</small></li><li>Regulator agent<small>Read delayed observations</small></li><li>Next-period budgets<small>Constrain subsequent route choices</small></li></ol><figcaption>Figure 1. Regulatory feedback loop. The final budget feeds back into the operator's route choice. The route network supplies feasible alternatives; population weights enter noise-aware routing and distributional analysis.</figcaption></figure>

## Agents and records

Table 2 distinguishes decision makers from the information they use.

| Entity | Role | Current implementation |
| --- | --- | --- |
| Operator | Select the cheapest feasible route | Implemented in both demonstrations |
| Regulator | Set fixed or adaptive exposure budgets | Implemented in both demonstrations |
| Drone | Execute missions and track its state | One drone in the mission walkthrough |
| Neighborhood | Hold population and exposure records | A spatial record, not a behavioral agent |
| Request | Specify a delivery task | An input record |

Table 2. Entity roles and implementation status. The demonstrations are separate prototypes, not an integrated fleet model.

## Four experimental factors

- Demand changes the number of requests.
- Population concentration changes residential distribution while holding total population constant.
- Information delay changes the age of the observation available to the regulator.
- Responsiveness changes the magnitude of a budget adjustment.

The population factor does not currently change building geometry or the route network. Delay and responsiveness apply only to the adaptive policy.

## Regulatory rule

The adaptive rule adjusts an exposure budget measured in the same units as exposure:

```text
next budget = current budget - gain × (observed exposure - target)
```

Lower and upper bounds constrain the result. Updates occur after a period closes and apply to the next period. The target guides the update; the current budget determines which missions can be accepted.

## Implementation direction

The present prototypes execute in the browser. The proposed research implementation uses Python with Mesa for agent states, NetworkX for routes, and GeoPandas for spatial data. This remains a proposed stack, not an implemented Python model.

[Read the complete specification and sources](specification.html)

[Read the formal ODD 2020 model specification](odd.html)
