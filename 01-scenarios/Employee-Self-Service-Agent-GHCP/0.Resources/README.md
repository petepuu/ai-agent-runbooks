# Resources — Employee Self-Service (GHCP)

This package contains documentation, capability contracts and four standalone runtime skill definitions. It contains no tenant export, screenshot, executable integration, credential, production policy or live test result.

## Resource inventory

| Resource | Purpose |
|---|---|
| [Capability matrix](Capability-matrix.md) | Canonical capability order, profiles, knowledge dependencies and proposed own-request contract |
| [Skill index](Skills/README.md) | Standalone imports in matrix order |
| [Controlled fixtures](../4.Sample-prompts.md#controlled-fixtures) | Complete synthetic policy, IT, catalog and status data for isolated tests |
| [Release gates](../3.Runbook.md#release-gates) | Owner-assigned configuration, acceptance, pilot and release requirements |

Do not upload this entire Resources directory as agent knowledge. Runtime skills are imported individually. Fixtures belong only in an isolated test source, and deployment records belong in an access-controlled operational location.

## Knowledge quality requirements

1. HR and IT owners name the authority for each domain, country/category and effective period. Maintain one current applicable policy or an explicit, authorized precedence rule.
2. Record source title, actual URL/ID, owner, version where available, applicability, permission scope and publication/effective dates separately.
3. Confirm readable text and usable citations, including tables and scanned documents. Exclude unusable material until the owner publishes an approved accessible version.
4. Validate knowledge-base/article and catalog category/item permissions, user mapping and revocation with non-maker identities. Connector service-account access is not caller access.
5. Validate each exact request URL in the intended employee portal without submitting anything. Do not construct guessed paths or treat a catalog entry as approval, stock availability or fulfillment.
6. Keep personal HR records out of the knowledge corpus. An indexed document is not a live request-status feed.
7. Define content/permission freshness limits and owner response times before release. No crawl interval or live freshness guarantee is assumed.

## Official-source verification

The pages below were fetched for the product claims listed. **Documented** means public guidance was verified, not that the tenant was tested. **Design choice** means a scenario boundary, and **proposed contract** means executable work remains outstanding. Deployment values and gates are owned in the Runbook.

| Official source | Documented guidance used | Tenant check still required / owner |
|---|---|---|
| [GHCP overview](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/overview) | Studio components include instructions, knowledge, tools, skills, model and memory. Build, test, evaluate and use may consume Copilot Credits | Harness access, model, region, data policy and budget / tenant and cost owners |
| [Create a new agent](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/build-new-agent) | Home > Agent or Agents > New agent, settings before first save, instructions and Save | Actual environment, roles, solution, language and schema / delivery owner |
| [Add knowledge](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/knowledge-add-existing-copilot) | Build > Knowledge supports SharePoint and ServiceNow source selection, no general-knowledge toggle | Selected connection visibility, citation behavior and employee permission boundary / delivery and identity owners |
| [Create skills](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/skills-create) | Build > Skills supports Create from blank and Generate with AI, descriptions guide activation | Imported instructions and activation tests / delivery owner |
| [Upload skills](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/skills-add-existing) | Build > Skills > Add skill > Upload a skill accepts Markdown with YAML name/description, or ZIP including SKILL.md and assets | All four definitions validate independently, only enabled profile imported / delivery owner |
| [Add tools](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/add-tools-custom-agent) | Tool picker exposes actions, custom tool routes include MCP and workflows | Real operation ID, schema, caller propagation and backend authorization / integration owner |
| [ServiceNow Knowledge deployment](https://learn.microsoft.com/en-us/microsoft-365/copilot/connectors/servicenow-knowledge-deployment) | Connector gallery path, authentication choices, scripted criteria flow, HRSD warning and Ready validation | ServiceNow prerequisites, article/base ACLs, HR roles, identity mapping and revocation / ServiceNow and identity owners |
| [ServiceNow Catalog deployment](https://learn.microsoft.com/en-us/microsoft-365/copilot/connectors/servicenow-catalog-deployment) | Indexes catalog items for discovery, has Simple/Advanced criteria flows and portal URL configuration | Category/item permissions, exact links and Studio consumption of this connection / catalog and delivery owners |
| [Preview](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/preview-overview) | Interactive conversations, new sessions, source/tool activity and feedback | Real evidence and identity-bound behavior / test owner |
| [Evaluate](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/analytics-agent-evaluation-intro) | Named evaluations, conversations, version and authenticated profile, General quality scoring does not compare expected answers | Availability and manual verification of exact safety/correctness criteria / test owner |
| [Publish overview](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/publication-fundamentals-publish-channels) | Draft and published versions are distinct, distribution follows publication | Restricted audience and rollback evidence / release owner |
| [Publish an agent](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/publication-publish-agent) | Publish chevron, configure channels, confirm, wait for completion, catalog/share options | Channel data policy, authentication, publication result / release and tenant owners |
| [Available channels](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/publication-channels-overview) | Teams and Microsoft 365 Copilot listed as available | Intended audience and cloud-specific behavior / tenant owner |

## Design choices and unresolved integration evidence

- **Design choice:** read-only baseline, no general web retrieval or external operation tools, memory off initially, no automatic employee-profile lookup.
- **Design choice:** live status covers only the caller's own allowlisted non-sensitive IT service requests. HR personal records and all writes remain excluded.
- **Proposed contract:** `ReadOwnRequests` has no implementation or vendor operation ID. It is OFF until owner-assigned backend, identity and release gates close.
- **Tenant gate:** the ServiceNow Catalog connector's existence does not prove the intended GHCP source picker can consume that configured catalog connection. Test it end to end. If unavailable, explicitly approve a curated SharePoint request directory and test its permissions/links instead.
- **Tenant gate:** no current HRIS knowledge adapter or personal-record route is assumed. A proposed published-policy connection must be qualified independently; it is not a baseline dependency.
- **Tenant gate:** official guidance does not establish exact licensing entitlement, language quality, data residency compliance, model availability or a working rollout in the target tenant.

## Operational records

Keep the content audit, aggregate deflection baseline, actual connection identifiers, model/version, real test citations, authorization evidence, pilot decision and rollback record in a restricted deployment store. Avoid full employee question logs when redacted categories and references are sufficient. Never put credentials, personal records or raw tool payloads in this repository.
