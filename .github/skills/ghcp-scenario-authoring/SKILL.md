---
name: ghcp-scenario-authoring
description: "Create an independent Copilot Studio GitHub Copilot harness scenario using an existing scenario's document format, section order and Mermaid style. Produce standalone Overview, Architecture, Runbook, Sample-prompts and runtime skills without source-scenario references or provenance banners; preserve original files and keep skills separate from tools."
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
- Exact agent display name, distinct from the `-GHCP` repository folder name.
- Any approved differences from the source.

Read established answers from the conversation and repository before asking.
For new briefs, collect the problem, users, evidence sources, independently enabled
actions, and exclusions. Do not inherit ServiceNow or ticket-analysis rules by default.
Use the full approved audience consistently in personas, diagrams, instructions
and tests. A subset use case does not narrow the whole audience: for an
all-employee HR brief, onboarding is one use case, not a new-hire-only scope.

## Procedure

1. **Inspect and protect.** Read repository instructions, Git status, the source's
   four documents, resources, and existing skill definitions. Read relevant indexes
   and nearby GHCP examples. Use the current
   `01-scenarios\HR-Onboarding-Agent-GHCP` as the internal presentation reference
   for the conventions below, not as a source of mandatory HR features.
   Resolve repository URLs to the local checkout when they
   name this repository; record its actual revision in working context only, not
   in the generated scenario.
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
   pages. Record retrieval dates in working context only and distinguish verified
   documentation from tenant validation and design decisions. If sources cannot
   be read, mark affected claims
   `[VERIFY]` and block dependent release steps. Do not invent portal settings,
   connector operation IDs, APIs, licensing entitlements, or feature availability.
3. **Inventory behavior, not just labels.** Read
   [translation rules](references/component-mapping.md). Identify the actual source
   harness. A declarative source may have no topics: note that in working context. Record
   each source behavior/asset and whether it is retained, re-expressed, deferred,
   or excluded for internal authoring analysis only. Publish the resulting
   capabilities and boundaries, not a source-to-target comparison. Identify
   unsupported promises and missing data paths instead of copying them into the
   new architecture.
4. **Design the capability boundary.** Keep agent-wide policy in instructions,
   retrieval context in knowledge, callable operations in tools, and task-specific
   procedures in skills. Use one skill per independently enabled capability.
   Preserve deterministic validation, authorization, approvals, and transactions
   in the backend/workflow where needed. Do not force a second agent or introduce
   writes merely to demonstrate the harness.
   Use one standard configuration containing the requested capabilities by default.
   Do not turn an in-scope autonomous workflow into an optional, proposed or OFF
   extension simply because the documentation does not deploy it. Separate design
   inclusion from implementation status and required setup/go-live checks. Create
   multiple profiles only when requested or actually required by the approved scope.
   For requested autonomy, verify event -> Workflows -> existing agent -> same
   Workflows -> validated action -> user. Draw one Workflows node with explicit
   agent-call and response-return edges, separate from the event channel.
   Do not substitute agent-calls-workflow tools for inbound
   invocation. Retain the approved authorization policy; do not silently add
   per-message human approval to routine autonomous processing. Unsupported or
   uncertain cases need explicit exception handling. Never claim an unsupported
   route works: name the concrete setup blocker in the Runbook/Resources.
   For indexed knowledge, distinguish source -> connector -> index ingestion from
   agent -> configured knowledge/index retrieval. ServiceNow Knowledge Copilot
   connector scenarios retrieve through M365 search/semantic index, not a direct
   agent-to-ServiceNow connection. Do not impose this path on other retrieval types.
5. **Create the sibling scenario.** Read all four templates:
   [Overview](templates/1.Overview.md), [Architecture](templates/2.Architecture.md),
   [Runbook](templates/3.Runbook.md), and [Sample prompts](templates/4.Sample-prompts.md).
   For adaptations, preserve the source's section names, hierarchy, order, table
   format and visual style; use the templates as content checklists, not replacement
   outlines. For new briefs, use their sample-scenario layout. Never collapse
   `In Scope / Out of Scope` into a generic capability paragraph or replace
   `Problem Statement`, `Solution Summary`, `Business Outcomes`, `Target Users`
   and `Knowledge Sources Used` with a shorter overview. Keep explicit In Scope
   and Out of Scope subsections. Write the result as a completely independent
   scenario, not a migration report. Do not add an opening metadata/provenance
   block (target host, source, revision, authored/retrieval date, scope changes,
   or original-preservation statement). Describe the host, current scope and
   implementation status in the normal business/architecture sections and put
   contracts/gates under appropriate existing sections or in Resources.
   Use the exact agent display name in page titles, diagrams, creation/selection
   steps, runtime identity instructions and references. Do not append GHCP or
   (GHCP) to the name unless requested; keep harness information in its own field.
   Default new scenario metadata to `**Status**: 📝 Draft` and
   `**Harness**: GitHub Copilot`. Use explicit `<br>` after metadata rows except the
   final row so GitHub does not collapse soft line breaks. Keep the `-GHCP` folder.
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
   permitted static resources into the new scenario and repair their links.
   Do not reference the original scenario, its files, revision, agent, sample
   assets or migration history anywhere in generated pages, resources, matrices
   or new index descriptions. Required resources must be local to the new scenario
   or independently configured; examples must be self-contained. Do not relocate
   removed provenance to Resources. Keep official product/knowledge-source
   citations and legally required attribution; those are distinct from scenario
   ancestry. Omit read/retrieved/reviewed dates from all generated pages and
   resources; retain policy effective dates and meaningful test/deployment dates.
   Original files and original index entries stay intact.
   Generate inline fenced Mermaid diagrams in Overview's `How It Works` and
   Architecture's `How It Works`, plus sequence diagrams under Architecture's
   `Data Flow`, following the source's style and locations. Do not replace these
   with prose, a table, an external diagram link or a copied screenshot. Show the
   actual components at a business-readable level. Use three stacked architecture
   layers (1. User and Event, 2. Agent, 3. Data and Integration), typically 6-8
   nodes. Sequence diagrams show the routine path, typically five participants
   and 6-8 messages, not nested validation/retry trees. Put operation schemas,
   approval/authorization details, queue and retry branches in the matrix/runbook
   and link them below. Standard capabilities use normal solid flowchart styling;
   reserve deferred/disabled labels for genuinely excluded or operator-paused
   behavior, not ordinary setup requirements. No invented integrations.
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
   test, plus a composed example with exact evidence/action sequence. Include operator-paused,
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
   preserve business sections and their relative order, excluding source
   provenance/history sections; record necessary host/scope label changes in
   working context only. Explicitly check Overview's scope subsections,
   both `How It Works` Mermaid blocks, Architecture's data-flow diagrams, and that
   diagram edges/statuses agree with the capability matrix. A missing source section
   or required diagram is an authoring defect, not a stylistic simplification.
   Check the single top breadcrumb on every main page: exact order and labels,
   one bold current page and three valid relative sibling links. Verify the final
   Related Resources table links to the other three pages and capability matrix,
   with no trailing breadcrumb. Check all generated documents and new index
   descriptions for source-scenario links/names/revisions, migration comparisons,
   asset dependencies and opening metadata banners; remove them without losing
   current scope, implementation status, safety boundaries or official citations.
   Check exact agent-name consistency, full-audience coverage, Draft/Harness
   metadata with hard breaks, simple diagram node/message counts and all requested
   capabilities in the standard configuration. Check ingestion vs retrieval and
   workflow invocation direction in every diagram, not just the architecture.
   Ensure no contradictory optional/OFF defaults remain in templates, skill indexes,
   runtime procedures, tests or repository descriptions. Approved read-only scopes
   remain read-only; these conventions do not add email or writes to every scenario.

## Results and Failure Handling

Return the authoring/output paths, meaningful adaptations, and actual outstanding
configuration or evidence gaps. Included definitions are not a deployed agent.
Preserve partial authored work and identify missing files if interrupted. A blocked
integration becomes an explicit setup/release blocker, not a fabricated
implementation or a silently reduced scenario scope. Mark a capability deferred
only when that is the agreed scope. Do not claim migration parity when behavior
or data access differs.

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
