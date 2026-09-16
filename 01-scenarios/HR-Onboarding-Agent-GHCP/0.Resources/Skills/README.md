# HR onboarding runtime skills

These files target **Microsoft Copilot Studio's GitHub Copilot harness**. They are not installed into the repository coding assistant.

| Matrix order | Capability / upload file | Baseline | Dependency |
|---|---|---|---|
| 1 | [hr-policy-answer](hr-policy-answer/SKILL.md) | ON after gates | Approved HR knowledge with caller ACLs |
| 2 | [hr-onboarding-checklist](hr-onboarding-checklist/SKILL.md) | ON after gates | Approved onboarding knowledge |
| 3 | [hr-email-draft](hr-email-draft/SKILL.md) | ON after gates | Inquiry text and approved HR knowledge |
| 4 | [hr-email-reply](hr-email-reply/SKILL.md) | OFF | Three implemented email operations and bound HR approval |

Each skill is standalone with its declared dependencies; none requires a sibling skill. The sending skill can accept a reviewed payload produced by a human. It does not implicitly enable drafting or arbitrary mailbox actions.

In the new agent, use **Build > Skills > Add skill > Upload a skill** and upload each enabled capability's `SKILL.md` separately. No ZIP or additional assets are needed for these files. If adding support files later, package `SKILL.md` and those files together in a ZIP and keep references inside that package.

Alternatively use **Create from blank**, copying YAML name/description into the corresponding fields and the Markdown body into Instructions, or **Generate with AI**, then inspect the complete result. Validate the installed name and body, not just the upload success message.

Uploading instructions creates no source connection, callable tool, workflow, event subscription or authorization. Do not upload the sending skill into the baseline. For production reviewed-email use, first close gates E1-E3 and align the real tool names with the skill and [matrix](../Capability-matrix.md). To gather gate evidence, the runbook permits isolated non-sending tests and, after E1-E2 close, an explicitly authorized restricted mailbox pilot. Keep those test/pilot agents separate from the baseline and production audience.

See [Runbook](../../3.Runbook.md) for global instructions and setup, and [acceptance cases](../../4.Sample-prompts.md) for positive, boundary and composed tests.
