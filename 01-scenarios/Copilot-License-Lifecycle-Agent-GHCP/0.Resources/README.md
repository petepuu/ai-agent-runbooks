# Copilot Licence Lifecycle Agent — Resources

## Contents and implementation status

| Folder / file | Purpose |
|---|---|
| [Capability-matrix.md](Capability-matrix.md) | Ten included capabilities, logical operation schemas, evidence and transaction controls |
| [Skills/README.md](Skills/README.md) | Runtime import order, dependencies and capability enablement |
| `Skills/<capability-id>/SKILL.md` | Standalone skill instructions, not tools or connection installers |
| [Sample Prompts](../4.Sample-prompts.md) | Self-contained synthetic fixtures and acceptance expectations |
| [Power Platform Solution](Power%20Platform%20Solution.md) | Illustrated steps to create the solution and set or verify the maker's preferred solution |
| [Build-prompts/1.Dataverse.md](Build-prompts/1.Dataverse.md) | First stage: one Power Apps Plans prompt for five tables with 31 custom columns in Copilot License Lifecycle; ask before creating a missing solution, no sample data or model-driven app |
| [Build-prompts/2.Agent-and-workflows.md](Build-prompts/2.Agent-and-workflows.md) | Second maker prompt: agent, workflows and integrations using the existing schema |

Delivered assets are documentation, illustrative screenshots and runtime
definitions only. No deployed agent, flow export, service credentials, real employee data or
successful tenant test evidence is included. Keep operational evidence and
environment values in the organization's access-controlled delivery record.

Run the two build stages in order. Paste the complete stage-1 prompt block into
Power Apps Plans, not the table-creation dialog. Review the data model and save
the tables into **Copilot License Lifecycle**, without building a model-driven app.
If the solution is missing, ask before creating it. Do not add sample data.
Verify the maker's preferred-solution setting, completing it manually if needed.
Use [Power Platform Solution](Power%20Platform%20Solution.md) for those manual steps.
These are authoring inputs
for the maker surfaces, not additional runtime skills. The second stage requires actual saved
metadata and the listed context files; repository paths alone do not make those
files readable in the Studio builder. Generated artifacts remain subject to the
same release gates and external integration requirements.

The five tables are `LicenseHolder`, `WaitlistEntry`, `ReclaimCase`,
`Configuration` and `AuditEvent`, with the 26 base columns and five approved
lookup columns in Architecture (31 custom columns).
The [storage contract](Capability-matrix.md#five-table-storage-contract) separates
this minimal schema from required integration controls. Missing durable approval,
recovery or evidence components block dependent capabilities; do not automatically
add tables, fields or lookups beyond that definition, or an expanded JSON store
to address those gaps.

## Release gates

Each gate must be resolved by its named role before dependent production behavior
is enabled. These are implementation requirements, not optional capabilities.

| Gate | Owner | Required evidence / acceptance |
|---|---|---|
| G1 — Host and components | Delivery engineer | [VERIFY] GHCP harness, approved model, reachable scoped MCP endpoint, exact tool schemas, ten imported skills and build access in the target tenant |
| G2 — Identity and approvals | M365 admin / integration owner | [VERIFY] Trusted caller binding, current role enforcement, non-maker attribution, exact-payload approval adapter and rejection of audit text presented as approval |
| G3 — Licence and usage evidence | M365 data owner | [VERIFY] Global-cloud API support, approved SKU resolution, CSV version/columns, non-anonymized authorized identity join, coverage/freshness, direct/inherited classification and complete paging |
| G4 — Notifications and routing | Messaging owner | [VERIFY] Actual Teams/Outlook operation IDs and supported authentication, sender/poster rights, both initial channels, reminders, dispute route and timeout reconciliation; no assumed provider message ID |
| G5 — Business configuration | Licence policy owner | [FILL] Approved settings/revision, threshold, grace/reminders, maximum evidence age, group IDs, configured tenant/SKU, queue order, approver/backup and escalation route; [VERIFY] backend enforces <=25 and approved-only reclaim |
| G6 — Runtime and recovery | Test / privacy owner | [VERIFY] 26 base columns plus five lookups, required link consistency and Restrict Delete, approved durable control integrations, capability pairs, concurrency, composed flow, failure paths and retention pass in isolation and approved pilot |
| G7 — Channels and cost | Release / cost owner | [VERIFY] Teams and M365 Copilot identity/confirmation parity, permitted audience, licensing, measured build/test/run costs and demonstrated budget controls |
| G8 — ALM and rollback | Power Platform / integration owner | [VERIFY] Dev/Test/Prod component transport, external backend deployment, connection rebinding, skill version traceability, action pause, proposal invalidation and reconciliation drill |

Do not reduce a gate to an instruction telling the model to be careful. In
particular, authenticated approval capture and backend atomic reservations are
code/integration requirements not supplied by these Markdown files.

## Official-source verification

**Documented** means the linked guidance supports the stated product behavior.
**Design choice** and **scenario-defined contract** describe this scenario's
required implementation. Neither establishes tenant availability or execution.

| Source | Documented claim used | Tenant check / owner |
|---|---|---|
| [Harnesses](https://learn.microsoft.com/en-us/microsoft-copilot-studio/harnesses-overview) and [GHCP overview](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/overview) | GHCP uses instructions, skills and tools in Studio; harnesses are distinct | G1, delivery engineer |
| [Create an agent](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/build-new-agent) | Home > Agent or Agents > New agent; details/solution before first save | G1/G8, delivery engineer |
| [Power Apps Plans](https://learn.microsoft.com/en-us/power-apps/maker/plan-designer/create-plan) and [prerequisites](https://learn.microsoft.com/en-us/power-apps/maker/plan-designer/plan-designer) | Plans proposes a data model; Save tables creates/selects a solution and publisher. Environment redirection may apply | G1/G8, verify environment, solution and all saved columns; do not build proposed apps in stage 1 |
| [Create a solution](https://learn.microsoft.com/en-us/power-apps/maker/data-platform/create-solution) and [preferred solution](https://learn.microsoft.com/en-us/power-apps/maker/data-platform/preferred-solution) | Display and unique names differ; preferred solution is maker-specific, with component limitations | G8, verify Copilot License Lifecycle, actual unique name/publisher and preferred indicator; configure agent solution separately |
| [Dataverse relationships](https://learn.microsoft.com/en-us/power-apps/maker/data-platform/create-edit-entity-relationships) | Lookup columns implement 1:N/N:1 associations; delete/cascade behavior is configurable | G6, verify the five approved lookups, requiredness and non-cascading history protection |
| [Natural-language builder](https://learn.microsoft.com/en-us/microsoft-copilot-studio/create-automation-natural-language) and [Workflows](https://learn.microsoft.com/en-us/microsoft-copilot-studio/workflows-experience/flows-overview) | Studio can generate agent/workflow artifacts; workflow types and connection setup remain explicit | G1/G2/G8, verify engine, invocation, artifacts and external backend dependencies |
| [Skills overview](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/skills-overview) and [upload](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/skills-add-existing) | Markdown with YAML name/description, or ZIP with SKILL.md and assets | G1, delivery engineer |
| [Tools](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/add-tools-custom-agent) and [MCP](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/tools-add-mcp-server) | Build > Tools supports MCP; adding a server exposes its tools | G1/G2, narrow facade and auth validation |
| [Users](https://learn.microsoft.com/en-us/graph/api/user-list?view=graph-rest-1.0) and [subscribed SKUs](https://learn.microsoft.com/en-us/graph/api/subscribedsku-list?view=graph-rest-1.0) | Directory fields, paging, permissions and subscription inventory | G3, M365 data owner |
| [Copilot usage API](https://learn.microsoft.com/en-us/microsoft-365/copilot/extensibility/api/admin-settings/reports/copilotreportroot-getmicrosoft365copilotusageuserdetail) | v1.0 `/copilot/reports` CSV, report versions, global-cloud/licensed-user scope and `Reports.Read.All` | G3, schema/identity/coverage checks |
| [Usage report](https://learn.microsoft.com/en-us/microsoft-365/admin/activity-reports/microsoft-365-copilot-usage?view=o365-worldwide) | Typical reporting delay, not real-time usage | G3/G5, freshness and residual-risk approval |
| [Assignment state](https://learn.microsoft.com/en-us/graph/api/resources/licenseassignmentstate?view=graph-rest-1.0) and [assignLicense](https://learn.microsoft.com/en-us/graph/api/user-assignlicense?view=graph-rest-1.0) | Direct/inherited provenance and allowed Graph licence operation | G2/G3, least privilege and safe readback |
| [Dataverse conditional operations](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/webapi/perform-conditional-operations-using-web-api) | ETags and optimistic concurrency | G6, atomic backend reservation/recovery implementation |
| [Custom APIs](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/custom-api) and [database transactions](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/org-service/use-executetransaction) | Code-backed custom operations and atomic database requests; batch requests are not for nesting in plug-ins | G2/G6, demonstrate the approved backend control implementation; no transaction includes Graph |
| [Teams connector](https://learn.microsoft.com/en-us/connectors/teams/) and [Outlook connector](https://learn.microsoft.com/en-us/connectors/office365/) | Connector requirements/limitations, including send-result limitations | G4, selected operation and connection proof |
| [Preview](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/preview-overview) and [Evaluate](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/analytics-agent-evaluation-intro) | Interactive traces and version/profile-based evaluation; General quality is not expected-answer comparison | G6, explicit backend assertions alongside quality scores |
| [Publish](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/publication-fundamentals-publish-channels) and [channels](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/publication-channels-overview) | Publish creates a live version; Teams and M365 Copilot are documented channels | G7/G8, restricted distribution and per-channel tests |
| [Credits and capacity](https://learn.microsoft.com/en-us/power-platform/admin/manage-copilot-studio-copilot-credits-capacity) | Agent/environment monitoring and allocation across harnesses | G7, current licensing, measured usage and actual enforcement |

## Design choices and operational records

The narrow MCP facade, ten tool-backed skills, approved-only writes, transactional
ledger, queue rules and notification recovery are **scenario-defined contracts**.
They are not built-in Copilot Studio guarantees. No indexed knowledge or
autonomous agent-to-workflow round trip is needed for this design.

Record signed policy, actual backend tool versions, connection owners, tenant/SKU
mapping, channel/version test runs and release approvals in a restricted delivery
record. Store no credentials or identifiable usage reports in this repository.
Maintain evidence of successful partial steps so rollback cannot erase history.
