# Sample Alerts

These are example alert records written in the format they would appear in a SIEM or ticketing system, used as reference material for the triage notes and the alert prioritisation model elsewhere in this repository. All examples are grounded in a single fictional client, **StayFit**, a South African fitness and wellness chain whose systems hold member personal information, health assessment data, and billing details, the kind of environment where South Africa's Protection of Personal Information Act (POPIA) is directly relevant. Timestamps are given in South African Standard Time (SAST, UTC+2).

## Alert 001: Leaked Credentials Discovered

- **Source:** Threat Intelligence Feed
- **Severity (initial):** Medium
- **Detected:** 2026-08-20 05:14 SAST
- **Summary:** Credentials matching a corporate email address appeared in a verified third party breach dataset.
- **Account:** svc-backup01 (service account, elevated access to StayFit's member records file share)
- **Indicators:** Email and password hash match confirmed against known breach corpus. No confirmed login activity yet.
- **Status:** Open, pending L1 triage

## Alert 002: Multiple Failed Login Attempts

- **Source:** Identity Provider Logs
- **Severity (initial):** Low
- **Detected:** 2026-08-20 05:22 SAST
- **Summary:** 47 failed authentication attempts against a single account within a 6 minute window.
- **Account:** j.dlamini (standard user account, StayFit membership operations team)
- **Indicators:** Source IP not previously associated with this user. No successful login recorded.
- **Status:** Open, pending L1 triage

## Alert 003: Large Outbound Data Transfer

- **Source:** Network Monitoring
- **Severity (initial):** Medium
- **Detected:** 2026-08-20 05:31 SAST
- **Summary:** Approximately 4.2 GB transferred from a file server to an external IP address over a 12 minute window.
- **Asset:** STAYFIT-FS-02 (holds member records, health assessment data, and billing information for all StayFit branches)
- **Indicators:** Destination IP not on approved backup or partner allowlist. Transfer occurred outside normal business hours. Data on this server falls under POPIA, as it includes special personal information (health data) and financial information.
- **Status:** Open, pending L1 triage

## Alert 004: Suspicious PowerShell Execution

- **Source:** Endpoint Detection and Response
- **Severity (initial):** Medium
- **Detected:** 2026-08-20 05:37 SAST
- **Summary:** PowerShell process launched with a base64 encoded command from an unusual parent process.
- **Asset:** STAYFIT-WKS-FIN-014 (finance department workstation, handles StayFit membership billing)
- **Indicators:** Parent process is a document reader application, not a standard administrative tool. Encoded command not yet decoded.
- **Status:** Open, pending L1 triage

## Alert 005: Network Outage

- **Source:** Infrastructure Monitoring
- **Severity (initial):** Low
- **Detected:** 2026-08-20 05:40 SAST
- **Summary:** Loss of connectivity reported across a segment of the internal network, affecting 3 servers including STAYFIT-FS-02.
- **Indicators:** No prior maintenance scheduled. Timing correlates closely with Alert 003.
- **Status:** Open, pending L1 triage

Note that Alert 005 correlates with Alert 003 by timing and by affected asset. This kind of correlation across alerts that would otherwise be ranked separately is exactly what the model in `docs/Alert-Prioritisation.md` is designed to catch. Given that STAYFIT-FS-02 holds POPIA-regulated data, a confirmed breach here carries both a direct security cost and regulatory exposure, POPIA allows for fines of up to R10 million for serious contraventions, which is part of why this correlation gets treated as high priority rather than two unrelated tickets.
