# Task 3: State Modelling & Transition Verification

## States Definition
1. **IDLE (ST-01):** Robot is switched on and waiting for requests. *Entry:* System startup or warehouse return arrival. *Exit:* Valid delivery request received.[cite: 1]
2. **NAVIGATING (ST-02):** Robot travels toward the destination clinic. *Entry:* Delivery request processed from IDLE. *Exit:* Destination reached, obstacle detected, or critical battery.[cite: 1]
3. **AVOIDING_OBSTACLE (ST-03):** Robot halts and navigates around a physical blockage. *Entry:* Obstacle detected signal during navigation. *Exit:* Obstacle cleared signal.[cite: 1]
4. **DELIVERING (ST-04):** Robot unloads or dispenses the medicine batch at the clinic. *Entry:* Destination reached confirmation. *Exit:* Delivery complete.[cite: 1]
5. **RETURNING (ST-05):** Robot travels back to the central warehouse. *Entry:* Delivery complete or critical battery abort. *Exit:* Warehouse arrival (transitions to IDLE).[cite: 1]
6. **SAFE_MODE (ST-06):** Restricted state protecting core hardware during faults. *Entry:* Critical battery or thermal breach (CR-01). *Exit:* Manual reset by authorized operator.[cite: 1]
7. **LINK_LOST (ST-07):** Fallback state when communication link fails for extended duration. *Entry:* Telemetry timeout. *Exit:* Communication restored.[cite: 1]

## State Transition Table & Verification
| Current State | Event / Condition | Next State | Req ID | Valid? | Verification Evidence |
| :--- | :--- | :--- | :--- | :--- | :--- |
| IDLE | DeliveryRequest | NAVIGATING | FR-04 | Yes | Request triggers navigation engine[cite: 1] |
| NAVIGATING | ObstacleDetected | AVOIDING_OBSTACLE | FR-04 | Yes | Sensor interrupt suspends motion[cite: 1] |
| AVOIDING_OBSTACLE | ObstacleCleared | NAVIGATING | FR-04 | Yes | Path clear signal resumes travel[cite: 1] |
| NAVIGATING | DestinationReached | DELIVERING | FR-04 | Yes | GPS/Destination coords matched[cite: 1] |
| DELIVERING | DeliveryComplete | RETURNING | FR-04 | Yes | Unload confirmation received[cite: 1] |
| RETURNING | WarehouseArrival | IDLE | FR-04 | Yes | Base dock sensors triggered[cite: 1] |
| NAVIGATING / DELIVERING | CriticalBattery | RETURNING (Abort) | FR-08 | Yes | Low power forces return route[cite: 1] |
| ANY ACTIVE STATE | CriticalThermal / Emergency | SAFE_MODE | CR-01 | Yes | Thermal sensor trip forces Safe Mode[cite: 1] |

## Verifier Investigation Notes
* **(a) Can IDLE -> DELIVERING happen?** No, doing so violates **SAFE-04** as delivery cannot occur without prior navigation/request processing[cite: 1].
* **(b) What if AVOIDING_OBSTACLE has no transition back?** The robot would remain permanently stuck and unable to complete delivery[cite: 1].
* **(c) Can AVOIDING_OBSTACLE -> DELIVERING occur?** No, bypassing navigation/obstacle clearance directly to delivery is unsafe[cite: 1].
* **(d) What happens on CriticalBattery during AVOIDING_OBSTACLE or DELIVERING?** Missing transitions can cause deadlocks or unsafe failures, hence abort/return handlers must be active[cite: 1].
