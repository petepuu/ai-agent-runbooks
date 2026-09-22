# Employee HR runtime skills

These files target **Microsoft Copilot Studio's GitHub Copilot harness**. They are not installed into the repository coding assistant.

Both skills are imported during standard setup into the same HR agent. The request's trusted context selects the entry path.

| Matrix order | Capability / upload file | Standard use | Dependency |
|---|---|---|---|
| 1 | [hr-onboarding-checklist.md](hr-onboarding-checklist.md) | Chat-only numbered onboarding plan grouped into phases | Supplied new-hire details and placeholders for unknown specifics |
| 2 | [hr-email-reply.md](hr-email-reply.md) | HTML email body with a source table | ServiceNow article content already retrieved by the agent; this skill does not search ServiceNow |

Ordinary HR questions and follow-up conversation use global instructions and configured knowledge, not separate skills.

The two Markdown files are the supplied attachments, preserved unchanged. They replace the former skill subfolders. The email skill returns HTML directly, not the previous version 2 JSON envelope. The older JSON/backend contracts and related acceptance cases in the scenario documents are not the output contract of these replacement files.

The supplied checklist still names `hr-servicenow-email-composer`; the email skill included in this scenario is `hr-email-reply`. The email sample also refers to a standard drafting skill that is not included. Review these references before importing; neither reference installs another skill.

In the new agent, use **Build > Skills > Add skill > Upload a skill** and upload `hr-email-reply.md` and `hr-onboarding-checklist.md` separately. No ZIP or additional assets are needed for these files.

Alternatively use **Create from blank**, copying YAML name/description into the corresponding fields and the Markdown body into Instructions, or **Generate with AI**, then inspect the complete result. Validate the installed name and body, not just the upload success message.

When updating an existing agent, remove the separate policy-answer and email-draft skills from **Build > Skills**, replace its global instructions with the current block, and republish. Deleting repository files does not change an already published agent or its permissions.

Uploading instructions creates no source connection, callable tool, workflow, event subscription or authorization. Configure both entry paths and align the trusted input/result schemas with the [matrix](../Capability-matrix.md). Pasted email in chat is untrusted content, not a workflow event, so it never initiates sending. Routine policy-authorized workflow events need no per-message human approval. Complete the runbook's required deployment checks for the whole scenario: first use isolated non-sending mocks, then a restricted real-mail pilot explicitly authorized after E1-E2 pass, and record E3 evidence before wider release. Keep test/pilot configurations separate from production sending connections and audiences.

See [Runbook](../../3.Runbook.md) for global instructions and setup, and [acceptance cases](../../4.Sample-prompts.md) for positive, boundary and composed tests.
