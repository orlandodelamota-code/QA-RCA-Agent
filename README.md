# QA RCA Agent

Cursor skill and webhook prompt for the QA RCA Coach. This private GitHub repo is the checkout the Cursor automation uses so a Jira ENREQ-label rule can start a cloud run. It is not published to Bitbucket.

The skill is [`.cursor/skills/qa-rca-coach/SKILL.md`](.cursor/skills/qa-rca-coach/SKILL.md). The investigation comment template is [`.cursor/skills/qa-rca-coach/references/output-format.md`](.cursor/skills/qa-rca-coach/references/output-format.md). Paste [`automation/webhook-prompt.md`](automation/webhook-prompt.md) into the automation instructions.

Source ticket: [QA-3114](https://smartsensebydigi.atlassian.net/browse/QA-3114). The Rovo agent stays on until a person compares a side-by-side run and turns it off.

## What talks to what

1. Jira **RCA Workflow** (Asset Monitoring) fires when **Labels** contains **ENREQ**.
2. The rule **Send web request** POSTs the ticket key to the Cursor automation webhook.
3. The automation checks out this repo on `main`, follows the webhook prompt, and posts one comment on that product-board ticket through the Atlassian connection selected on the automation.

Allowed projects: AM, LW, SSV, DOPS, DEVOPS, TOOLS, VOY, LSS. A Quality Assurance (QA) key is ignored. Bug versus Task is decided on the product-board ticket. A Task gets only the short “bug-related issues only” note.

## Cursor automation

Open [cursor.com/automations](https://cursor.com/automations) and select **QA RCA Coach webhook test**.

| Setting | Value |
|---------|--------|
| Trigger | Incoming webhook |
| Repository | `orlandodelamota-code/QA-RCA-Agent` |
| Branch | `main` |
| Model | Auto |
| Tools | Atlassian (connect it on this automation; a chat connection does not carry over) |
| Instructions | Paste the prompt block from [`automation/webhook-prompt.md`](automation/webhook-prompt.md) |

Save the automation. The webhook address and API key appear only after the first save. If a later save rotates them, copy the new values into Jira.

Leave Share private if the automation should stay on your account. Choosing this repository does not commit the coach anywhere else and does not put it in Bitbucket.

## Jira Send web request

On the Asset Monitoring **RCA Workflow**:

1. Keep **Work item updated**.
2. Keep **Labels contains ENREQ**.
3. Remove the Rovo **QA RCA Coach** action and the canned “description quality check” comment if they are still on this flow. Turning the Rovo agent off stops Rovo’s comments; it does not add this web request. Turning this webhook off does not stop Rovo.
4. Add **Send web request**:
   - Method: POST
   - URL: the webhook address from the saved Cursor automation
   - Body: Custom data, exactly `{"issueKey": "{{issue.key}}"}`
   - Headers:

| Key | Value | Hidden |
|-----|--------|--------|
| `Content-Type` | `application/json` | no |
| `Authorization` | `Bearer` then one space, then the webhook key | yes |

The header **key** is only `Authorization`. The word `Bearer` belongs in the **value**, once, with a single space before the key. Do not put `Authorization` or a second `Bearer` in the value. Do not wrap the key in quotes or angle brackets.

5. Do not also trigger this rule when a comment is added. The coach’s own comment would start another run.
6. Leave the flow enabled and save.

## Comment shape

Automated comments start with:

`QA RCA Coach — automated similar-ticket investigation`

They then use the similar-ticket template and end with a Continue link into the interactive 5 Whys. The run does not ask questions and does not start the 5 Whys.

If that marker, or the older heading `SIMILAR TICKET INVESTIGATION & GAP ANALYSIS`, is already on the ticket, the run does not post again. If Rovo and this webhook finish at the same time, both comments can still land.

## Troubleshooting

| Audit log / symptom | What to change |
|---------------------|----------------|
| No request in the audit log | The rule did not run. Update a ticket in a project that rule covers (Asset Monitoring), and the ticket must already match **Labels contains ENREQ**. An LSS key does not fire an Asset Monitoring rule. |
| 401 `Malformed Authorization header` | Value must be `Bearer <key>` only. Key column must be `Authorization`. |
| 400 `Automation does not have git configuration` | Set the repository to this repo and branch `main`, then save. **No repository** does not clear this error for this webhook. |
| 400 `Failed to start background composer: [not_found]` | Set the model to **Auto**. Confirm this GitHub repo is connected in Cursor, then save and use **Run test** on the automation page. |
| HTTP 2xx, run says no Atlassian/Jira tools | The automation has no working Atlassian MCP. Add it under the automation's Tools as an MCP server and finish its sign-in there. A Jira connection in the Cursor app does not carry over to automation runs. |
| HTTP 2xx and no Jira comment | The run started and then stopped. Confirm the key is in AM, LW, SSV, DOPS, DEVOPS, TOOLS, VOY, or LSS, the ENREQ label is present, and Atlassian is connected on the automation. A QA-only prompt posts nothing on an Asset Monitoring ticket. |
| Two comments | Rovo and this webhook both ran. They are separate. Turn Rovo off only after a side-by-side comparison. |

## Cutover

This repo does not disable the Rovo agent. After one bug-type ENREQ is investigated by both, compare the comments, then turn the Rovo agent off.
