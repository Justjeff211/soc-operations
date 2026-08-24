# SOC Operations: Designing a Real World Security Operations Workflow

## Scenario Context

To keep the workflow grounded rather than abstract, the examples in `examples/` are built around a single fictional client: **StayFit**, a South African fitness and wellness chain. StayFit holds member personal information, health assessment data, and billing details, which puts it squarely under South Africa's Protection of Personal Information Act (POPIA), the same way a real gym operator's systems would be. Timestamps throughout the examples are given in South African Standard Time (SAST, UTC+2), and any financial figures are in South African Rand (ZAR).

## 1. Project Overview

I wanted to properly understand how a Security Operations Centre actually works end to end, not just how individual tools like a SIEM or an EDR agent function in isolation. Most learning resources cover detection engineering or incident response as separate topics, but in a real SOC they are one continuous process: a log source generates data, that data becomes an alert, an analyst has to decide what that alert means, and the outcome of that decision eventually feeds back into how future alerts are detected in the first place. This project maps that entire loop, from raw telemetry through to lessons learned, using the kind of workflow documentation an analyst would actually produce and reference on the job.

## 2. SOC Operations Concept

A SOC without structure does not scale. On a quiet day an analyst can rely on instinct and still get the right answer, but SOCs rarely have quiet days. Alert volume is constant, shifts hand over mid investigation, and a decision made at 3am needs to be defensible when someone reviews it the next morning. Structured workflows exist to solve three problems at once: consistency, the same type of alert gets triaged the same way regardless of who is on shift, auditability, every escalation or closure has a documented reason, and improvement, patterns in what gets closed or escalated should change how detections are tuned over time. This project treats those three problems as the actual design brief rather than an afterthought.

## 3. Architecture

The repository is split into three parts. The `docs/` folder contains the written explanation of each part of the SOC operation, `architecture/` contains the raw Mermaid source for each diagram so they can be reused or edited independently, and `examples/` contains sample artefacts, alert tickets, triage notes and an incident report template, written the way they would actually look on the job rather than as abstract placeholders. Each diagram is also embedded directly inside its related documentation file so it renders on GitHub without needing to open a separate file.

## 4. Workflow Explanation

The core of the project is the SIEM and SOAR alert to resolution workflow in `docs/SIEM-SOAR-Workflow.md`. It starts with six categories of log source (endpoint, firewall, identity, cloud, network monitoring and threat intelligence feeds), moves through collection, parsing, normalisation and enrichment, and then into four detection methods that can independently generate an alert. From there the alert moves through L1 triage, L2 investigation and, where the escalation criteria are met, L3 review and formal incident declaration. Every stage that involves a human decision includes what evidence that decision should be based on, not just the decision itself.

## 5. Incident Handling Process

Once an incident is declared, the project follows the standard preparation, detection and analysis, containment, eradication, recovery, and lessons learned lifecycle, documented in `docs/Incident-Response-Lifecycle.md`. Containment is split into short term and long term actions because those are genuinely different decisions in practice, isolating a host immediately is not the same as deciding how to permanently remediate the access path that let the compromise happen.

## 6. Alert Prioritisation Model

`docs/Alert-Prioritisation.md` covers the part of SOC work that is hardest to get right under pressure, deciding what to look at first when several things go wrong at once. The model deliberately avoids ranking alerts by how alarming they sound. A large outbound data transfer sounds worse than a burst of failed logins, but if the failed logins are hitting a domain admin account and the data transfer is going to an approved backup location, the ranking should flip. The decision tree in `architecture/alert-decision-tree.mmd` works through confidence level, active exploitation indicators, account privilege, asset criticality and data sensitivity in that order, and the documentation walks through five example alerts, leaked credentials, failed logins, a large outbound transfer, suspicious PowerShell execution, and a network outage, showing how each one is actually ranked and why.

## 7. Technologies and Concepts

SIEM and SOAR platform concepts (log correlation, playbooks, automated response actions), EDR and endpoint telemetry, threat intelligence feeds and matching, the NIST and SANS incident response lifecycles, MITRE ATT&CK as a reference point for attack indicators, and the operational structure of managed detection and response (MDR/MXDR) services as delivered by managed security providers.

## 8. Skills Demonstrated

Designing SOC workflows that hold up under real alert volume, applying a structured incident response methodology rather than an improvised one, building a defensible alert prioritisation model instead of relying on instinct, writing SOC documentation the way an analyst actually would (triage notes, incident reports, escalation criteria), and using Mermaid to produce clean, GitHub native architecture and workflow diagrams.

## 9. Future Improvements

Planned additions include mapping each alert type in the prioritisation model to specific MITRE ATT&CK techniques, adding a sample Microsoft Sentinel KQL detection rule alongside the correlation logic it is based on, and expanding the SOC roles documentation to cover shift handover procedure, which is one of the more overlooked failure points in real SOC operations.

## Repository Structure

```
SOC-Operations/
├── README.md
├── docs/
│   ├── SOC-Operations-Overview.md
│   ├── SIEM-SOAR-Workflow.md
│   ├── MDR-MXDR-Service-Model.md
│   ├── Incident-Response-Lifecycle.md
│   ├── Alert-Prioritisation.md
│   ├── SOC-Roles-and-Responsibilities.md
│   └── Detection-Tuning-Process.md
├── architecture/
│   ├── siem-soar-workflow.mmd
│   ├── mdr-service-model.mmd
│   ├── incident-response.mmd
│   └── alert-decision-tree.mmd
└── examples/
    ├── sample-alerts.md
    ├── triage-notes.md
    └── incident-report-template.md
```
