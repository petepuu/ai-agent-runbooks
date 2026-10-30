# Resources - Employee Self-Service Agent with GHCP Harness

These are authoring artifacts, not an installed ESS solution or a deployed GHCP agent.

| Artifact | Purpose |
|---|---|
| [Capability matrix](Capability-matrix.md) | Source dispositions, 28 capability IDs, proposed tools, dependencies and default OFF states |
| [Integration contracts](Integration-contracts.md) | Proposed backend inputs/outputs, scope, identity, approvals, concurrency and reconciliation |
| [References and validation checks](Official-sources.md) | Setup links, support boundaries and tenant validation owners |
| [Skills index](Skills/README.md) | All 28 standalone runtime definitions in matrix/import order |

The source is `01-scenarios\Employee-Self-Service-Agent` at local revision `a776697d6c072ec6d4cf8b09ba7670bd11a3e73c`. No original asset is copied or modified. The new destination is the sibling `01-scenarios\Employee-Self-Service-Agent-GHCP`.

Runtime skills have only `name` and quoted `description` front matter, and Scope, Tools, Inputs, Procedure, and Results and Failure Handling sections. They reference no unpackaged repository resources. Upload a standalone `SKILL.md` through **Build > Skills > Upload a skill**. A ZIP is needed only if adding supporting assets. Importing skills does not create connections, workflows or backend authorization.

Keep actual tenant values, credentials, personal records and restricted test evidence out of this public repository. Store release evidence in the organization's approved restricted location, recorded by the owners in the [runbook](../3.Runbook.md#release-gates).
