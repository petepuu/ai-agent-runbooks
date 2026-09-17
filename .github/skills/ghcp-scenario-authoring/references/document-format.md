# Source-scenario document format

## Existing scenarios: source first

The four source documents are the presentation contract. Before drafting, extract
their heading text, heading levels and order, and note metadata blocks, tables,
horizontal rules, diagrams, examples and navigation. Retain that structure in the
GHCP sibling. Preserving filenames alone is not format preservation. Preserve
business structure, not source provenance/history sections or metadata banners.

Use the companion templates as coverage checklists. Do not replace a source outline
with the templates when it already has a different established structure. A new
brief uses the sample-scenario structure in those templates; adapt domain labels
instead of inheriting HR or ServiceNow scope.

Keep the source's heading capitalization, numbering and visual conventions,
including scope checkmarks/crosses where used. Preserve distinct sections even
when their content is brief. Do not substitute "Capabilities and scope" for
"In Scope / Out of Scope", or "Users and outcomes" for separate audience/outcome
sections. Use explicit "Not applicable" for excluded behavior or identify a
concrete setup prerequisite for included behavior; do not silently omit the section.

Host-specific step labels and scenario labels may change when keeping them would
be misleading (for example, a direct agent trigger becomes a Workflows-initiated
agent call). Preserve the numbered step/category pattern and section position,
and record the reason in working context only. Do not copy stale portal
instructions merely to preserve a heading. Additional GHCP details can be nested
under existing sections or linked to Resources. Do not bury the business overview
under the integration contract.

## Independent scenario content

Generated scenarios stand alone. After the breadcrumb and title, start the normal
page content without a target-host/source/revision/authored-date/scope-change
banner. Describe the runtime, capabilities and implementation status in the
relevant business or architecture sections.

Do not include references or links to the original scenario, comparisons with its
agent or steps, source-to-target mappings, preservation statements or asset
provenance inventories. This applies to all generated pages, Resources, capability
matrices and new index descriptions, not just the Overview. Do not move these
references to a footer or Resources. Keep required permitted assets within the
new scenario, or describe independent configuration; make test fixtures
self-contained. Preserve official documentation and knowledge-source citations and any legally required attribution.
Do not publish read/retrieved/reviewed dates, including in Resources or footers;
keep those in working context only. Policy effective dates and meaningful test
or deployment dates are not reading metadata and should remain.

Source inspection, revision tracking and behavior mapping remain internal
authoring steps. They do not become a prerequisite for reading or deploying the
generated scenario.

## Agent name, audience and status

Use one exact agent display name in titles, diagrams, setup/selection steps,
global identity instructions and descriptive links. Keep `-GHCP` only in repository
paths unless it is part of the requested display name. State the harness separately.
Do not rename existing folders or schema identifiers merely to change display text.

Under Scenario Overview use Scenario Type, Harness, Agent Type, Primary Tools,
Complexity and Status rows. Default to `**Harness**: GitHub Copilot` and
`**Status**: 📝 Draft`. End every metadata row except the last with visible `<br>`;
ordinary newlines are soft breaks and render as a single paragraph on GitHub.
Two trailing spaces also work, but are easier to lose when manually editing.

Describe implementation and tenant-readiness requirements in normal prose, not in
a long status field or repeated warning banners. Keep the full approved audience
throughout the scenario, even if the title or a subset skill concerns onboarding.

Include requested capabilities in one standard design by default. Required setup
does not make a capability optional, proposed or OFF. State genuine unsupported
routes as concrete blockers in the runbook without claiming that the design has
been deployed. Do not erase approved read-only or per-action authorization limits.

## Reference layout

The source's business outline and the refined HR Onboarding GHCP scenario illustrate
the expected format. Use these as internal references, never as ancestry links in
generated pages:

| Document | Source structure, in order |
|---|---|
| Overview | Scenario Overview; Problem Statement; Solution Summary with Key Capabilities and How It Works; Business Outcomes; In Scope / Out of Scope with separate In Scope and Out of Scope subsections; Target Users; Knowledge Sources Used |
| Architecture | 1. Logical Architecture with How It Works; 2. Key Components; 3. Data Flow with named scenario sequence diagrams; 4. Security & Governance Considerations; Related Resources |
| Runbook | Overview; Prerequisites; numbered phases for environment, source setup, agent creation, testing and deployment; numbered steps within phases; Summary Checklist; Related Resources |
| Sample prompts | How to Use These Prompts; domain-specific numbered categories with Prompt/Expected Output tables and examples; Tips for Getting Better Responses; Out-of-Scope Topics; Related Resources |

Acceptance fixtures, composed tests and deployment gates are additions,
not replacements for those sections. For another source, preserve that source's
equivalent headings rather than forcing this HR-specific outline onto it.

## Breadcrumbs

Add navigation only to the four main scenario pages (`1.Overview.md`,
`2.Architecture.md`, `3.Runbook.md`, `4.Sample-prompts.md`), not supporting resource
documents, the authoring skill itself or standalone runtime `SKILL.md` files.

Use these exact display labels and order:

`1. Overview > 2. Architecture > 3. Runbook > 4. Sample Prompts`

Make the current page **bold plain text**, with no self-link. Link each other
label to its relative sibling filename. For example, Architecture uses:

```markdown
[1. Overview](1.Overview.md) > **2. Architecture** > [3. Runbook](3.Runbook.md) > [4. Sample Prompts](4.Sample-prompts.md)
```

The breadcrumb must be one line, placed as the **first nonblank line before the
page title**, separated from the title by a blank line. Do not repeat it at the
bottom. This navigation addition is intentional even when the original source
has no breadcrumbs.

End each main page with `## Related Resources` and a two-column Resource/Link table.
List the other three main pages in document order, omitting the current page, then
the capability matrix. Use the row names Scenario Overview, Architecture,
Step-by-Step Runbook and Sample Prompts for the corresponding pages. For example,
Architecture ends with:

```markdown
## Related Resources

| Resource | Link |
| --- | --- |
| Scenario Overview | [Overview](1.Overview.md) |
| Step-by-Step Runbook | [Runbook](3.Runbook.md) |
| Sample Prompts | [Prompts](4.Sample-prompts.md) |
| Capability and Integration Contracts | [Capability matrix](0.Resources/Capability-matrix.md) |
```

Keep source sections intact and reuse an existing Related Resources heading rather
than creating a duplicate. Put official-source commentary and extra resource links
before the final section or in an appropriate earlier section. The table is the
last content on the page; remove redundant inline footer navigation.

## Inline diagrams are required

Use fenced `mermaid` in the Markdown so diagrams render in the same reading surface
as the source. No binary/export is required. Use screenshots only as supplementary
observed evidence; do not restore stale screenshots or generate fake portal captures.

| Location | Required diagram |
|---|---|
| Overview > How It Works | High-level `flowchart TD`: input, named agent, actual retrieval surface, response and included action path |
| Architecture > Logical Architecture > How It Works | `flowchart TB`: 1. User and Event Layer, 2. Agent Layer - Copilot Studio, 3. Data and Integration Layer |
| Architecture > Data Flow | Short `sequenceDiagram` for interactive flow and each included distinct event/action flow |

Keep the logical architecture to about 6-8 nodes. Group skills into one box and
workflow intake/invocation/validation/delivery into one Workflows box instead of separate operation,
outbox, status and queue nodes. Use `direction LR` within layers and explicit
layer placement: users and event channels in layer 1, the agent/instructions/skills
in layer 2, and Workflows alongside knowledge/index and source systems in layer 3.
Use
`USER --> AGENT` and `AGENT --> DATA` ordering. Keep the workflow round trip explicit:
event channel -> Workflows -> agent -> same Workflows -> response channel -> user.
Do not combine the event channel with the workflow or hide the agent's return
behind a separate delivery box. Omit other redundant edges when needed. When rendered
layout is available, confirm the three layers actually appear top-to-bottom.

Sequence diagrams should normally have around five participants and 6-8 messages,
showing the routine successful path. For autonomous email: employee -> mailbox ->
Workflows -> agent -> indexed knowledge -> result -> validated reply -> employee.
Do not draw every backend operation or nested `alt` tree. Summarize exceptional
cases in a short paragraph and link the detailed matrix/runbook. Add complexity
only where the business flow genuinely requires it, not to prove control coverage.
These are authoring targets, not platform limits.

Match the sample's restrained Microsoft palette: blue inputs (`#0078D4`), green
agent/skills (`#107C10`), orange knowledge/tools (`#E07000`), purple outputs
(`#5C2D91`), and light neutral layer containers. Keep labels short, use quoted node
labels and `<br/>` for line breaks, and label edges with their data or action.
Use solid edges and the normal palette for included flowchart paths. Only
genuinely deferred alternatives need neutral styling and explicit labels.
Sequence return arrows may remain dashed; that does not mean a capability is OFF.

In sequence-diagram message and note text, use commas or plain wording instead
of literal semicolons: Mermaid treats semicolons as statement separators and can
parse the remaining label as a new, invalid statement. If a semicolon is necessary,
encode it as `#59;`. Check labels as well as balanced fences before delivery.

Represent skills as procedures used by the agent, never services receiving API
calls. Retrieval goes to the actual configured knowledge surface, not to a
fictional search tool. For ServiceNow connector-backed knowledge, show ingestion
from ServiceNow via the Copilot connector into M365 search/semantic index and
runtime retrieval from that index, with no direct agent-to-ServiceNow arrow.
Use the same retrieval label in Overview and all sequence diagrams. For live APIs
or other source types, draw their verified route instead.

Draw writes only when in scope. A concise "Validates and sends authorized reply"
edge can represent the integration boundary; describe exact controls in the matrix.
Workflows invoking an agent and sending its validated result is not the agent
calling a send tool. Routine autonomous replies do not gain a human-approval step
unless the approved policy requires one. Keep separate setup and deployment facts
in normal prose rather than repeating them in every node.

Do not invent a fixed model, successful send, native email channel, automatic
topic conversion or connected agent for a diagram. Retain exact action ordering
in deterministic workflows where required, not in an assumed skill-selection order.

## Verification

Compare source and target outlines before finishing. Check every original section
with business content is represented in the same relative order at the appropriate
heading level; exclude source-history/provenance sections and record justified
host-specific substitutions in working context. Check table schemas and numbered
step/category conventions, not just the existence of the four filenames.

For each main page, verify exactly one breadcrumb at the beginning, with one bold
current-page label and three valid relative sibling links. Verify all four labels
and their order, not merely the presence of a `>` separator. Verify exactly one
final Related Resources section with the other three page links and the capability
matrix, and no bottom breadcrumb or self-link. Do not add this navigation to
Resources or runtime skills.

Verify that the delivered scenario and its index descriptions have no source-
scenario references, cross-scenario asset dependencies or opening metadata
banners. Do not mistake citations to operational knowledge or official product
documentation for references to the original scenario.

Check at least one Mermaid flowchart under each How It Works heading and the
named Data Flow sequences. Check balanced fences, node/participant references,
linked assets, and correspondence between each diagram and the standard configuration,
identity boundary, tool contract and failure behavior. Check the exact agent name,
three-layer order, simplicity targets and no direct-source retrieval shortcuts.
Check metadata hard breaks and the Draft icon, and distinguish operator-paused
negative tests from included-by-default capability status. Render using available
local documentation tooling or the editor preview when possible; never claim
rendering was checked if only source syntax was inspected. Do not install a new
diagram tool solely for Markdown validation.
