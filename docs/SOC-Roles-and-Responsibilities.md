# SOC Roles and Responsibilities

The alert lifecycle and the incident response process both rely on a team structure that mirrors the escalation path itself. This document sets out what each role is actually responsible for.

## L1 Analyst

First point of contact for incoming alerts. Responsible for initial triage, checking an alert against known and already documented patterns, and making the first call on whether it can be closed immediately or needs to go further. Speed and consistency matter more than depth at this stage, an L1 analyst is not expected to fully investigate every alert, they are expected to correctly identify which ones need someone who can.

## L2 Analyst

Takes alerts that were not closed at L1 and investigates them properly, correlating across log sources, pulling additional evidence, and checking against threat intelligence. Responsible for applying the escalation criteria consistently, an L2 analyst who escalates too readily wastes L3 time on things that were not incidents, and one who escalates too rarely risks missing a genuine one.

## L3 Threat Hunter or Incident Responder

Reviews escalated alerts with full authority to determine scope and formally declare an incident. Once an incident is declared, this is the role that leads containment, eradication and recovery. Also responsible for proactive threat hunting, looking for activity that did not trigger any automated alert in the first place, which is a different skill from responding to what the SIEM has already flagged.

## SOC Manager

Oversight of the whole team's output, not individual alerts. Responsible for quality control across triage decisions, resourcing the team appropriately for alert volume, and making the call on anything that affects the customer relationship directly, a major incident notification, a report that needs context beyond raw numbers, or a pattern of missed detections that needs addressing at a process level rather than a single analyst level.

## Why the Escalation Path Matters as a Team Structure, Not Just a Workflow

The L1 to L3 progression described in `SIEM-SOAR-Workflow.md` is not just a description of what happens to an alert, it is a description of who is qualified to make each decision. An L1 analyst closing an alert that should have been escalated is a training or process gap. An L3 analyst spending their time on alerts that should have been closed at L1 is a resourcing problem. Keeping the technical workflow and the team structure aligned is what makes a SOC scale without either burning out senior staff on routine work or letting genuine incidents sit in a junior analyst's queue too long.
