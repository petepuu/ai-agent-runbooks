# Employee HR runtime skills

These files target **Microsoft Copilot Studio's GitHub Copilot harness**. They are not installed into the repository coding assistant.

| Matrix order | Capability / upload file | Baseline | Autonomous-workflow | Dependency |
|---|---|---|---|---|
| 1 | [hr-policy-answer](hr-policy-answer/SKILL.md) | ON after gates | ON after gates | Approved HR knowledge with caller ACLs or enforced recipient-safe workflow scope |
| 2 | [hr-onboarding-checklist](hr-onboarding-checklist/SKILL.md) | ON after gates | ON after gates | Applicable approved onboarding knowledge, including role changes only where supported |
| 3 | [hr-email-draft](hr-email-draft/SKILL.md) | ON after gates | ON after gates | Inquiry text and authorized evidence, chat drafting never sends |
| 4 | [hr-email-reply](hr-email-reply/SKILL.md) | OFF | OFF until E1-E3, then ON | Trusted invocation/context, recipient-safe knowledge, response catalog, implemented deterministic validation/send/status/exception backend |

Each skill is standalone with its declared dependencies; none requires a sibling skill. The workflow-reply skill prepares structured evidence/block references, not mail. It does not implicitly enable another capability. No agent mailbox tools are used. All five proposed backend contracts are called by the surrounding workflow/operator, not imported with a skill.

In the new agent, use **Build > Skills > Add skill > Upload a skill** and upload each enabled capability's `SKILL.md` separately. No ZIP or additional assets are needed for these files. If adding support files later, package `SKILL.md` and those files together in a ZIP and keep references inside that package.

Alternatively use **Create from blank**, copying YAML name/description into the corresponding fields and the Markdown body into Instructions, or **Generate with AI**, then inspect the complete result. Validate the installed name and body, not just the upload success message.

Uploading instructions creates no source connection, callable tool, workflow, event subscription or authorization. Do not upload the workflow-reply skill into the baseline. For production autonomous-workflow use, close E1-E3 and align the trusted input/result schemas with the [matrix](../Capability-matrix.md). Routine policy-authorized events then need no per-message human approval. To gather evidence without circular gates, first use isolated non-sending mocks, then a restricted real-mail pilot explicitly authorized after E1-E2 pass. Keep those test/pilot configurations separate from production sending connections and audiences.

See [Runbook](../../3.Runbook.md) for global instructions and setup, and [acceptance cases](../../4.Sample-prompts.md) for positive, boundary and composed tests.
