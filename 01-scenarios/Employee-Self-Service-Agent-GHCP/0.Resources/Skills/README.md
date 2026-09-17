# Runtime Skill Index

Import in the following order, which matches the [capability matrix](../Capability-matrix.md). Each file is a complete, independently usable runtime definition for the GitHub Copilot harness in Microsoft Copilot Studio. None references a sibling skill or requires repository access.

| Order | Capability ID / skill | Profile | Dependencies |
|---|---|---|---|
| 1 | [hr-policy-guidance](hr-policy-guidance/SKILL.md) | Baseline | Configured approved HR knowledge, trusted contact route when available |
| 2 | [it-support-guidance](it-support-guidance/SKILL.md) | Baseline | Configured approved IT KB, trusted support route when available |
| 3 | [request-catalog-navigation](request-catalog-navigation/SKILL.md) | Baseline | Configured approved catalog or request directory with actual URLs |
| 4 | [own-request-status](own-request-status/SKILL.md) | Proposed / OFF | Implemented and caller-authorized `ReadOwnRequests` custom contract, explicit profile enablement |

Use **Build > Skills > Add skill > Upload a skill**, select one `SKILL.md`, and inspect its validated name, description and full instructions. Repeat in order for enabled capabilities. The first three need no external operation tools. Do not import the fourth into a baseline agent.

For isolated extension testing, import the fourth into a separate test agent with a synthetic backend and trusted test-profile configuration. Only an owner-approved, caller-authorized pilot may access real allowlisted IT requests. Broad extension release follows its own evidence gates, not baseline approval.

No supporting asset package is needed for these files. If assets are added later, upload a ZIP containing `SKILL.md` and every referenced asset. See [official upload guidance](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/skills-add-existing). Create from blank or Generate with AI are [documented alternatives](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/skills-create); inspect any generated changes.

Skill upload does not create knowledge connections, operation implementations, authentication, permission rules or deterministic orchestration. Configure those dependencies and run standalone, boundary and composed tests before release. Follow the [Runbook](../../3.Runbook.md) for agent-wide instructions and rollout.
