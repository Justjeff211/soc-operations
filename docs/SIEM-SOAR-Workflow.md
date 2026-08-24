# SIEM and SOAR Alert to Resolution Workflow

This document covers the complete path an alert takes, from the log source that generated the underlying data through to resolution and the feedback loop back into detection tuning.

## Log Sources

Detection is only as good as the data feeding it. This workflow assumes six categories of log source:

- **Endpoint telemetry**, process execution, file activity, and behavioural data from EDR agents
- **Firewall logs**, allowed and blocked connections, port and protocol activity
- **Identity logs**, authentication attempts, privilege changes, conditional access decisions
- **Cloud logs**, activity across cloud platforms, resource changes, access to storage and workloads
- **Network monitoring data**, traffic patterns, DNS queries, unusual volumes or destinations
- **Threat intelligence feeds**, known indicators of compromise, malicious IPs, domains, and file hashes

## Log Ingestion

Raw log data is not directly usable for detection. It passes through four stages before it reaches a correlation engine:

1. **Collection agents** pull or receive logs from each source
2. **Parsing** breaks raw log entries into structured fields
3. **Normalisation** maps those fields into a consistent schema so that, for example, a source IP field means the same thing whether it came from a firewall or a cloud platform
4. **Enrichment** adds context, resolving an IP to a geolocation, matching a hash against threat intelligence, or tagging an asset with its business criticality

## Detection

Once normalised and enriched, log data is evaluated by four detection methods, any of which can independently generate an alert:

- **Correlation rules**, pattern matches across multiple log sources within a defined time window
- **Analytics**, statistical or volume based anomalies
- **Behaviour based detection**, deviations from an established baseline for a user or system
- **Threat intelligence matching**, direct hits against known malicious indicators

## Alert Lifecycle

```mermaid
%% SIEM and SOAR Alert to Resolution Workflow
%% Traces the full path from raw log source through detection, triage, escalation and resolution, with a feedback loop back into detection tuning

flowchart TD

    subgraph LS["Log Sources"]
        direction TB
        LS1["Endpoint Telemetry"]
        LS2["Firewall Logs"]
        LS3["Identity Logs"]
        LS4["Cloud Logs"]
        LS5["Network Monitoring Data"]
        LS6["Threat Intelligence Feeds"]
    end

    subgraph ING["Log Ingestion"]
        direction TB
        I1["Collection Agents"]
        I2["Parsing"]
        I3["Normalisation"]
        I4["Enrichment"]
        I1 --> I2 --> I3 --> I4
    end

    subgraph DET["Detection"]
        direction TB
        D1["Correlation Rules"]
        D2["Analytics"]
        D3["Behaviour Based Detection"]
        D4["Threat Intelligence Matching"]
    end

    LS1 --> I1
    LS2 --> I1
    LS3 --> I1
    LS4 --> I1
    LS5 --> I1
    LS6 --> I4

    I4 --> D1
    I4 --> D2
    I4 --> D3
    I4 --> D4

    D1 --> AC["Alert Created"]
    D2 --> AC
    D3 --> AC
    D4 --> AC

    AC --> T1["L1 Triage: review alert context, confirm log integrity, check known false positive patterns"]
    T1 --> Q1{"Sufficient evidence to close as false positive?"}
    Q1 -->|"Yes, documented"| CLOSE1["Close Alert, Log Justification"]
    Q1 -->|"No"| T2["L2 Investigation: correlate related events, pull endpoint and network evidence, check threat intel context"]

    T2 --> Q2{"Escalation criteria met: confirmed malicious activity, privileged account involved, or business critical asset affected?"}
    Q2 -->|"No"| CLOSE2["Close Alert, Update Detection Logic if Needed"]
    Q2 -->|"Yes"| T3["L3 Escalation: senior analyst or threat hunter reviews scope"]

    T3 --> DEC{"Incident Declared?"}
    DEC -->|"No, contained to noise or misconfiguration"| CLOSE3["Close, Document Root Cause"]
    DEC -->|"Yes"| CONT["Containment: isolate host, disable account, block indicator"]

    CONT --> ERAD["Eradication: remove malware, close vulnerability, rotate credentials"]
    ERAD --> REC["Recovery: restore service, monitor for recurrence, validate clean state"]
    REC --> LL["Lessons Learned: root cause review, update playbooks, tune detections"]

    LL -.Feedback Loop.-> DET
    CLOSE1 -.Feedback Loop.-> DET
    CLOSE2 -.Feedback Loop.-> DET
    CLOSE3 -.Feedback Loop.-> DET
```

### L1 Triage

The analyst reviews the alert in context: what triggered it, what the raw evidence looks like, and whether it matches a known and already documented false positive pattern. The required evidence at this stage is the alert itself, the raw log entries that generated it, and any existing documentation for that alert type. The decision point is whether there is enough evidence to close the alert immediately with a documented justification, or whether it needs deeper investigation.

### L2 Investigation

If the alert is not closed at L1, an L2 analyst correlates it against related events across other log sources, pulls additional endpoint or network evidence, and checks it against threat intelligence context. The escalation criteria at this stage are specific: confirmed malicious activity, a privileged account involved, or a business critical asset affected. If none of those apply, the alert is closed and, where relevant, the detection logic is updated to reduce future noise. If any of them apply, it goes to L3.

### L3 Escalation and Incident Declaration

A senior analyst or threat hunter reviews the full scope of the activity and makes the formal call on whether this is an incident. If it is contained to noise, misconfiguration, or an already known and accepted risk, it is closed with the root cause documented. If it is confirmed, it becomes a formal incident and moves into containment.

### Containment, Eradication and Recovery

These stages are covered in detail in `Incident-Response-Lifecycle.md`. In this workflow they represent the point where the alert has fully transitioned from a detection problem into an incident response problem.

### Lessons Learned and the Tuning Feedback Loop

Every path through this workflow, whether an alert is closed at L1, closed at L2, closed without a declared incident at L3, or resolved through the full incident response process, feeds back into detection. Closed alerts that follow a repeatable pattern indicate a detection rule that needs tuning to reduce noise. Escalated alerts that turn out not to be incidents indicate the same thing from the other direction. Confirmed incidents indicate a gap that needs a new or improved detection rule so the same activity is caught faster next time. This feedback loop is what stops a SOC's detection coverage from staying static while the threat landscape moves on.

The raw source for this diagram is available at `architecture/siem-soar-workflow.mmd`.
