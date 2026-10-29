# Causal claims example (QA-3553)

Side-by-side check that every causal / root-cause statement is labeled **Verified** (with evidence link) or **Hypothesis** (with confidence / Needed evidence). Uses one standard mobile bug and one analytics / data-style ENREQ.

## What changed

| Area | Before | After |
|------|--------|-------|
| Operating rules | Soft "treat unsupported as hypotheses" | Hard Verified vs Hypothesis on every causal claim |
| Shared-cause flag | Unlabeled `Shared root cause still open` | `Shared cause: Verified — link` or `Hypothesis (confidence) — Needed evidence` |
| ✅ Root cause quality | Allowed without a citation | Requires `| Evidence: [retrieved link]`; otherwise use ❓ Inconclusive |
| Notes / Fix / risk rationale | Could imply a cause as fact | Causal wording must be tagged; similarity-only notes stay unlabeled |
| Analytics / data pipelines | Inferred joins / schema causes could read as grounded | Default to Hypothesis unless logs/comments/Confluence were retrieved |

## Case A — standard bug (ENREQ-9220 / AM-4618 blank white screen)

Observational similarity stays unlabeled. Shared cause and temporary-fix remaining cause are tagged.

📋 SIMILAR TICKET INVESTIGATION & GAP ANALYSIS

🔍 SEARCH CRITERIA

Source ENREQ: [ENREQ-9220](https://smartsensebydigi.atlassian.net/browse/ENREQ-9220)
Platform: Mobile | Focus: SmartSense app launch — blank white screen on open
Keywords: blank white screen, SmartSense App, iOS, app 1.65.1, SSO

📂 SIMILAR OPEN TICKETS (2 found)

[ENREQ-9073](https://smartsensebydigi.atlassian.net/browse/ENREQ-9073): SmartSense App 1.64.1 Blank white Screen when using SSO to Login: Darden
- Status: In Progress | Linked: AM-4460 (Closed), AM-4479 (Done)
- Note: Same blank-screen / SSO path on 1.64.1; still open while ENREQ-9220 reports it again on 1.65.1.
- ⚠ Shared cause: Hypothesis (Medium) — Needed evidence: on-device log or linked commit showing the same launch-path failure on 1.65.1 and 1.64.1

[AM-4550](https://smartsensebydigi.atlassian.net/browse/AM-4550): Investigate why Company Code 'Darden' fails for SSO on Android (blank "Log In SSO" screen)
- Status: Refine | Linked: AM-4479 (Done)
- Note: Blank SSO login screen for Darden on Android; platform differs from the iOS source ENREQ (❓ Platform Unconfirmed for shared cause).

📁 SIMILAR CLOSED TICKETS (2 found)

[AM-4524](https://smartsensebydigi.atlassian.net/browse/AM-4524): App intermittently whitescreens at launch (pre-login) — suspected corrupted persisted session state
- Resolution: Done | Quality: ⚠️ Temporary
- Fix: Launch-path hardening and session clear shipped in 1.65.1 (Bitbucket PR 1073). Hypothesis (Medium): remaining cause is a bad persisted session that still whitescreens — Needed evidence: on-device retest of a corrupted session on 1.65.1
- Gaps / Risk: No recorded on-device retest of a bad persisted session on 1.65.1 | High

[AM-4460](https://smartsensebydigi.atlassian.net/browse/AM-4460): [ENREQ-9073] SmartSense App 1.64.1 Blank white Screen when using SSO to Login
- Resolution: Done | Quality: ⚠️ Temporary
- Fix: Closed after reinstall / app update mitigation; no commit linked on this ticket.
- Gaps / Risk: No test plan or acceptance criteria for the blank screen | High

📊 GAP ANALYSIS SUMMARY

Test Coverage Gaps:
1. No recorded iOS cold-open check with an existing session on 1.65.1.
2. QA SSO login path still blocked (AM-2478 Backlog).
3. AM-4524 / AM-4479 recovery UI not verified on a device that still whitescreens.

Fix Quality Overview: 0 root cause, 2 temporary, 0 inconclusive
Overall Recurrence Risk: High — blank screen still reported on the build that carried the launch and toast mitigations.

💡 RECOMMENDATIONS

⭐ Priority 1: Add an iOS regression test on 1.65.1 for cold open with a persisted session and SSO company-code login; fail if the screen stays blank.
⭐ Priority 2: Finish a durable QA SSO login (AM-2478) so this path can be tested without a customer device.

**Case A check:** Notes stay observational. Shared cause is Hypothesis with Needed evidence (no retrieved log proving same root). Temporary Fix names a remaining cause only as Hypothesis. No ✅ without Evidence.

## Case B — analytics / data-style (ENREQ-9218 data did not backfill)

Complex data gaps invite uncited pipeline inferences. Those must be Hypothesis unless a retrieved comment or ticket states the cause.

📋 SIMILAR TICKET INVESTIGATION & GAP ANALYSIS

🔍 SEARCH CRITERIA

Source ENREQ: [ENREQ-9218](https://smartsensebydigi.atlassian.net/browse/ENREQ-9218)
Platform: Unconfirmed | Focus: sensor reading backfill — missing readings after gateway restore
Keywords: data did not backfill, missing readings, gateway, firmware 1.96, Tribe Clinical

📂 SIMILAR OPEN TICKETS (1 found)

None found

📁 SIMILAR CLOSED TICKETS (2 found)

[ENREQ-9053](https://smartsensebydigi.atlassian.net/browse/ENREQ-9053): Data Did not Backfill: Alcon
- Resolution: Done | Quality: ❓ Inconclusive
- Fix: Closed as done; no commit/PR linked on the ticket in this check.
- Gaps / Risk: No documented recovery verification or root-cause write-up on the ticket | Medium

[ENREQ-8985](https://smartsensebydigi.atlassian.net/browse/ENREQ-8985): Jolt: DLHT65N Sensors sporadically missing readings
- Resolution: Done | Quality: ❓ Inconclusive
- Fix: Ticket closed; retrieved fields did not include a linked commit describing a systemic fix.
- Gaps / Risk: Sporadic missing-readings pattern may recur without a cited durable fix | Medium

📊 GAP ANALYSIS SUMMARY

Test Coverage Gaps:
1. No recorded post-restore backfill check for firmware 1.96 after a 1+ hour gateway outage.
2. Gap table on ENREQ-9218 (14 missed readings) is not paired with a gateway/sensor log citation in similar closed tickets.
3. Closed backfill ENREQs lack linked Evidence for ✅ root-cause classification.

Fix Quality Overview: 0 root cause, 0 temporary, 2 inconclusive
Overall Recurrence Risk: Medium — Hypothesis (Medium): backfill after gateway restore may still drop readings — Needed evidence: gateway/sensor upload logs or a closed ticket with Evidence that a systemic backfill fix shipped

💡 RECOMMENDATIONS

⭐ Priority 1: On ENREQ-9218, pull gateway and sensor logs for 2026-10-09 16:31–17:46 (device 11666000000233489968) before naming a pipeline cause.
⭐ Priority 2: Add a regression test that power-cycles a gateway on firmware 1.96 and asserts historical readings backfill within the expected window.

**Case B check:** No Verified pipeline/join/schema cause without retrieved logs. Closed tickets stay ❓ when Evidence is missing (not forced to ✅). Recurrence rationale that states a cause is tagged Hypothesis with Needed evidence.

## Verified evidence sample (DASRV-1747 compliance report "null")

When a retrieved comment or PR states the cause, ✅ is allowed:

[DASRV-1747](https://smartsensebydigi.atlassian.net/browse/DASRV-1747): Reports showing "null" instead of temperatures in compliance report
- Resolution: Done | Quality: ✅ Root cause | Evidence: [DASRV-1747 comment 2024-03-28](https://smartsensebydigi.atlassian.net/browse/DASRV-1747) (F→C preference switch produced two day rows; unit column removed from Snowflake source)
- Fix: Removed unused reading measure unit column from the compliance Snowflake source so a unit change no longer duplicates the day row.
- Gaps / Risk: None found | Low

This is Verified because the ticket comment documents the cause and the Development field shows merged Bitbucket PRs. Without that retrieved citation, the same ticket would stay ❓ Inconclusive — not ✅.

## First-glance verdict

Both cases stay within the short QA-3552 field budget while making every causal claim either Verified+linked or Hypothesis+confidence/Needed evidence. A reviewer can see which statements are grounded and what would confirm the rest.
