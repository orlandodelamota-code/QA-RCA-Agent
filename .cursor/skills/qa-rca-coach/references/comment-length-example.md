# Comment length example (QA-3552)

Side-by-side check against the existing coach comment on [AM-4618](https://smartsensebydigi.atlassian.net/browse/AM-4618) (ENREQ-9220 blank white screen). Use this to confirm the new template still works for a first-glance review.

## What changed

| Area | Before | After |
|------|--------|-------|
| Search Criteria | Source, Keywords, Feature Area, Error Type, Platform, Excluded, plus a long "Query narrowed…" paragraph | Source ENREQ; Platform \| Focus; Keywords only |
| Open / Closed count | Top 3 each | Top **2** each (overflow note still allowed) |
| Open ticket fields | Status; Linked Dev Ticket; multi-paragraph Key Context; optional shared-cause flag | Status \| Linked; one-line Note; optional shared-cause flag |
| Closed ticket fields | Resolution; long Fix Description; Fix Quality; multi-bullet Test Coverage Gaps; Recurrence Risk | Resolution \| Quality; one-line Fix; Gaps / Risk on one line |
| Gap summary | Long numbered list | Up to 3 short items |
| Recommendations | Two priorities plus several bullets | Two priorities plus at most one extra |

## New format (populated from AM-4618 facts)

📋 SIMILAR TICKET INVESTIGATION & GAP ANALYSIS

🔍 SEARCH CRITERIA

Source ENREQ: [ENREQ-9220](https://smartsensebydigi.atlassian.net/browse/ENREQ-9220)
Platform: Mobile | Focus: SmartSense app launch — blank white screen on open
Keywords: blank white screen, SmartSense App, iOS, app 1.65.1, SSO

📂 SIMILAR OPEN TICKETS (3 found)

[ENREQ-9073](https://smartsensebydigi.atlassian.net/browse/ENREQ-9073): SmartSense App 1.64.1 Blank white Screen when using SSO to Login: Darden
- Status: In Progress | Linked: AM-4460 (Closed), AM-4479 (Done)
- Note: Same blank-screen / SSO path on 1.64.1; still open while ENREQ-9220 reports it again on 1.65.1.
- ⚠ Shared cause: Hypothesis (Medium) — Needed evidence: on-device log or linked commit showing the same launch-path failure on 1.65.1 and 1.64.1

[AM-4550](https://smartsensebydigi.atlassian.net/browse/AM-4550): Investigate why Company Code 'Darden' fails for SSO on Android (blank "Log In SSO" screen)
- Status: Refine | Linked: AM-4479 (Done)
- Note: Blank SSO login screen for Darden on Android; platform differs from the iOS source ENREQ (❓ Platform Unconfirmed for shared cause).

1 additional tickets found — run manually in Jira for full results

📁 SIMILAR CLOSED TICKETS (3 found)

[AM-4524](https://smartsensebydigi.atlassian.net/browse/AM-4524): App intermittently whitescreens at launch (pre-login) — suspected corrupted persisted session state
- Resolution: Done | Quality: ⚠️ Temporary
- Fix: Launch-path hardening and session clear shipped in 1.65.1 (Bitbucket PR 1073). Hypothesis (Medium): remaining cause is a bad persisted session that still whitescreens — Needed evidence: on-device retest of a corrupted session on 1.65.1
- Gaps / Risk: No recorded on-device retest of a bad persisted session on 1.65.1 | High

[AM-4460](https://smartsensebydigi.atlassian.net/browse/AM-4460): [ENREQ-9073] SmartSense App 1.64.1 Blank white Screen when using SSO to Login
- Resolution: Done | Quality: ⚠️ Temporary
- Fix: Closed after reinstall / app update mitigation; no commit linked on this ticket.
- Gaps / Risk: No test plan or acceptance criteria for the blank screen | High

1 additional tickets found — run manually in Jira for full results

📊 GAP ANALYSIS SUMMARY

Test Coverage Gaps:
1. No recorded iOS cold-open check with an existing session on 1.65.1.
2. QA SSO login path still blocked (AM-2478 Backlog).
3. AM-4524 / AM-4479 recovery UI not verified on a device that still whitescreens.

Fix Quality Overview: 0 root cause, 2 temporary, 0 inconclusive (of tickets shown)
Overall Recurrence Risk: High — blank screen still reported on the build that carried the launch and toast mitigations.

💡 RECOMMENDATIONS

⭐ Priority 1: Add an iOS regression on 1.65.1 for cold open with a persisted session and SSO company-code login; fail if the screen stays blank.
⭐ Priority 2: Finish a durable QA SSO login (AM-2478) so this path can be tested without a customer device.
- On a 1.65.1 device that still whitescreens, record whether the AM-4524 error boundary or AM-4479 toast appears.

## First-glance verdict

Still enough for a reviewer to see: source and platform, the two closest open and closed tickets with status/fix quality, shared-cause risk, top gaps, and two actions. Causal wording uses Verified/Hypothesis labels per [QA-3553](https://smartsensebydigi.atlassian.net/browse/QA-3553) (see [causal-claims-example.md](causal-claims-example.md)). Detail that moved out of the comment (full PR changelog, dropped-ticket list, third ticket in each group) remains available by opening the linked tickets or running a broader Jira search from the overflow note.
