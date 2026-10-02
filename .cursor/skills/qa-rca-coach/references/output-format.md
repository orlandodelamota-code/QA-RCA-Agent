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

The first line of the Jira comment is the loop-prevention marker: `QA RCA Coach — automated similar-ticket investigation`

Then the template above, then:

🔗 CONTINUE IN RCA COACH

The full RCA session, including the 5 Whys, root cause identification, and CAPA, is completed in the RCA Coach.

👉 [Continue RCA for ENREQ-XXXX](https://cursor.com/link/prompt?text=Use%20the%20QA%20RCA%20Coach%20skill%20in%20interactive%20mode%20on%20ENREQ-XXXX.%20The%20Similar%20Ticket%20Investigation%20already%20ran.%20Do%20not%20re-run%20it.%20Proceed%20directly%20into%20the%205%20Whys%20using%20the%20investigation%20comment.)

Replace both `ENREQ-XXXX` occurrences (visible label and encoded prompt) with the source ENREQ key. Open the link to start the coach with this ticket pre-loaded. The investigation will not re-run; proceed directly into the 5 Whys using the findings above.

## Condensed delta format (manual re-run mid-session)

Same headers, but list only tickets or findings that are new or changed since the last run, and omit unchanged sections with "No change since last run".
