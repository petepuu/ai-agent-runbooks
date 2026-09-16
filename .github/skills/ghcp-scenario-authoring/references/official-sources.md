# Official sources and evidence rules

Reviewed **2026-09-16**. Refresh relevant pages during each authoring task. These are
source-backed observations, not a guarantee of availability in a particular tenant.
Microsoft Learn is authoritative for the Studio host; GitHub documentation below
only establishes where the repository authoring skill is discovered.

| Source | Verified guidance | Authoring consequence |
|---|---|---|
| [Harnesses](https://learn.microsoft.com/en-us/microsoft-copilot-studio/harnesses-overview) | GitHub Copilot, standard, and Copilot chat are distinct harnesses | Identify the source accurately; don't label every older agent standard-harness |
| [GHCP overview](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/overview) | Instructions, knowledge, tools, skills, connected agents, model and memory; no transfer between standard and GHCP harnesses | Create a new agent/side-by-side adaptation, not an in-place conversion |
| [Create an agent](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/build-new-agent) | Home > Agent or Agents > New agent; name, instructions, save, then add components | Document a new Studio build, not a renamed exported solution |
| [Skills overview](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/skills-overview) | Skills package task-specific instructions/resources and load progressively based on purpose | A skill is neither a deterministic topic trigger nor an external service connection |
| [Create a skill](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/skills-create) | Build > Skills; Create from blank or Generate with AI; names use lowercase letters, numbers, hyphens | Supply activation descriptions, procedures, edge cases and response formats |
| [Upload skills](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/skills-add-existing) | Markdown with YAML name/description, or ZIP containing SKILL.md plus optional assets | Standalone runtime definitions can be uploaded one by one |
| [Tools overview](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/tools-overview) | Tools call APIs/workflows and obtain real-time data; conversational use needs no explicit topic flow per call | Keep tool contracts separate from skill instructions |
| [Add tools](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/add-tools-custom-agent) | Tool picker includes connectors, MCP and workflows; custom-tool routes include MCP and workflows | Verify selected operations and execution identity; don't invent a REST-import path |
| [Add knowledge](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/knowledge-add-existing-copilot) | Build > Knowledge supports source selection including ServiceNow and SharePoint; no general-knowledge toggle | Verify individual connections and enforce grounding through instructions and tests, not an invented toggle |
| [GitHub Agent Skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills) | Repository skills can live under .github/skills | The coding assistant discovers the authoring skill here; Studio still needs its own imports |

## Verification record for each generated scenario

Record the source URL, claim it supports, tenant check still needed, and responsible
role. Track reviewed/retrieved dates in working context only, not generated scenario
documents or Resources. Keep references close to consequential setup steps. Label:

- **Documented:** supported by a source actually read.
- **Design choice:** a boundary or convention selected for this scenario.
- **Proposed contract:** an integration interface without delivered executable code.
- **Tenant gate:** `[VERIFY]` with an owner and acceptance criterion.
- **Deployment value:** `[FILL]` with an owner, kept out of runtime skill definitions.

Do not carry exact prices, product versions, crawl intervals, channel availability,
preview/GA claims, connector privileges, or legacy licensing statements from an old
scenario without rechecking them. Current overview pages mark some natural-language
authoring features as preview and describe usage-based Copilot Credit consumption
for building, testing and evaluating as well as use. Check the linked current
billing guidance for any more specific claim.

Microsoft documentation may mix shared standard-harness pages with `agents-experience`
pages. Prefer the latter for GHCP authoring steps, and explicitly verify discrepancies
in the target environment. If a page is unavailable, retain the link but do not call
its claims freshly verified.
