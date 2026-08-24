# Sample Triage Notes

Written the way an analyst would actually document their triage process, referencing Alert 003 and Alert 005 from `sample-alerts.md` together, since they were correlated during triage. All times are given in South African Standard Time (SAST, UTC+2).

## L1 Triage: Alert 003 (Large Outbound Data Transfer)

**Analyst:** SOC-L1-07
**Time:** 2026-08-20 05:45 SAST

Reviewed raw transfer logs. Destination IP 185.203.x.x not present in approved backup or partner destination list. Checked against threat intelligence feed, IP not currently flagged as known malicious, but no positive attribution either.

Transfer occurred at 05:31 SAST, well outside STAYFIT-FS-02's normal backup window (scheduled 03:00 to 03:30 SAST) and outside standard business hours for this department.

Not closing as false positive. No documented legitimate reason for this transfer found in current runbook. Checking for related activity before escalating.

Found Alert 005 (network outage on same segment, timestamp 05:40 SAST, 3 servers affected including STAYFIT-FS-02) logged 9 minutes after this transfer began. Flagging as potentially related rather than treating as a separate infrastructure ticket.

**Decision:** Escalating to L2. Escalation criteria met: business critical asset affected (STAYFIT-FS-02 holds member records, health data, and billing information across all StayFit branches), and pattern does not match known legitimate activity.

## L2 Investigation: Alert 003 and Alert 005 (Correlated)

**Analyst:** SOC-L2-03
**Time:** 2026-08-20 06:02 SAST

Pulled endpoint logs for STAYFIT-FS-02. No EDR alert generated on the host itself in the relevant window, transfer does not appear to originate from process level activity on the server, more consistent with a network level exfiltration path or a compromised account with direct access to the share.

Checked identity logs for any account with access to the relevant file share around the transfer window. One service account, svc-backup01, shows a successful authentication 6 minutes prior to the transfer starting.

This is the same account flagged in Alert 001 (leaked credentials, same day, 05:14 SAST). Confidence that these are related has increased significantly. Credentials leaked at 05:14, account authenticated at 05:25, transfer began at 05:31, network segment lost connectivity at 05:40, most likely as a side effect of load or a deliberate action to disrupt monitoring.

**Escalation criteria met:** confirmed malicious activity indicators (unexplained authentication immediately following a credential leak, followed by unauthorised transfer), privileged service account involved, business critical asset affected, and the data involved falls under POPIA, which raises the business impact beyond the immediate technical scope.

**Decision:** Escalating to L3. Recommending immediate short term containment on svc-backup01 pending L3 review, given the time sensitivity and regulatory exposure.
