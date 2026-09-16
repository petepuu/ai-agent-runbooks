# HR Onboarding Agent (GHCP) - Resources

## Contents and implementation status

| Artifact | Purpose |
|---|---|
| [Capability matrix](Capability-matrix.md) | Four capabilities, profiles, dependencies and proposed operation contracts |
| [Skill index](Skills/README.md) | Standalone Studio upload files in matrix order |
| [Runbook](../3.Runbook.md) | Agent instructions, configuration, owner-assigned gates and release procedure |
| [Acceptance cases](../4.Sample-prompts.md) | Evidence and action expectations, not executed test results |

These files are authoring deliverables, not a solution package. No connector, tool, workflow or approval is created by importing a skill. No runtime skill links to repository-only supporting files.

## Provenance and asset disposition

Source: `01-scenarios\HR-Onboarding-Agent`, repository `petepuu/ai-agent-runbooks`, revision `697593082501641fd9adf31381c328533d6989ce`, inspected 2026-09-16. This revision includes the user's deletion of the older GHCP folder; this adaptation was authored anew rather than restoring that folder. The original source has four Markdown scenario documents, a connector setup guide, two sample documents and 101 PNG screenshots; no runtime skills or executable topic/flow exports were present.

The four source documents, connector guide and sample document text were read for behavior and evidence inventory. Screenshots were inventoried but not revalidated as current UI evidence. No binary or screenshot is copied into this sibling.

| Original asset | Use in this adaptation |
|---|---|
| [Source overview](../../HR-Onboarding-Agent/1.Overview.md) and [architecture](../../HR-Onboarding-Agent/2.Architecture.md) | Business scope and source component inventory |
| [Source runbook](../../HR-Onboarding-Agent/3.Runbook.md) and [prompts](../../HR-Onboarding-Agent/4.Sample-prompts.md) | Actual knowledge, email and onboarding behavior; sample outputs are not evidence |
| [Connector setup guide](../../HR-Onboarding-Agent/0.Resources/Sample-documents/ServiceNow-Tenant-%26-Copilot-Connector-Setup-Guide.md) | Historical context only; do not follow demo MFA skip, admin/Everyone access or old availability claims |
| [Contoso Employee Handbook](../../HR-Onboarding-Agent/0.Resources/Sample-documents/Contoso%20Employee%20Handbook.docx) | Optional synthetic demonstration source; remains in original |
| [Wellness Benefits Sample Knowledge](../../HR-Onboarding-Agent/0.Resources/Sample-documents/Wellness%20Benefits%20Sample%20Knowledge.docx) | Optional synthetic demonstration source; remains in original |
| Original Images directory | Excluded from target; no claims of fresh screenshots or observed deployment |

Links to original documents are optional provenance/sample references, not prerequisites for importing the standalone skills. For production, HR supplies approved articles through the configured knowledge source.

## Source quality findings

The sample handbook says **15 vacation days**, whereas the wellness document says **20 days annually, prorated by start date**. The handbook says **13 national paid holidays** but enumerates ten; the wellness document says **10 company-recognized holidays**. Neither sample provides enough authority/applicability metadata to resolve these differences.

The source's sample prompt table labels medical/dental/vision cost as "Included"; the wellness document instead gives plan cost tiers, not exact monthly premiums. The source's wide prompt coverage does not establish evidence for individual first-paycheck dates, bank-detail setup, leave approval or IT provisioning.

**Gate G2, HR owner:** resolve authoritative production policy by country/entity/category and effective period, or exclude conflicting topics from pilot scope. Do not edit the originals as part of this adaptation. Keep the conflicting samples only in an isolated test corpus if testing abstention. Do not copy sample policy facts, contact addresses, credentials or provider recommendations into runtime instructions.

## Official-source verification

All successful reads below were retrieved on **2026-09-16**. "Documented" is evidence about product guidance, not proof that a specific tenant is configured or licensed. The [runbook gates](../3.Runbook.md#release-gates) are authoritative for owner-assigned deployment checks.

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

An attempted GHCP `agents-experience/triggers-overview` documentation lookup returned HTTP 404 on 2026-09-16. No event-to-agent invocation claim is marked verified on that basis. E1 blocks the proposed intake integration until the integration owner records a current supported route and exercises it; a native Email channel is not a fallback.

**Design choices:** one agent, baseline read-only, memory off initially, four independent skills, narrow reviewed-email extension and no unattended sending. **Proposed contracts:** the three email operations in the matrix. **Deployment values and tenant gates:** only in the runbook; no unresolved placeholders are embedded in runtime skills.

[Overview](../1.Overview.md) | [Architecture](../2.Architecture.md) | [Runbook](../3.Runbook.md) | [Prompts](../4.Sample-prompts.md)
