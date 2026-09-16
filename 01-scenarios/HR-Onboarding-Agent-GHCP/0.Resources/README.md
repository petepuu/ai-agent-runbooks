# HR Onboarding Agent (GHCP) - Resources

## Contents and implementation status

| Artifact | Purpose |
|---|---|
| [Capability matrix](Capability-matrix.md) | Four capabilities, profiles, dependencies and proposed operation contracts |
| [Skill index](Skills/README.md) | Standalone Studio upload files in matrix order |
| [Runbook](../3.Runbook.md) | Agent instructions, configuration, owner-assigned gates and release procedure |
| [Acceptance cases](../4.Sample-prompts.md) | Evidence and action expectations, not executed test results |

These files are authoring deliverables, not a solution package. No connector, tool, workflow or approval is created by importing a skill. No runtime skill links to repository-only supporting files.

## Knowledge and test resources

HR supplies approved onboarding, handbook, benefits and wellness articles through the configured ServiceNow knowledge source. The [runbook](../3.Runbook.md#phase-1-servicenow-knowledge-base-setup) covers connection setup, source scope, citations and permissions. Production HR articles and tenant configuration are not included.

The [controlled fixtures](../4.Sample-prompts.md#controlled-fixtures) contain the complete synthetic text needed for onboarding, wellness and conflicting-leave tests. Load these only into an isolated test knowledge source and record its actual article IDs/URLs; fixture labels are not live citations. Email tests additionally require the proposed backend and its test context.

## Knowledge quality requirements

For each production policy, record authority, population, country/entity/category, effective period, owner and permission rules. If permitted evidence conflicts and lacks a priority or supersession rule, preserve the uncertainty rather than choose an entitlement.

Fixtures F3 and F4 intentionally state **15** and **20 vacation days** for the same test cohort without priority metadata. They test conflict handling, not company policy. Fixture F2 omits monthly medical premiums; an answer must say that the value is unspecified rather than claim it is "Included." Prompt coverage does not establish evidence for personal pay dates, bank-detail setup, leave approval or IT provisioning.

**Gate G2, HR owner:** resolve authoritative production policy or exclude conflicting topics from pilot scope. Keep conflict fixtures in the isolated test corpus. Do not copy synthetic policy facts, contact addresses, credentials or provider recommendations into runtime instructions.

## Official-source verification

"Documented" is evidence about product guidance, not proof that a specific tenant is configured or licensed. The [runbook gates](../3.Runbook.md#release-gates) are authoritative for owner-assigned deployment checks.

| Official source | Documented claim read | Remaining tenant check / owner |
|---|---|---|
| [Harnesses](https://learn.microsoft.com/en-us/microsoft-copilot-studio/harnesses-overview) | GHCP, standard and Copilot chat are different runtimes | G1: correct experience / delivery owner |
| [GHCP overview](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/overview) | Instructions, knowledge, tools, skills, model and memory; no cross-harness transfer; build/test/evaluate may consume credits | G1: environment, approved model, credits and memory / tenant and cost owners |
| [Create a new agent](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/build-new-agent) | Home > Agent or Agents > New agent; name/settings/instructions before save | G1: actual authoring access and schema name / delivery owner |
| [Skills overview](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/skills-overview) | Description-driven activation and progressive loading; skills are not tools | G4: matching and non-matching traces / delivery owner |
| [Create skills](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/skills-create) | Create from blank or Generate with AI; inspect before adding | G4: imported names, descriptions and bodies / delivery owner |
| [Upload existing skills](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/skills-add-existing) | Build > Skills > Add skill > Upload a skill; Markdown front matter or ZIP with SKILL.md | G4: import validation / delivery owner |
| [Add knowledge](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/knowledge-add-existing-copilot) | Build > Knowledge supports ServiceNow; no general-knowledge toggle | G2-G3: select actual connection, test source limits and ACLs / source and identity owners |
| [ServiceNow Knowledge deployment](https://learn.microsoft.com/en-us/microsoft-365/copilot/connectors/servicenow-knowledge-deployment) | Permissions, identity mapping, Simple/Advanced criteria, HRSD warning, AccessUrl and distinct sync types | G2-G3: scoped connector, HR criteria and revocation / ServiceNow and M365 admins |
| [Add tools](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/add-tools-custom-agent) | Connector action selection; custom MCP and workflow routes | E1: actual email operations and execution identities / integration owner |
| [Evaluate](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/analytics-agent-evaluation-intro) | Test conversations, version and user profile; General quality does not compare expected answers | G4: deterministic acceptance review in addition to scores / test owner |
| [Publish overview](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/publication-fundamentals-publish-channels) | Draft/live separation, publish states, channel distribution and republishing | G5: approved version and audience / release owner |
| [Available channels](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/publication-channels-overview) | Teams and M365 Copilot available; native Email unavailable | G5: actual channel identity/rendering; E1: separate event integration / tenant and integration owners |

An attempted GHCP `agents-experience/triggers-overview` documentation lookup returned HTTP 404. No event-to-agent invocation claim is marked verified on that basis. E1 blocks the proposed intake integration until the integration owner records a current supported route and exercises it; a native Email channel is not a fallback.

**Design choices:** one agent, baseline read-only, memory off initially, four independent skills, narrow reviewed-email extension and no unattended sending. **Proposed contracts:** the three email operations in the matrix. **Deployment values and tenant gates:** only in the runbook; no unresolved placeholders are embedded in runtime skills.

[Overview](../1.Overview.md) | [Architecture](../2.Architecture.md) | [Runbook](../3.Runbook.md) | [Prompts](../4.Sample-prompts.md)
