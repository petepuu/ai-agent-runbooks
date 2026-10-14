# HubSpot CRM Insights Agent — Runtime Skills

The standard configuration has one skill. Import it after the HubSpot MCP server is added and before testing CRM questions. The file is standalone: it names the MCP tools it depends on, and the agent's instructions route CRM questions to it.

| Order | Capability / file | Purpose | Required tools |
|---|---|---|---|
| 1 | [hubspot-crm-read](hubspot-crm-read/SKILL.md) | Resolve the HubSpot account and user, discover the schema, find and read companies, contacts, deals, tickets and permitted activities, and answer with record names and IDs, stating filters, currency, missing data and limits | HubSpot MCP tools `get_user_details`, `discover_hubspot_schema`, `search_properties`, `get_properties`, `search_crm_objects`, `get_crm_objects` and `tool_guidance`. `search_owners` when available. `query_crm_data` and `get_organization_details` optionally |

The tools are the real tools of HubSpot's remote MCP server, enabled one by one in **Build > Tools**. They are listed with their limits in the [Capability and Integration Contracts](../Capability-matrix.md#agent-tool-contracts). The agent has no write, email or approval tool, and no workflow. The read-only boundary comes from disabling every tool that is not on the allowlist. The skill and the agent instructions add a behavioral layer on top of that, and they cannot replace it.

## Skill inputs

| Skill | Runs when | Inputs |
|---|---|---|
| hubspot-crm-read | The user asks about HubSpot records: a company, contact, deal or ticket, a customer briefing, a bounded pipeline summary or the data in the account | The question and, when the user gives them, a company or deal name, filters, a time range and the wanted output. The skill resolves records through the tools and takes no identifier from a name |

Relative dates need verified current-date and timezone context, and "my records" is resolved through `search_owners`. Without either, the skill asks the user instead of guessing.

## Import

Upload `SKILL.md` in **Build > Skills > Upload a skill**. No ZIP is needed because the file has no extra assets. You can also use Create from blank or Generate with AI and then replace the generated instructions with the content of the file. Inspect the name, the quoted description and the imported instructions afterward. Paste only the instructions, never the surrounding explanation or code fences. Uploading a skill does not create a tool, connection or authorization.

The skill contains no account names, URLs of a particular tenant, owner names or credentials. Everything that depends on the deployment lives in the agent instructions and in the [Runbook](../../3.Runbook.md).

For an operational pause, remove the HubSpot MCP tool from the agent or unpublish and unshare the agent. Removing the skill alone does not revoke the HubSpot connection or the MCP tools. See the [retirement steps](../../3.Runbook.md#step-4-3-retire-the-agent-and-connection).

## Quick behavior checks

After import, confirm in Preview with **End user preview** off:

1. The activity trace shows a **Skill** node for `hubspot-crm-read` and Tool nodes for the HubSpot calls. A sentence such as "I used the skill" is not evidence.
2. A request to create or change a record is refused and no write tool is called.
3. A question about a name that does not exist returns "no accessible match", not an invented record.

Use the [capability tests and the composed test](../../4.Sample-prompts.md#capability-tests) before sharing the agent.
