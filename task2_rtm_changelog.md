# Task 2: Change Requests, Traceability Matrix & Test Cases

## Version-Controlled Change Log
| CR ID | Original Requirement | Changed Requirement | Reason |
| :--- | :--- | :--- | :--- |
| **CR-01** | The robot shall enter Safe Mode when a critical battery or thermal condition is detected.[cite: 1] | The robot shall enter Safe Mode within 3 seconds when chamber or battery temperature exceeds the critical threshold or battery capacity falls below the emergency level.[cite: 1] | Adds precise time bounds and specific trigger conditions for full testability.[cite: 1] |
| **CR-02** | The system shall support communication with multiple robots simultaneously.[cite: 1] | The system shall support at least 20 simultaneously connected robots.[cite: 1] | Replaces vague scalability terms with a clear quantitative metric matching the pilot scope.[cite: 1] |
| **CR-03** | Only authenticated operators shall be permitted to issue robot commands.[cite: 1] | The system shall require authenticated and role-authorised operators before accepting any robot command.[cite: 1] | Clarifies authorization constraints by explicitly coupling authentication with role-based checks.[cite: 1] |

## Test Cases Traceable to Scenario Steps
* **TC-01 (Command Received):** Send command via web portal -> Verified by Auth module -> Logged successfully. *(Re-run if CR-03 changes)*[cite: 1]
* **TC-02 (Authenticated):** Issue command with valid credentials and role -> Accepted by command processor. *(Re-run if CR-03 changes)*[cite: 1]
* **TC-03 (Executed):** Robot executes navigation request -> State changes from IDLE to NAVIGATING.[cite: 1]
* **TC-04 (Status Reported):** Telemetry reports real-time battery and temperature metrics through low-bandwidth link. *(Re-run if CR-01 or CR-02 changes)*[cite: 1]
* **TC-05 (Logged):** Event log records transaction details and timestamps accurately.[cite: 1]
* **TC-06 (Safe Mode Trigger):** Simulate thermal limit breach -> Verify Safe Mode entry within 3 seconds. *(Re-run if CR-01 changes)*[cite: 1]
* **TC-07 (Multi-Robot Load):** Simulate 20 concurrent robot connections -> Verify portal responsiveness. *(Re-run if CR-02 changes)*[cite: 1]
* **TC-08 (Obstacle Handling):** Trigger obstacle sensor during navigation -> Verify pause and resume sequence.[cite: 1]
