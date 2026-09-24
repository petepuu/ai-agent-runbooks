# Copilot Licence Lifecycle Agent — Runtime Skills

All ten skills are included in the standard configuration. Import them in the
order below. Each file is self-contained with its declared tool dependencies;
none depends on another skill file being imported.

| Order | Capability / file | Purpose | Required tool contracts |
|---|---|---|---|
| 1 | [copilot-licence-inventory](copilot-licence-inventory/SKILL.md) | Read inventory and assignment evidence | `lifecycle_inventory` |
| 2 | [copilot-usage-insight](copilot-usage-insight/SKILL.md) | Usage, dormancy, trends and reclaim history | `lifecycle_usage` |
| 3 | [copilot-waitlist-status](copilot-waitlist-status/SKILL.md) | Read queue status and historical waits | `lifecycle_waitlist` |
| 4 | [copilot-waitlist-intake](copilot-waitlist-intake/SKILL.md) | Create a pending request | Shared action tools plus `lifecycle_waitlist_add` |
| 5 | [copilot-waitlist-review](copilot-waitlist-review/SKILL.md) | Approve, reject or reprioritize under policy | Shared action tools plus `lifecycle_waitlist_review` |
| 6 | [copilot-dormancy-notify](copilot-dormancy-notify/SKILL.md) | Confirm a bounded warning/reminder campaign | Shared action tools plus `lifecycle_notify` |
| 7 | [copilot-licence-reclaim](copilot-licence-reclaim/SKILL.md) | Remove an eligible direct assignment | Shared action tools plus `lifecycle_reclaim` |
| 8 | [copilot-licence-assign](copilot-licence-assign/SKILL.md) | Assign the approved eligible queue head | Shared action tools plus `lifecycle_assign` |
| 9 | [copilot-reclaim-cancel](copilot-reclaim-cancel/SKILL.md) | Cancel an open case without reversing a licence change | Shared action tools plus `lifecycle_cancel` |
| 10 | [copilot-reclaim-dispute](copilot-reclaim-dispute/SKILL.md) | Record and route a dispute, not restore a licence | Shared action tools plus `lifecycle_dispute` |

Shared action tools are `lifecycle_context`, `lifecycle_prepare` and
`lifecycle_status`. These are scenario-defined contracts implemented by the
scoped backend, not supplied code or built-in Microsoft operations.
See [Capability and Integration Contracts](../Capability-matrix.md).

Upload each `SKILL.md` in **Build > Skills > Add skill > Upload a skill**.
No ZIP is needed for these standalone files; if adding assets, package them
with the skill. Inspect name, quoted description and imported instructions
afterward. Uploading a skill does not create a tool, connection, schedule,
approval adapter or permission.

The first three skills are read-only. Each remaining skill has its own action
tool and permission; shared preparation/status tools do not authorize execution.
Routine daily/weekly jobs and approved notification-campaign resumption are
backend work, not additional skills.

For an operational pause, disable the action at the backend, coordinate tool
exposure and scheduled paths, and invalidate pending proposals. Removing a skill
alone does not revoke permission. Preserve shared read/status connections needed
by other enabled capabilities and reconciliation.

Use [T01-T10 and the composed tests](../../4.Sample-prompts.md#capability-tests)
before publication. Skill selection is not guaranteed ordering; the backend
enforces dependencies, approvals and transaction controls.
