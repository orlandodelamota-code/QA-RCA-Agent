# Similar Ticket Investigation: Required Output Format

Use this template exactly. Replace bracketed items. Keep every section header; write "None found" for empty sections. Do not add prose before or after.

---

📋 SIMILAR TICKET INVESTIGATION & GAP ANALYSIS

🔍 SEARCH CRITERIA

Source ENREQ: [ENREQ-XXXX](https://smartsensebydigi.atlassian.net/browse/ENREQ-XXXX)
Keywords: [extracted from ENREQ ticket]
Feature Area: [identified area]
Error Type: [identified type]
Platform: [Mobile / Web / Unconfirmed]
Excluded from results: [ENREQ-XXXX] and cloned tickets ([AM-XXXX], etc.)

📂 SIMILAR OPEN TICKETS (X found)

[TICKET-KEY](https://smartsensebydigi.atlassian.net/browse/TICKET-KEY): [Summary]
- Status: [status]
- Linked Dev Ticket: [PROD-YYYY], [status, assignee]
- Key Context: [relevant finding]
- ⚠ Potential shared root cause: still unresolved (include only when it applies)

📁 SIMILAR CLOSED TICKETS (X found)

[TICKET-KEY](https://smartsensebydigi.atlassian.net/browse/TICKET-KEY): [Summary]
- Resolution: [Fixed / Won't Fix / etc.]
- Fix Description: [what was done]
- Fix Quality: [✅ Root cause resolution / ⚠️ Temporary fix / ❓ Inconclusive], [rationale]
- Test Coverage Gaps:
  - [Gap 1]
  - [Gap 2]
- Recurrence Risk: [High / Medium / Low]

(Add "X additional tickets found ..." note under each group when results were truncated.)

📊 GAP ANALYSIS SUMMARY

Test Coverage Gaps: [numbered list]
Fix Quality Overview: X root cause fixes, Y temporary, Z inconclusive
Overall Recurrence Risk: [High / Medium / Low], [brief rationale]

💡 RECOMMENDATIONS

⭐ Priority 1: [highest priority recommendation]
⭐ Priority 2: [second priority recommendation]
- [Additional recommendation]
- [Additional recommendation]

(Recommendations cover: new test cases to write with suggested scope; existing test cases to strengthen or extend; process improvements such as checklist items, review gates, monitoring or alerting.)

---

## Automated mode only: append this block

Post the comment with `contentFormat: "html"`. The first paragraph's text is the marker: `QA RCA Coach — automated similar-ticket investigation`. The Task notice and the no-linked-ENREQ notice also start with it. There is one marked comment per ticket; later runs edit it rather than adding another.

The next paragraph mentions every quality coach for the ticket's project key, from [Quality Coaches and their teams](https://smartsensebydigi.atlassian.net/wiki/spaces/EN/pages/6228082716/Quality+Coaches+and+their+teams). Use the account ID copied from that page:

`<span data-type="mention" data-user-id="ACCOUNT_ID">@Display Name</span>`

Then the template above, as HTML, then:

🔗 CONTINUE IN RCA COACH

The full RCA session, including the 5 Whys, root cause identification, and CAPA, is completed in the RCA Coach.

👉 [Continue RCA for ENREQ-XXXX](https://cursor.com/link/prompt?text=Read%20https%3A%2F%2Fgithub.com%2Forlandodelamota-code%2FQA-RCA-Agent%2Fblob%2Fmain%2F.cursor%2Fskills%2Fqa-rca-coach%2FSKILL.md%20and%20https%3A%2F%2Fgithub.com%2Forlandodelamota-code%2FQA-RCA-Agent%2Fblob%2Fmain%2F.cursor%2Fskills%2Fqa-rca-coach%2Freferences%2Foutput-format.md%20from%20the%20public%20repo%20first%2C%20then%20use%20the%20QA%20RCA%20Coach%20skill%20in%20interactive%20mode%20on%20ENREQ-XXXX.%20The%20Similar%20Ticket%20Investigation%20already%20ran.%20Do%20not%20re-run%20it.%20Proceed%20directly%20into%20the%205%20Whys%20using%20the%20investigation%20comment.)

Replace both `ENREQ-XXXX` occurrences (visible label and encoded prompt) with the source ENREQ key. Open the link to start the coach with this ticket pre-loaded. The prompt reads the public skill files first, then goes into the 5 Whys. The investigation will not re-run.

## Condensed delta format (manual re-run mid-session)

Same headers, but list only tickets or findings that are new or changed since the last run, and omit unchanged sections with "No change since last run".
