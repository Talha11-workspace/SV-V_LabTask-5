# SV&V Lab Task 5 — Identify Operations

## Smart Museum Artifact Conservation System

The following operations are performed by the system. Operation names describe actions or decisions and do not reuse system state names.

| Operation ID | Operation name | Trigger or input | Main purpose | Result or output |
|---|---|---|---|---|
| O1 | Perform Power-On Self-Check | Chamber powered on | Test sensors and environmental-control devices before normal operation. | Self-check result and device diagnostic records. |
| O2 | Validate Essential Device Readiness | Self-check results | Confirm that all essential sensors and control devices are working correctly. | Readiness decision; failed devices are identified. |
| O3 | Initialize Environmental Monitoring | Successful readiness validation | Start continuous observation of chamber and artifact-related measurements. | Monitoring services enabled. |
| O4 | Register Artifact Identity | Artifact placed inside chamber | Record the artifact identification information. | Artifact record created and linked to the chamber session. |
| O5 | Load Required Environmental Profile | Artifact record and conservation limits | Load the artifact-specific temperature, humidity, light, vibration, and safety limits. | Active environmental profile available for comparison. |
| O6 | Authorize Conservation Start | Door closed and profile loaded | Confirm that the conditions required for normal conservation are satisfied. | Conservation activity may begin, or a blocking reason is recorded. |
| O7 | Sample Chamber Conditions | Periodic monitoring cycle | Read temperature, humidity, light exposure, vibration, door status, artifact condition, and power availability. | Timestamped sensor sample. |
| O8 | Evaluate Environmental Compliance | Current sample and artifact profile | Compare measured conditions with the permitted ranges. | Compliance result for each monitored environmental factor. |
| O9 | Issue Temperature Correction | Temperature outside permitted range | Command the environmental-control mechanism to restore temperature. | Temperature-recovery command and recovery timer. |
| O10 | Issue Humidity Correction | Humidity outside permitted range | Command the environmental-control mechanism to restore humidity. | Humidity-recovery command and recovery timer. |
| O11 | Verify Environmental Recovery | Recovery command and new sensor readings | Confirm through measurements that the corrected condition is actually within limits. | Verified recovery or failed-recovery result. |
| O12 | Escalate Artifact Protection | Recovery period expires without compliance | Prioritize artifact protection over normal conservation operation. | Protection actions enabled and normal operation suspended. |
| O13 | Apply Additional Protection Controls | Protection response active | Reduce light exposure and activate additional environmental controls where available. | Reduced-risk chamber configuration. |
| O14 | Generate Conservation Alert | Protection response or serious abnormality | Notify the museum operator of the condition and required attention. | Alert containing severity, cause, time, and chamber details. |
| O15 | Detect Vibration Hazard | Vibration sensor exceeds permitted threshold | Identify significant vibration while an artifact is inside. | Vibration event recorded and risk-increasing activities suspended. |
| O16 | Verify Vibration Stabilization | Vibration has fallen below threshold | Confirm that vibration remains below the limit for the required stabilization period. | Stabilization verified or rejected. |
| O17 | Handle Door-Open Interruption | Door opens during conservation | Immediately suspend normal conservation activities while the chamber is open. | Door interruption recorded and normal environmental operation paused. |
| O18 | Verify Resumption Readiness | Door closes after an interruption | Recheck artifact conditions and sensor readiness before resuming normal activity. | Resume authorization or continued suspension. |
| O19 | Manage Power Loss | Power unavailable during conservation | Switch to emergency power when available and determine the safe response. | Emergency supply active or safe-shutdown command issued. |
| O20 | Record Power Incident | Power loss or emergency supply failure | Preserve the incident details for audit and operator review. | Time-stamped power incident record. |
| O21 | Confirm Safe Artifact Removal | Operator requests artifact removal | Verify safe chamber conditions and ensure no active protection response is underway. | Removal authorized or denied with reasons. |

## Operation Grouping

| Group | Included operations |
|---|---|
| Startup and readiness | O1, O2, O3 |
| Artifact setup | O4, O5, O6 |
| Routine monitoring | O7, O8 |
| Environmental recovery | O9, O10, O11 |
| Protection and alerting | O12, O13, O14 |
| Vibration response | O15, O16 |
| Door safety | O17, O18 |
| Power continuity and shutdown | O19, O20 |
| Artifact handling safety | O21 |

## Key Behavioral Rules

1. Normal conservation cannot begin until the essential self-check succeeds.
2. An open chamber door blocks normal conservation and suspends active environmental operation.
3. A correction command is not proof of recovery; sensor readings must verify recovery.
4. Protection mode requires protective controls and an operator alert when recovery fails.
5. Vibration response cannot end until the vibration remains below the threshold for the required stabilization period.
6. Emergency power is used when available; otherwise the system records the incident and performs a safe shutdown.
7. Artifact removal is allowed only after the system confirms safe conditions and no active protection response.
