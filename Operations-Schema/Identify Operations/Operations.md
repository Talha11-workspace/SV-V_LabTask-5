# SV&V Lab Task 5 - Identify Operations

## Smart Museum Artifact Conservation System

| OP_ID | Operations | Purpose |
|---|---|---|
| O1 | Perform Power-On Self-Check | Test all essential sensors and environmental-control devices before normal operation. |
| O2 | Validate Essential Device Readiness | Confirm that every essential device is working correctly and can support safe conservation. |
| O3 | Initialize Environmental Monitoring | Start continuous collection, timestamping, and health monitoring of chamber conditions. |
| O4 | Register Artifact Identity | Record the artifact identification information and link it to the active chamber session. |
| O5 | Load Required Environmental Profile | Load and validate the artifact-specific temperature, humidity, light, vibration, and safety limits. |
| O6 | Authorize Conservation Start | Confirm that the artifact profile is loaded, sensors are ready, and the chamber door is closed. |
| O7 | Sample Chamber Conditions | Collect current temperature, humidity, light, vibration, door, artifact-condition, and power readings. |
| O8 | Evaluate Environmental Compliance | Compare current readings with the limits required for the artifact. |
| O9 | Issue Temperature Correction | Send a bounded control command to return temperature to the permitted range. |
| O10 | Issue Humidity Correction | Send a bounded control command to return humidity to the permitted range. |
| O11 | Verify Environmental Recovery | Use fresh sensor readings to confirm that a corrected condition has actually returned within limits. |
| O12 | Escalate Artifact Protection | Move from normal conservation priorities to protective action when recovery fails or risk is critical. |
| O13 | Apply Additional Protection Controls | Reduce light exposure and activate available auxiliary controls to reduce risk to the artifact. |
| O14 | Generate Conservation Alert | Notify the museum operator about protection events, serious faults, and required action. |
| O15 | Detect Vibration Hazard | Identify significant vibration and suspend activities that could increase risk to the artifact. |
| O16 | Verify Vibration Stabilization | Confirm that vibration remains below the permitted threshold for the required time. |
| O17 | Handle Door-Open Interruption | Immediately suspend normal conservation when the chamber door opens. |
| O18 | Verify Resumption Readiness | Recheck environmental conditions, artifact condition, and sensor status before resuming after a door interruption. |
| O19 | Manage Power Loss | Transfer to verified emergency power when available or initiate a safe response when it is unavailable. |
| O20 | Record Power Incident | Store a time-stamped record of power loss, emergency-power status, and the system response. |
| O21 | Confirm Safe Artifact Removal | Authorize removal only when the chamber is safe and no protection response is active. |

## Operation Categories

| Category | Operation IDs |
|---|---|
| Startup and readiness | O1, O2, O3 |
| Artifact setup | O4, O5, O6 |
| Monitoring and compliance | O7, O8 |
| Environmental recovery | O9, O10, O11 |
| Protection and alerting | O12, O13, O14 |
| Vibration response | O15, O16 |
| Door safety | O17, O18 |
| Power continuity and incident handling | O19, O20 |
| Artifact handling safety | O21 |
