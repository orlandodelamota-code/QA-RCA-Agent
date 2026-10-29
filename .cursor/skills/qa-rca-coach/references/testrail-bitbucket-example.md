# TestRail and Bitbucket search example (QA-3554)

Side-by-side check that Similar Ticket Investigation stays Jira-first while additive TestRail and Bitbucket / code findings land in the short comment. Uses [AM-4524](https://smartsensebydigi.atlassian.net/browse/AM-4524) (blank white screen / launch hydration) facts that already link TestRail cases and a Bitbucket PR.

## What changed

| Area | Before | After |
|------|--------|-------|
| Search sources | Jira + Confluence only | Jira + Confluence, plus TestRail and Bitbucket / code when available |
| Gap Analysis | Inferred coverage gaps from tickets alone | Prefer gaps grounded in retrieved C-ids or missing TestRail Case ID cells |
| Coach starting point | Ticket summaries | Retrieved PR / file paths when linked (for example `App.js`, `ErrorBoundary.js`) |
| Missing tools | N/A | Keep section headers; one limitation line; continue — never block |
| Recent release / merge | Not called out | Timeline from RLM/fixVersions + MERGED PR timestamps; Compass/component when linked; cause claims stay Hypothesis unless cited |

## Populated short comment (additive sections highlighted)

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
- Fix: Launch-path hardening and session clear shipped in 1.65.1 ([Bitbucket PR 1073](https://bitbucket.org/tempalert/smartsense-next/pull-requests/1073)). Hypothesis (Medium): remaining cause is a bad persisted session that still whitescreens — Needed evidence: on-device retest of a corrupted session on 1.65.1
- Gaps / Risk: No recorded on-device retest of a bad persisted session on 1.65.1 | High

[AM-4460](https://smartsensebydigi.atlassian.net/browse/AM-4460): [ENREQ-9073] SmartSense App 1.64.1 Blank white Screen when using SSO to Login
- Resolution: Done | Quality: ⚠️ Temporary
- Fix: Closed after reinstall / app update mitigation; no commit linked on this ticket.
- Gaps / Risk: No test plan or acceptance criteria for the blank screen | High

🧪 RELATED TESTRAIL (2 found)

[C65333](https://smartsensetest.testrail.io/index.php?/cases/view/65333): Legacy SAML SSO login
- Match: refs/link on AM-4524 | Coverage: SSO login path covered; no C-id filled for AC1 corrupted-snapshot recovery on the same ticket

[C3227152](https://smartsensetest.testrail.io/index.php?/cases/view/3227152): Foreground session expiry (Legacy SSO)
- Match: refs/link on AM-4524 | Coverage: expiry toast / logout path; does not assert cold-open after a corrupt AsyncStorage session

🔧 RELATED CODE / BITBUCKET (2 found)

[PR 1073](https://bitbucket.org/tempalert/smartsense-next/pull-requests/1073): AM-4524 Guard launch hydration and harden legacy SSO token handoff
- Repo: tempalert/smartsense-next | Tied to: AM-4524 | Paths: App.js, app/components/ErrorBoundary.js, app/utils/StatePersist.js | Merge: MERGED ~2026-10-13 (Graph lastUpdatedAt)

[AM-4524 commits via Graph](https://smartsensebydigi.atlassian.net/browse/AM-4524): linked ExternalCommit set (e.g. 00de285)
- Repo: smartsense-next | Tied to: AM-4524 | Paths: not listed on commit objects | Merge: commits linked on the closed bug

⏱ RECENT RELEASE / MERGE TIMING (2 found)

- Release: [RLM-915](https://smartsensebydigi.atlassian.net/browse/RLM-915) — Deployed (SSM App v1.65.1); Fix version SSM App v1.65.1 on AM-4524 | Reported on: app 1.65.1 / related ENREQ blank-screen reports
- Merge window: [PR 1073](https://bitbucket.org/tempalert/smartsense-next/pull-requests/1073) — MERGED ~2026-10-13, aligned with RLM-915 window
- Hypothesis (Medium) — Needed evidence: customer repro confirming whitescreen still occurs on 1.65.1 *after* PR 1073 (mitigation) vs whether an earlier 1.64.x/1.65.0 change introduced the corrupt-session path

📊 GAP ANALYSIS SUMMARY

Test Coverage Gaps:
1. AM-4524 left TestRail Case ID empty for AC1 (corrupted session → login, no whitescreen) despite linking C65333 / C3227152 for SSO paths.
2. No recorded iOS cold-open check with a bad persisted session on 1.65.1.
3. QA SSO login path still blocked (AM-2478 Backlog).

Fix Quality Overview: 0 root cause, 2 temporary, 0 inconclusive
Overall Recurrence Risk: High — blank screen still reported on the build that carried the launch and toast mitigations.

💡 RECOMMENDATIONS

⭐ Priority 1: Add or fill a TestRail case for corrupted AsyncStorage cold-open on iOS 1.65.1 (AM-4524 AC1 gap); fail if the screen stays blank.
⭐ Priority 2: Start code review at [PR 1073](https://bitbucket.org/tempalert/smartsense-next/pull-requests/1073) (`App.js` hydration try/catch, `ErrorBoundary.js`) before expanding the 5 Whys.

**Check:** TestRail and Bitbucket rows only cite retrieved links from AM-4524. Jira similar-ticket search is unchanged. Unavailable-tool runs would keep the two headers with a single limitation line instead of fabricated C-ids or PRs.

## During the 5 Whys (interactive)

Same AM-4524 evidence can deepen a Why without re-running the full investigation. Example turn after Why #2 ("hydration fails on a corrupt AsyncStorage snapshot"):

```
Why #2: Hydration fails on a corrupt AsyncStorage snapshot and never reaches login.
- Tag: Hypothesis (Medium)
- Needed evidence (before dig): TestRail case or unit test that asserts corrupt-session cold open; code path that catches loadAppState failure

Evidence pulled this turn:
- TestRail: C65333, C3227152 — SSO login / expiry only; AC1 corrupt-snapshot cell on AM-4524 still empty
- Bitbucket: PR 1073 — App.js try/catch around loadAppState; ErrorBoundary.js; StatePersist.js

Why #2 (updated): Hydration fails on a corrupt AsyncStorage snapshot and never reaches login.
- Tag: Verified — Evidence: PR 1073 (App.js hydration guard) + AM-4524 AC1 with empty TestRail Case ID
- Next Why suggestion: Why was AC1 left without a TestRail case while SSO paths were linked?
```

Root-cause table later cites those same links in the evidence column. CAPA can name "add C-id for AC1" and "regression around App.js hydration" without inventing coverage.

## Unavailable-source shape (still valid)

🧪 RELATED TESTRAIL (unavailable)

TestRail not connected — continued with Jira/Confluence only

🔧 RELATED CODE / BITBUCKET (unavailable)

Bitbucket / code search unavailable — continued with linked PRs/commits only

⏱ RECENT RELEASE / MERGE TIMING (unavailable)

Release / merge timing unavailable — continued with Jira/Confluence only

(If no PR/commit URLs were on the tickets either, use: `Bitbucket / code search unavailable — continued with Jira/Confluence only` or `None found`.)
