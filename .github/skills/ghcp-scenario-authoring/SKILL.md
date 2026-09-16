---
name: ghcp-scenario-authoring
description: "Create a Copilot Studio GitHub Copilot harness scenario while preserving the source scenario's document format, section order and Mermaid diagrams. Produce Overview, Architecture, Runbook, Sample-prompts and runtime skills; preserve the original and keep skills separate from tools."
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
   Read [document format](references/document-format.md) and extract the source's
   heading outline, levels, order, metadata, tables, separators and diagram locations
   before drafting. The source scenario, not a generic summary, is the formatting
   contract. Reuse that outline for all four target documents.
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
5. **Create the sibling scenario.** Read all four templates:
   [Overview](templates/1.Overview.md), [Architecture](templates/2.Architecture.md),
   [Runbook](templates/3.Runbook.md), and [Sample prompts](templates/4.Sample-prompts.md).
   For adaptations, preserve the source's section names, hierarchy, order, table
   format and visual style; use the templates as content checklists, not replacement
   outlines. For new briefs, use their sample-scenario layout. Never collapse
   `In Scope / Out of Scope` into a generic capability paragraph or replace
   `Problem Statement`, `Solution Summary`, `Business Outcomes`, `Target Users`
   and `Knowledge Sources Used` with a shorter overview. Keep explicit In Scope
   and Out of Scope subsections. Place GHCP provenance near the title and extra
   mapping/contracts/gates under appropriate existing sections or in Resources.
   Add the four-page breadcrumb as the first nonblank line of each main
   scenario document: `1. Overview > 2. Architecture > 3. Runbook > 4. Sample Prompts`.
   Bold the current page without a self-link; link the other three to their sibling
   Markdown files. Do not repeat the breadcrumb at the bottom. End each main page
   with `## Related Resources` and a Resource/Link table linking to the other three
   main pages (no self-link), followed by Capability and Integration Contracts
   linking to `0.Resources/Capability-matrix.md`.
   This applies only to the four main pages, not resource documents or runtime
   `SKILL.md` files. Follow [document format](references/document-format.md#breadcrumbs)
   for exact labels, links and placement.
   Preserve the standard filenames. Adapt content, do not recursively copy stale
   screenshots, exports, credentials, or unrelated assets. Copy only necessary
   permitted static resources and repair their links. Add a provenance block with
   source path/link, actual revision, target host, authored date, scope changes,
   and implementation status. Original files and original index entries stay intact.
   Generate inline fenced Mermaid diagrams in Overview's `How It Works` and
   Architecture's `How It Works`, plus sequence diagrams under Architecture's
   `Data Flow`, following the source's style and locations. Do not replace these
   with prose, a table, an external diagram link or a copied screenshot. Show the
   actual GHCP components, skills, knowledge, tools and outputs; label proposed,
   OFF and tenant-gated paths in the diagrams themselves. No invented integrations.
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
   Markdown checks. Compare the source/target heading outlines for all four files:
   preserve source sections and their relative order; explain necessary host/scope
   label changes and additions. Explicitly check Overview's scope subsections,
   both `How It Works` Mermaid blocks, Architecture's data-flow diagrams, and that
   diagram edges/statuses agree with the capability matrix. A missing source section
   or required diagram is an authoring defect, not a stylistic simplification.
   Check the single top breadcrumb on every main page: exact order and labels,
   one bold current page and three valid relative sibling links. Verify the final
   Related Resources table links to the other three pages and capability matrix,
   with no trailing breadcrumb.

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
