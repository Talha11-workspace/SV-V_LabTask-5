# SV&V Lab Task 5 — Complete Operation Schema

## Smart Museum Artifact Conservation System

This document expands the operations identified in Operations.md. Each schema defines the trigger, preconditions, inputs, processing, outputs, postconditions, and safety behavior for one operation.

## Schema Conventions

| Field | Meaning |
|---|---|
| Trigger | Event or condition that starts the operation. |
| Preconditions | Conditions that must be true before execution. |
| Inputs | Data, sensor readings, commands, or records consumed. |
| Processing | Main work performed by the system. |
| Outputs | Decisions, commands, records, or notifications produced. |
| Postconditions | Facts that should be true after successful completion. |
| Failure and safety behavior | Response when the operation cannot complete normally. |

## O1 — Perform Power-On Self-Check

| Schema field | Definition |
|---|---|
| Trigger | Chamber power becomes available. |
| Preconditions | System controller has started; diagnostic interfaces are available. |
| Inputs | Sensor list, actuator list, calibration metadata, and device communication status. |
| Processing | Query every essential sensor and environmental-control device; check communication, plausible readings, and basic actuator response. |
| Outputs | Self-check report containing pass, fail, or unavailable status for each device. |
| Postconditions | A complete diagnostic result is recorded before normal monitoring is considered. |
| Failure and safety behavior | Keep normal conservation disabled and identify failed or unavailable devices. |

## O2 — Validate Essential Device Readiness

| Schema field | Definition |
|---|---|
| Trigger | Self-check report becomes available. |
| Preconditions | O1 has completed and all essential devices have a diagnostic result. |
| Inputs | Self-check report and the list of essential devices. |
| Processing | Confirm that every essential sensor and control device meets its readiness criteria. |
| Outputs | Readiness decision and list of blocking faults, if any. |
| Postconditions | The system is permitted to begin environmental monitoring only when the decision is pass. |
| Failure and safety behavior | Block normal conservation and request maintenance or operator attention. |

## O3 — Initialize Environmental Monitoring

| Schema field | Definition |
|---|---|
| Trigger | Device readiness validation passes. |
| Preconditions | Essential sensors are operational and time synchronization is available. |
| Inputs | Sampling schedule, sensor configuration, chamber identifier, and diagnostic result. |
| Processing | Start scheduled sensor acquisition, timestamping, health tracking, and data storage. |
| Outputs | Monitoring session identifier and first monitoring cycle request. |
| Postconditions | The system can observe the chamber continuously and operate in MONITORING mode. |
| Failure and safety behavior | Keep conservation disabled if monitoring cannot start reliably. |

## O4 — Register Artifact Identity

| Schema field | Definition |
|---|---|
| Trigger | An operator places an artifact inside the chamber. |
| Preconditions | Environmental monitoring is active; artifact identity data is available. |
| Inputs | Artifact ID, name or catalog number, operator ID, placement time, and chamber ID. |
| Processing | Validate the identity format, create or retrieve the artifact record, and link it to the active chamber session. |
| Outputs | Registered artifact record and registration confirmation. |
| Postconditions | The system knows which artifact is inside the chamber. |
| Failure and safety behavior | Do not authorize conservation if the identity is missing, duplicated, or invalid. |

## O5 — Load Required Environmental Profile

| Schema field | Definition |
|---|---|
| Trigger | Artifact registration succeeds. |
| Preconditions | A valid artifact record exists and a conservation profile can be retrieved. |
| Inputs | Required temperature range, humidity range, light limit, vibration limit, recovery periods, and safety thresholds. |
| Processing | Validate profile completeness and units, then load the limits for the active chamber session. |
| Outputs | Active artifact environmental profile and profile-validation result. |
| Postconditions | All required limits are available for compliance decisions. |
| Failure and safety behavior | Keep normal conservation blocked and report the missing or invalid profile. |

## O6 — Authorize Conservation Start

| Schema field | Definition |
|---|---|
| Trigger | Door status or profile availability changes. |
| Preconditions | Monitoring is active, an artifact is registered, and a valid profile is loaded. |
| Inputs | Door status, profile-validation result, sensor readiness, and artifact presence. |
| Processing | Check that the door is closed and all required conditions are satisfied. |
| Outputs | Start authorization or a blocking-reason list. |
| Postconditions | If authorized, normal conservation may begin in CONSERVATION_ACTIVE mode. |
| Failure and safety behavior | Continue monitoring without active conservation while the door is open or requirements are incomplete. |

## O7 — Sample Chamber Conditions

| Schema field | Definition |
|---|---|
| Trigger | Scheduled monitoring cycle or an immediate safety request. |
| Preconditions | Monitoring service is active and sensor channels are configured. |
| Inputs | Temperature, humidity, light, vibration, door, artifact-condition, and power sensors. |
| Processing | Read, validate, timestamp, and store the latest values; identify stale, missing, or implausible readings. |
| Outputs | Current chamber-condition sample and sensor-health indicators. |
| Postconditions | Compliance and safety operations have an up-to-date measurement set. |
| Failure and safety behavior | Mark the affected channel unhealthy and prevent unsafe resumption when a required reading is unavailable. |

## O8 — Evaluate Environmental Compliance

| Schema field | Definition |
|---|---|
| Trigger | A new chamber-condition sample is available. |
| Preconditions | An artifact profile and current sample exist. |
| Inputs | Current temperature, humidity, light, vibration, door status, artifact condition, power status, and permitted limits. |
| Processing | Compare each measurement with its permitted range or threshold and classify the result. |
| Outputs | Compliance decision for each factor: within limit, outside limit, or indeterminate. |
| Postconditions | Any required correction, protection, vibration, door, or power operation can be selected. |
| Failure and safety behavior | Treat indeterminate safety-critical measurements conservatively and block normal operation when needed. |

## O9 — Issue Temperature Correction

| Schema field | Definition |
|---|---|
| Trigger | Temperature is outside the artifact's permitted range. |
| Preconditions | Temperature sensor is trusted, a profile is loaded, and the relevant control mechanism is available. |
| Inputs | Measured temperature, target range, correction limits, actuator status, and recovery period. |
| Processing | Select a bounded temperature-control command and send it to the environmental-control mechanism. |
| Outputs | Command record, target range, command time, and recovery deadline. |
| Postconditions | A temperature-recovery attempt is active; the system has not yet declared recovery. |
| Failure and safety behavior | If control is unavailable or unsafe, escalate toward protection rather than repeatedly issuing unverified commands. |

## O10 — Issue Humidity Correction

| Schema field | Definition |
|---|---|
| Trigger | Humidity is outside the artifact's permitted range. |
| Preconditions | Humidity sensor is trusted, a profile is loaded, and the relevant control mechanism is available. |
| Inputs | Measured humidity, target range, correction limits, actuator status, and recovery period. |
| Processing | Select a bounded humidity-control command and send it to the environmental-control mechanism. |
| Outputs | Command record, target range, command time, and recovery deadline. |
| Postconditions | A humidity-recovery attempt is active; the system has not yet declared recovery. |
| Failure and safety behavior | If control is unavailable or unsafe, escalate toward protection and alert the operator. |

## O11 — Verify Environmental Recovery

| Schema field | Definition |
|---|---|
| Trigger | A temperature or humidity recovery deadline is reached, or a new reading arrives during recovery. |
| Preconditions | A correction command has been issued and fresh sensor readings are available. |
| Inputs | Post-command readings, target range, correction command ID, and recovery deadline. |
| Processing | Compare fresh readings with the permitted range and confirm that the condition is stable enough for the recovery rule. |
| Outputs | Verified-recovery or failed-recovery decision. |
| Postconditions | Only a verified result can clear the environmental exception. |
| Failure and safety behavior | Never infer recovery from the command alone; escalate when the reading remains outside the permitted range or is unavailable. |

## O12 — Escalate Artifact Protection

| Schema field | Definition |
|---|---|
| Trigger | Environmental recovery fails within the allowed recovery period. |
| Preconditions | O11 reports failed recovery or a safety-critical condition requires immediate protection. |
| Inputs | Failed-recovery result, artifact profile, current measurements, and available protection capabilities. |
| Processing | Suspend normal conservation priorities, activate protection workflow, and record the escalation reason. |
| Outputs | Protection-response record and protection actions to execute. |
| Postconditions | The system operates in PROTECTION_MODE and prioritizes artifact safety. |
| Failure and safety behavior | Use the safest available configuration and keep the operator alert active if protection controls are limited. |

## O13 — Apply Additional Protection Controls

| Schema field | Definition |
|---|---|
| Trigger | Protection escalation requests additional controls. |
| Preconditions | Protection response is active and control devices have been assessed. |
| Inputs | Light-control capability, auxiliary environmental controls, current conditions, and artifact limits. |
| Processing | Reduce light exposure and activate appropriate additional environmental controls within safe bounds. |
| Outputs | Actuator commands and updated protection configuration. |
| Postconditions | The chamber is configured to reduce risk to the artifact. |
| Failure and safety behavior | Record unavailable controls and use remaining safeguards without exceeding device limits. |

## O14 — Generate Conservation Alert

| Schema field | Definition |
|---|---|
| Trigger | Protection mode starts, a serious fault occurs, or operator attention is required. |
| Preconditions | Alert transport or local logging is available. |
| Inputs | Alert severity, cause, chamber ID, artifact ID, measurements, timestamps, and recommended action. |
| Processing | Build, persist, and deliver a deduplicated alert to the museum operator. |
| Outputs | Alert record, delivery status, and acknowledgment requirement where configured. |
| Postconditions | The event is traceable and the operator has been informed or delivery failure is recorded. |
| Failure and safety behavior | Persist the alert locally if remote delivery fails and retry without blocking protective actions. |

## O15 — Detect Vibration Hazard

| Schema field | Definition |
|---|---|
| Trigger | A vibration sample exceeds the artifact's permitted threshold. |
| Preconditions | An artifact is registered and the vibration sensor is healthy. |
| Inputs | Vibration measurement, threshold, artifact ID, and current operating activity. |
| Processing | Confirm the threshold crossing, record the event, and suspend activities that could increase risk. |
| Outputs | Vibration-hazard record and suspension command. |
| Postconditions | The system operates in VIBRATION_RESPONSE and risk-increasing activities are paused. |
| Failure and safety behavior | Treat uncertain or repeated high readings conservatively and notify the operator when necessary. |

## O16 — Verify Vibration Stabilization

| Schema field | Definition |
|---|---|
| Trigger | Vibration falls below the permitted threshold. |
| Preconditions | Vibration response is active and the sensor remains healthy. |
| Inputs | Continuous vibration readings, permitted threshold, and required stabilization period. |
| Processing | Confirm that every required reading remains below the threshold for the full stabilization period. |
| Outputs | Stabilization-passed or stabilization-failed decision. |
| Postconditions | Only a passed decision permits evaluation of normal conservation resumption. |
| Failure and safety behavior | Reset the stabilization timer when a reading exceeds the threshold or becomes unreliable. |

## O17 — Handle Door-Open Interruption

| Schema field | Definition |
|---|---|
| Trigger | Door status changes from closed to open while conservation is active. |
| Preconditions | An artifact is inside and the door sensor is available. |
| Inputs | Door event, current activities, artifact ID, and active control commands. |
| Processing | Immediately suspend normal conservation activities and record the interruption. |
| Outputs | Door-interruption record and suspension commands. |
| Postconditions | Normal environmental operation does not continue while the chamber is open. |
| Failure and safety behavior | Treat an uncertain door status as open until verified closed and safe. |

## O18 — Verify Resumption Readiness

| Schema field | Definition |
|---|---|
| Trigger | Door closes after a door-open interruption. |
| Preconditions | The door sensor reports closed and monitoring is available. |
| Inputs | Door status, artifact-condition reading, environmental readings, sensor-health status, and active profile. |
| Processing | Recheck environmental compliance, artifact condition, sensor readiness, and any active protection response. |
| Outputs | Resumption authorization or blocking reasons. |
| Postconditions | Normal conservation resumes only after all required checks pass. |
| Failure and safety behavior | Keep normal operation suspended and escalate if the artifact condition, sensors, or environment are unsafe. |

## O19 — Manage Power Loss

| Schema field | Definition |
|---|---|
| Trigger | Primary power becomes unavailable during chamber operation. |
| Preconditions | Power monitoring is active and emergency-power status can be checked. |
| Inputs | Power-loss event, emergency-power availability, remaining battery or backup capacity, and active operation. |
| Processing | Attempt transfer to emergency power; validate the transfer before continuing any conservation activity. |
| Outputs | Emergency-power activation result or safe-shutdown decision. |
| Postconditions | The chamber either operates on verified emergency power or begins safe shutdown. |
| Failure and safety behavior | Do not continue normal conservation without verified power; prioritize artifact safety and incident recording. |

## O20 — Record Power Incident

| Schema field | Definition |
|---|---|
| Trigger | Primary power loss, emergency-power failure, or safe shutdown. |
| Preconditions | Local incident storage is available or can queue the record. |
| Inputs | Power timestamps, power-source status, active artifact, chamber conditions, and system response. |
| Processing | Create an immutable time-stamped incident record for audit and recovery review. |
| Outputs | Power-incident record and operator-review item. |
| Postconditions | The power event and system response can be reconstructed later. |
| Failure and safety behavior | Use durable local storage or a recovery queue if network storage is unavailable. |

## O21 — Confirm Safe Artifact Removal

| Schema field | Definition |
|---|---|
| Trigger | Operator requests artifact removal. |
| Preconditions | Artifact identity is known, chamber conditions are measurable, and current protection status is available. |
| Inputs | Removal request, door status, environmental compliance, artifact condition, sensor health, and protection-response status. |
| Processing | Confirm safe chamber conditions, no active protection response, and authorization for the operator to remove the artifact. |
| Outputs | Removal-authorized or removal-denied decision with reasons. |
| Postconditions | Removal is allowed only after all safety conditions pass and the decision is recorded. |
| Failure and safety behavior | Deny removal when any required check fails, the chamber is unsafe, or protection response is active. |

## Operation Relationships

| Relationship | Required sequence or rule |
|---|---|
| Startup | O1 → O2 → O3; normal conservation remains blocked until readiness succeeds. |
| Artifact preparation | O4 → O5 → O6; an open door or missing profile prevents authorization. |
| Routine control | O7 → O8; out-of-range readings may invoke O9 or O10. |
| Recovery proof | O9 or O10 → O11; commands alone never prove recovery. |
| Protection escalation | O11 failure → O12 → O13 and O14. |
| Vibration safety | O15 → O16; stabilization must complete before resumption is considered. |
| Door safety | O17 → O18; closing the door alone does not resume conservation. |
| Power safety | O19 → O20; unavailable emergency power leads to safe shutdown and incident recording. |
| Artifact removal | O21 succeeds only when the chamber is safe and no protection response is active. |
