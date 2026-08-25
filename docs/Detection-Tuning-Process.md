# Detection Tuning Process

Every closed alert in this workflow is meant to feed back into detection. This document covers what that actually means in practice, rather than treating it as a vague final step.

## Why Tuning Is Not Optional

A detection rule that generates constant false positives does not get ignored safely, it gets ignored dangerously. Analysts under alert fatigue start closing things faster and with less scrutiny, and the genuine positive eventually gets treated the same way as the ninety nine false ones before it. Tuning exists to keep the signal to noise ratio high enough that analysts can actually apply proper judgement to every alert rather than developing a habit of dismissal.

## The Process

1. **Review closed alerts on a regular cadence.** Not individually as they close, but in aggregate, weekly or biweekly depending on volume, looking for patterns rather than one off cases.
2. **Identify repeat patterns in false positives.** If the same rule is generating the same type of false positive repeatedly, that is a tuning candidate, not something to keep manually closing indefinitely.
3. **Identify near misses.** Alerts that were closed but, on review, were closer to a genuine concern than the closing analyst realised at the time. These indicate a gap in the escalation criteria or the documentation an L1 analyst was working from, not necessarily a bad decision.
4. **Adjust the rule.** This might mean changing a threshold, adding an exclusion for a known legitimate pattern, or adding additional context requirements before the rule fires at all.
5. **Validate the change before it goes live everywhere.** Where possible, test the adjusted rule against historical data or in a staging environment to confirm it still catches genuine activity while reducing the noise it was built to address.
6. **Document the change and why it was made.** Future review of this rule, and any incident investigation that touches it, needs to know what changed and when, not just what the rule currently does.

## Where Tuning Requests Actually Come From

In this workflow, tuning is not a separate task someone remembers to do occasionally, it is generated directly by the alert lifecycle itself. Every alert closed at L1 or L2 is a potential tuning candidate. Every alert escalated to L3 that turns out not to be an incident is a potential tuning candidate from the opposite direction. Every confirmed incident that was not caught by an existing detection is a gap that needs a new rule, not just a lesson to remember informally. Treating detection tuning as a direct output of the alert lifecycle, rather than a separate periodic exercise, is what keeps detection coverage aligned with how the environment and the threat landscape are actually changing.
