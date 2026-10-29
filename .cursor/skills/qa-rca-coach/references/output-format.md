# Similar Ticket Investigation: Required Output Format

Use this template exactly. Replace bracketed items. Keep every section header; write "None found" for empty sections. Do not add prose before or after.

Keep the comment short enough for a first-glance Jira read. Prefer one short line per field. Do not paste long comment quotes, PR changelogs, or query-narrowing narratives into Search Criteria or ticket notes.

**Hard reject before posting:** if the draft still has any of `Feature Area:`, `Error Type:` (separate line), `Excluded from results:`, `Key Context:`, `Linked Dev Ticket:`, `Fix Description:`, per-ticket `Test Coverage Gaps:` bullets, per-ticket `Recurrence Risk:`, more than **2** tickets under Open or under Closed, more than **2** Related TestRail items, more than **2** Related Code / Bitbucket items, a "Query narrowed" / "Dropped as" paragraph, an unlabeled `Shared root cause still open` line, a `Quality: ✅` without `| Evidence:`, any causal / root-cause sentence that is not tagged **Verified** or **Hypothesis**, or any TestRail case / Bitbucket PR / file path that was not retrieved — rewrite to this template. Do not post the long or uncited draft.

### Causal claims (QA-3553)

Every causal or root-cause statement in this comment must be labeled:

- **Verified** — include at least one evidence link to a source you actually retrieved (Jira ticket, Confluence page, commit/PR, or documented log/reference)
- **Hypothesis** — include confidence **High** / **Medium** / **Low** and/or `Needed evidence: …`

Observational similarity ("same symptom", "same feature path", "reported again on build X") is not a causal claim and needs no label. Prefer Hypothesis when logs, dataset joins, or fix evidence were not retrieved — especially on analytics / data-pipeline ENREQs. Never present an inferred cause as fact.

---

📋 SIMILAR TICKET INVESTIGATION & GAP ANALYSIS

🔍 SEARCH CRITERIA

Source ENREQ: [ENREQ-XXXX](https://smartsensebydigi.atlassian.net/browse/ENREQ-XXXX)
Platform: [Mobile / Web / Unconfirmed] | Focus: [feature area] — [error type]
Keywords: [3–6 short terms from the ENREQ]

📂 SIMILAR OPEN TICKETS (X found)

[TICKET-KEY](https://smartsensebydigi.atlassian.net/browse/TICKET-KEY): [Summary]
- Status: [status] | Linked: [PROD-YYYY (status)] or None
- Note: [one short sentence on why it is related — similarity only, or Verified/Hypothesis if stating a cause]
- ⚠ Shared cause: Verified — [evidence link] | still open
  (or: ⚠ Shared cause: Hypothesis (High|Medium|Low) — Needed evidence: [what]; omit the flag when it does not apply)

📁 SIMILAR CLOSED TICKETS (X found)

[TICKET-KEY](https://smartsensebydigi.atlassian.net/browse/TICKET-KEY): [Summary]
- Resolution: [Fixed / Won't Fix / etc.] | Quality: [✅ Root cause | Evidence: link] / [⚠️ Temporary] / [❓ Inconclusive]
- Fix: [one short sentence on what was done; if naming a remaining cause, tag Hypothesis (confidence) — Needed evidence: …]
- Gaps / Risk: [one short gap or "None found"] | [High / Medium / Low]

(Add "X additional tickets found ..." note under each group when results were truncated. Show at most **2** tickets per group.)

🧪 RELATED TESTRAIL (X found | unavailable | none)

[C#####](https://smartsensetest.testrail.io/index.php?/cases/view/#####): [case title or short label]
- Match: [keyword / refs KEY] | Coverage: [one short note — covers path / gap / regression candidate]

(Or a single line: `None found` / `TestRail not connected — continued with Jira/Confluence only` / `TestRail search failed — continued with Jira/Confluence only`. Show at most **2** cases. Prefer links retrieved from the ENREQ or similar tickets; never invent C-ids.)

🔧 RELATED CODE / BITBUCKET (X found | unavailable | none)

[PR or commit link]: [one-line title]
- Repo: [workspace/repo] | Tied to: [ticket key] | Paths: [up to 3 retrieved file paths, or "not listed"]

(Or a single line: `None found` / `Bitbucket / code search unavailable — continued with linked PRs/commits only` / `Bitbucket / code search unavailable — continued with Jira/Confluence only`. Show at most **2** PRs or commits. Never invent paths or PR numbers.)

📊 GAP ANALYSIS SUMMARY

Test Coverage Gaps: [up to 3 short numbered items; cite TestRail C-ids when retrieved]
Fix Quality Overview: X root cause, Y temporary, Z inconclusive
Overall Recurrence Risk: [High / Medium / Low] — [one short rationale; tag Verified/Hypothesis only if the rationale states a cause]

💡 RECOMMENDATIONS

⭐ Priority 1: [highest priority recommendation]
⭐ Priority 2: [second priority recommendation]
- [One additional recommendation, if needed]

(Recommendations cover: new test cases to write with suggested scope; existing test cases to strengthen or extend; process improvements such as checklist items, review gates, monitoring or alerting. When Bitbucket PRs or file paths were retrieved, point coaches at those starting points. Keep each recommendation to one or two sentences. Do not embed unmarked root-cause assertions.)

---

## Automated mode only: append this block

Post the comment with `contentFormat: "html"`. The first paragraph's text is the marker: `QA RCA Coach — automated similar-ticket investigation`. The Task notice and the no-linked-ENREQ notice also start with it. There is one marked comment per ticket; later runs edit it rather than adding another.

When the ticket's project key has one or more coaches on [Quality Coaches and their teams](https://smartsensebydigi.atlassian.net/wiki/spaces/EN/pages/6228082716/Quality+Coaches+and+their+teams), the next paragraph mentions each of them. Use the account ID copied from that page. When the project has no coach, omit this paragraph and do not mention anyone:

`<span data-type="mention" data-user-id="ACCOUNT_ID">@Display Name</span>`

Then the template above, as HTML, then:

🔗 CONTINUE IN RCA COACH

The full RCA session, including the 5 Whys, root cause identification, and CAPA, is completed in the RCA Coach.

👉 [Continue RCA for ENREQ-XXXX](https://cursor.com/link/prompt?text=Read%20https%3A%2F%2Fgithub.com%2Forlandodelamota-code%2FQA-RCA-Agent%2Fblob%2Fmain%2F.cursor%2Fskills%2Fqa-rca-coach%2FSKILL.md%20and%20https%3A%2F%2Fgithub.com%2Forlandodelamota-code%2FQA-RCA-Agent%2Fblob%2Fmain%2F.cursor%2Fskills%2Fqa-rca-coach%2Freferences%2Foutput-format.md%20from%20the%20public%20repo%20first%2C%20then%20use%20the%20QA%20RCA%20Coach%20skill%20in%20interactive%20mode%20on%20ENREQ-XXXX.%20The%20Similar%20Ticket%20Investigation%20already%20ran.%20Do%20not%20re-run%20it.%20Proceed%20directly%20into%20the%205%20Whys%20using%20the%20investigation%20comment.)

Replace both `ENREQ-XXXX` occurrences (visible label and encoded prompt) with the source ENREQ key. Open the link to start the coach with this ticket pre-loaded. The prompt reads the public skill files first, then goes into the 5 Whys. The investigation will not re-run.

## Condensed delta format (manual re-run mid-session)

Same headers, but list only tickets or findings that are new or changed since the last run, and omit unchanged sections with "No change since last run".
