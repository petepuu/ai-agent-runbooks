# Runtime skill index

Seven starter skills replace the earlier 28-skill inventory. Import only approved rows into the **Microsoft Copilot Harness in Microsoft Copilot Studio**, in this matrix order. Each file is standalone; tool bindings/permissions must be configured separately and remain OFF until validated.

| Order | Capability / standalone upload |
|---|---|
| 1 | [update-workday-home-phone-number-mcp](update-workday-home-phone-number-mcp/SKILL.md) |
| 2 | [get-workday-cost-center](get-workday-cost-center/SKILL.md) |
| 3 | [get-workday-employee-information-mcp](get-workday-employee-information-mcp/SKILL.md) |
| 4 | [request-workday-time-off-mcp](request-workday-time-off-mcp/SKILL.md) |
| 5 | [create-servicenow-incident](create-servicenow-incident/SKILL.md) |
| 6 | [list-open-servicenow-tickets](list-open-servicenow-tickets/SKILL.md) |
| 7 | [list-hr-cases](list-hr-cases/SKILL.md) |

Knowledge answers and source-backed navigation use global instructions and approved SharePoint/ServiceNow knowledge; no separate runtime skills are needed. A knowledge-only release imports none of these seven live skills.

Adapted from the seven user-supplied definitions on 2026-11-11. Names and core procedures are retained; own-record enforcement, minimal profile reads, unique caller resolution, safe retries and truthful status handling are aligned with the scenario. The supplied other-person incident search is intentionally omitted. Standard time-off submission is in scope; Leave of Absence is not.

For an existing deployment, remove the old runtime definitions rather than importing both inventories. Coordinate the [capability matrix](../Capability-matrix.md), tool exposure, backend permissions, active sessions and pending operations; removing a skill alone does not revoke a tool.

Upload through **Build > Skills > Upload a skill**. These Markdown files require no supporting assets or ZIP. These are Studio runtime definitions, not skills for the repository authoring assistant.
