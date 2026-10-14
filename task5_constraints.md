# Task 5: Constraints, Formalisation & Violations

## System Constraints (10 Constraints)
1. **CN-01:** The robot must not enter a railway level-crossing if the ARLCCS barrier is open or closing while a train approaches.[cite: 1]
2. **CN-02:** The delivery robot must not be in the `DELIVERING` state while the conservation chamber door is open.[cite: 1]
3. **CN-03:** The chamber must not initiate active conservation unless the chamber door is completely closed and sealed.[cite: 1]
4. **CN-04:** The robot must immediately transition to Safe Mode if battery capacity falls below emergency limits.[cite: 1]
5. **CN-05:** Communication loss exceeding the timeout threshold must trigger autonomous fallback navigation or safe stopping.[cite: 1]
6. **CN-06:** Medicine batch removal is strictly prohibited when the protection mode response is active.[cite: 1]
7. **CN-07:** Significant vibration levels must suspend robot navigation and risky chamber operations immediately.[cite: 1]
8. **CN-08:** Operator commands must always be validated against role-based authorization before execution.[cite: 1]
9. **CN-09:** A waiting robot in the IDLE state must never transition directly into the DELIVERING state.[cite: 1]
10. **CN-10:** ARLCCS barriers must stay closed during train passage and open only when sensors confirm the crossing is clear.[cite: 1]

## Formalised Constraints & Violation Scenarios (8 Cases)
1. **Constraint:** Train approach requires closed barriers. (`Train_Present → ¬Barrier_Open`)[cite: 1]
   * *Violation Scenario:* `Train_Present = TRUE` and `Barrier_Open = TRUE`.[cite: 1]
   * *Response:* ARLCCS alarm triggers, barrier close command forced, and approaching robots receive stop signals.[cite: 1]

2. **Constraint:** No delivery while door is open. (`Robot_Delivering → ¬Door_Open`)[cite: 1]
   * *Violation Scenario:* `Robot_Delivering = TRUE` and `Door_Open = TRUE`.[cite: 1]
   * *Response:* System suspends delivery, logs safety violation, and alerts mission control.[cite: 1]

3. **Constraint:** Conservation requires closed door. (`Active_Conservation → ¬Door_Open`)[cite: 1]
   * *Violation Scenario:* `Active_Conservation = TRUE` and `Door_Open = TRUE`.[cite: 1]
   * *Response:* Conservation pauses, internal environment alerts activate, and operator is notified.[cite: 1]

4. **Constraint:** Critical battery forces Safe Mode. (`Critical_Battery → Safe_Mode_Active`)[cite: 1]
   * *Violation Scenario:* `Critical_Battery = TRUE` and `Safe_Mode_Active = FALSE`.[cite: 1]
   * *Response:* Watchdog forces Safe Mode, cuts non-essential power, and logs incident.[cite: 1]

5. **Constraint:** Batch removal safety check. (`Batch_Removal → (Chamber_Safe ∧ ¬Protection_Mode)`)[cite: 1]
   * *Violation Scenario:* `Batch_Removal = TRUE` and `Protection_Mode = TRUE`.[cite: 1]
   * *Response:* Door lock remains engaged, release command rejected, and operator alert generated.[cite: 1]

6. **Constraint:** Vibration suspension rule. (`High_Vibration → (¬Robot_Navigating ∧ ¬Chamber_Risky_Ops)`)[cite: 1]
   * *Violation Scenario:* `High_Vibration = TRUE` and `Robot_Navigating = TRUE`.[cite: 1]
   * *Response:* Robot halts navigation, suspends risky operations, and enters stabilization monitoring.[cite: 1]

7. **Constraint:** Role-based command authorization. (`Execute_Command → Authenticated_Authorized`)[cite: 1]
   * *Violation Scenario:* `Execute_Command = TRUE` and `Authenticated_Authorized = FALSE`.[cite: 1]
   * *Response:* Command rejected instantly, security event logged, and unauthorized attempt flagged.[cite: 1]

8. **Constraint:** IDLE state restriction. (`Robot_Idle → ¬Robot_Delivering`)[cite: 1]
   * *Violation Scenario:* `Robot_Idle = TRUE` and `Robot_Delivering = TRUE`.[cite: 1]
   * *Response:* State machine exception triggered, transition blocked, and error logged.[cite: 1]
