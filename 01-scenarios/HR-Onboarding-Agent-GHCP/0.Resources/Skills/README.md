# Employee HR runtime skills

These files target **Microsoft Copilot Studio's GitHub Copilot harness**. They are not installed into the repository coding assistant.

All four skills are imported during standard setup into the same HR agent. The request's trusted context selects the entry path.

| Matrix order | Capability / upload file | Standard use | Dependency |
|---|---|---|---|
| 1 | [hr-policy-answer](hr-policy-answer/SKILL.md) | Chat guidance and workflow evidence | Approved HR knowledge with caller ACLs or enforced recipient-safe workflow scope |
| 2 | [hr-onboarding-checklist](hr-onboarding-checklist/SKILL.md) | Chat checklist and workflow evidence | Applicable approved onboarding knowledge, including role changes only where supported |
| 3 | [hr-email-draft](hr-email-draft/SKILL.md) | Draft-only chat response | Inquiry text and authorized evidence, chat drafting never sends |
| 4 | [hr-email-reply](hr-email-reply/SKILL.md) | Structured response to authenticated workflow intake | Trusted invocation/context, recipient-safe knowledge, response catalog, deterministic validation/send/status/exception backend configured during setup |

Each skill is standalone with its declared dependencies; none requires a sibling skill. The workflow-reply skill prepares structured evidence/block references, not mail. It does not implicitly enable another capability. No agent mailbox tools are used. All five scenario-defined backend contracts are implemented during setup and called by the surrounding workflow/operator, not imported with a skill.

In the new agent, use **Build > Skills > Add skill > Upload a skill** and upload all four `SKILL.md` files separately in matrix order. No ZIP or additional assets are needed for these files. If adding support files later, package `SKILL.md` and those files together in a ZIP and keep references inside that package.

Alternatively use **Create from blank**, copying YAML name/description into the corresponding fields and the Markdown body into Instructions, or **Generate with AI**, then inspect the complete result. Validate the installed name and body, not just the upload success message.

Uploading instructions creates no source connection, callable tool, workflow, event subscription or authorization. Configure both entry paths and align the trusted input/result schemas with the [matrix](../Capability-matrix.md). Pasted email in chat is untrusted content, not a workflow event, so it never initiates sending. Routine policy-authorized workflow events need no per-message human approval. Complete the runbook's required deployment checks for the whole scenario: first use isolated non-sending mocks, then a restricted real-mail pilot explicitly authorized after E1-E2 pass, and record E3 evidence before wider release. Keep test/pilot configurations separate from production sending connections and audiences.

See [Runbook](../../3.Runbook.md) for global instructions and setup, and [acceptance cases](../../4.Sample-prompts.md) for positive, boundary and composed tests.
