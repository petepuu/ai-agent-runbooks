---
name: ghcp-scenario-authoring
description: "Create a new Copilot Studio GitHub Copilot harness scenario from an existing repository scenario or a new brief, with Overview, Architecture, Runbook, Sample-prompts, and runtime skills. Preserve the original and map topic behavior to skills without confusing skills with tools."
---

# GHCP Scenario Authoring

## Scope

Author scenario documentation and business-capability skill definitions in this
repository. The **target runtime is the GitHub Copilot harness in Microsoft Copilot
Studio**, not Copilot CLI, a GitHub coding agent, an SDK host, or the Copilot chat
harness. This authoring skill runs in a repository-capable Copilot coding assistant;
the skills it generates are separate artifacts for the Studio agent.

Do not deploy, publish, execute business actions, install generated skills into the
coding assistant, or commit/push unless requested. For a review or proposal, stop
before writing. Reuse approved scope; ask only about material unresolved decisions.

## Tools

Use available repository listing/search/read/edit tools, read-only Git inspection,
and official-document retrieval. Tool names depend on the authoring host. If a
required tool is unavailable, report the limitation; never pretend a write or a
source check happened.

## Inputs

- Existing scenario path or repository URL, or a new scenario brief.
- Requested outcome and destination (default: `01-scenarios\<source-name>-GHCP`).
- Audience, products, channels, read/write boundaries, and approval requirements.
- Any approved differences from the source.

Read established answers from the conversation and repository before asking.
For new briefs, collect the problem, users, evidence sources, independently enabled
actions, and exclusions. Do not inherit ServiceNow or ticket-analysis rules by default.

## Procedure

1. **Inspect and protect.** Read repository instructions, Git status, the source's
   four documents, resources, and existing skill definitions. Read relevant indexes
   and nearby GHCP examples. Resolve repository URLs to the local checkout when they
   name this repository; record its actual revision, not the URL's assumed revision.
   Use authorized read-only retrieval for other sources. Never overwrite the source.
   Reject a destination equal to, inside, or containing the source; also reject paths
   outside `01-scenarios`. If the destination exists, ask whether to update that GHCP
   variant or choose a new name. Preserve unrelated changes.
2. **Verify the actual host and evidence.** Read
   [official sources](references/official-sources.md) and fetch the relevant current
   pages. Record retrieval date and distinguish verified documentation from tenant
   validation and design decisions. If sources cannot be read, mark affected claims
   `[VERIFY]` and block dependent release steps. Do not invent portal settings,
   connector operation IDs, APIs, licensing entitlements, or feature availability.
3. **Inventory behavior, not just labels.** Read
   [translation rules](references/component-mapping.md). Identify the actual source
   harness. A declarative source may have no topics: mark that explicitly. Record
   each source behavior/asset and whether it is retained, re-expressed, deferred,
   or excluded. Identify unsupported promises and missing data paths instead of
   copying them into the new architecture.
4. **Design the capability boundary.** Keep agent-wide policy in instructions,
   retrieval context in knowledge, callable operations in tools, and task-specific
   procedures in skills. Use one skill per independently enabled capability.
   Preserve deterministic validation, authorization, approvals, and transactions
   in the backend/workflow where needed. Do not force a second agent or introduce
   writes merely to demonstrate the harness.
5. **Create the sibling scenario.** Use all four
   [templates](templates/1.Overview.md), following their companion links.
   Preserve the standard filenames. Adapt content, do not recursively copy stale
   screenshots, exports, credentials, or unrelated assets. Copy only necessary
   permitted static resources and repair their links. Add a provenance block with
   source path/link, actual revision, target host, authored date, scope changes,
   and implementation status. Original files and original index entries stay intact.
6. **Write the matrix and runtime skills.** Under `0.Resources`, create a README,
   `Capability-matrix.md`, and `Skills\README.md`. Follow the
   [mapping reference](references/component-mapping.md) for required matrix columns.
   Author `Skills\<capability-id>\SKILL.md` sequentially in matrix order using the
   [runtime template](templates/runtime-skill.md). Folder, YAML `name`, and matrix
   ID must match: lowercase kebab-case, at most 64 characters. Quote descriptions.
   Use only `name` and `description` front matter unless current target docs justify
   other fields. Clearly distinguish configured knowledge retrieval from named
   operation tools. A pure reasoning skill need not invent a tool.
7. **Make delivery actionable.** Include agent creation, global instructions,
   source configuration, tool/auth setup when applicable, skill import, Preview,
   Evaluate, controlled publishing, ownership, and rollback. Link current official
   guidance. Upload runtime `SKILL.md` files via Build > Skills > Upload a skill;
   for supporting assets, package the skill with those assets in a ZIP. Alternatively
   use Create from blank or Generate with AI, then inspect the result. Uploading
   Markdown does not create tools, connections, workflows, or authorization.
8. **Exercise end-to-end behavior.** Give each capability a positive and boundary
   test, plus a composed example with exact evidence/action sequence. Include OFF,
   denied, unavailable, empty, partial, pending, unknown, and failed outcomes where
   relevant. Distinguish instructions to test from tests actually run. Do not
   manufacture citations, screenshots, tool outputs, or deployment success.
9. **Align and verify.** Add the new variant to directly relevant repository indexes
   without replacing the original. Check skill/matrix/index one-to-one coverage,
   front matter, relative links and anchors, balanced code fences, tool dependencies,
   and absence of unresolved template tokens in completed files. `[VERIFY]` and
   `[FILL]` may remain only as explicit owner-assigned deployment gates, never hidden
   in a runtime instruction. Compare source files with the initial state to ensure
   no source changes. Use existing doc tooling; don't install dependencies just for
   Markdown checks.

## Results and Failure Handling

Return the authoring/output paths, meaningful adaptations, and actual outstanding
configuration or evidence gaps. Included definitions are not a deployed agent.
Preserve partial authored work and identify missing files if interrupted. A blocked
integration becomes an explicit OFF capability or a release blocker, not a fabricated
implementation. Do not claim migration parity when behavior or data access differs.

## Example requests

> Use ghcp-scenario-authoring to adapt
> `01-scenarios\IT-Service-Desk-Insights-Agent` into a new `-GHCP` sibling.
> Preserve the original and keep the new scenario read-only.

> Use ghcp-scenario-authoring for `01-scenarios\<existing-scenario>`.
> Map its topics and actions to focused skills, preserving backend approvals.
> Create all four scenario documents and label unimplemented tool contracts.

> Use ghcp-scenario-authoring to create a new read-only procurement-policy scenario
> for Copilot Studio's GitHub Copilot harness using approved SharePoint knowledge.
> Ask about missing scope decisions before writing.
