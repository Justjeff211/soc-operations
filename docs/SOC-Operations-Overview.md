# SOC Operations Overview

This document is the starting point for the rest of the repository. It explains how the different parts of a SOC operation connect, before going into the detail of each one in its own file.

A SOC operation exists to answer one question repeatedly and reliably: does this piece of data represent a threat, and if it does, what needs to happen next. Everything in this repository is structured around that question.

The workflow begins before an alert ever exists, with raw log data arriving from endpoints, firewalls, identity systems, cloud platforms, network monitoring tools and threat intelligence feeds. That data has to be collected, parsed into a usable format, normalised so that different log formats can be compared against each other, and enriched with additional context before it is useful for detection. This part of the process is covered in `SIEM-SOAR-Workflow.md`.

Once an alert is generated, it enters the human part of the process, triage, investigation, and where necessary, escalation. This is where the majority of analyst time is actually spent, and where inconsistent decision making causes the most damage, either by missing a real incident buried in noise, or by escalating things that did not need it and burning analyst time that should have gone somewhere else.

If an alert is confirmed as an incident, it moves into the formal incident response lifecycle covered in `Incident-Response-Lifecycle.md`, following containment, eradication, recovery and lessons learned. The lessons learned stage is not a formality here, it directly feeds back into how detections are tuned, which is covered separately in `Detection-Tuning-Process.md`.

Running underneath all of this is the question of prioritisation, covered in `Alert-Prioritisation.md`. A SOC rarely deals with one alert at a time, and the order analysts choose to work through alerts in matters as much as how they handle each one individually.

Finally, `MDR-MXDR-Service-Model.md` and `SOC-Roles-and-Responsibilities.md` step back from the workflow itself and cover how this operates as a service, who is responsible for which part of it, and how a managed security provider structures a SOC to deliver this to a customer rather than running it purely in house.

## How the Documents Connect

| Document | Covers |
|---|---|
| `SIEM-SOAR-Workflow.md` | Log sources through detection through alert triage and escalation |
| `Incident-Response-Lifecycle.md` | What happens once an alert is confirmed as an incident |
| `Alert-Prioritisation.md` | How to rank multiple simultaneous alerts using consistent criteria |
| `Detection-Tuning-Process.md` | How closed alerts and confirmed incidents feed back into better detection |
| `MDR-MXDR-Service-Model.md` | How this operates as a service delivered to a customer |
| `SOC-Roles-and-Responsibilities.md` | Who is responsible for which decision in the workflow |
