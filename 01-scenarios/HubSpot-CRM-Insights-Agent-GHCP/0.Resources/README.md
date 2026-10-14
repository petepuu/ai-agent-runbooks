# HubSpot CRM Insights Agent — Resources

## Contents and implementation status

| Folder / file | Purpose |
|---|---|
| [Capability-matrix.md](Capability-matrix.md) | One included capability, the MCP tool allowlist, the identity and connection ownership, the OAuth configuration contract and the limits |
| [Skills/README.md](Skills/README.md) | Runtime import, tool dependencies and upload notes |
| `Skills/<capability-id>/SKILL.md` | Standalone skill instructions, not tools or connection installers |
| [Sample Prompts](../4.Sample-prompts.md) | Acceptance cases, with every result recorded as Not run |

Delivered assets are documentation and a runtime definition for development and test. No deployed agent, HubSpot connector, connection, credential, real CRM data or successful tenant test evidence is included. Keep operational evidence and environment values in the organization's access-controlled delivery record, and never store a client secret, token, cookie or customer record in this repository.

Build in this order: the HubSpot developer account and sample records ([Runbook Phase 1](../3.Runbook.md#phase-1-hubspot-developer-account-and-sample-data)), then the agent and its instructions ([Phase 2](../3.Runbook.md#phase-2-agent-creation-in-copilot-studio)), then the MCP server, its OAuth connection and the tool allowlist, then the skill, then the tests. Uploading a skill does not create tools, connections or authorization. Test before you share the agent.

## Release gates

Each gate is closed by its named role before dependent behavior is enabled. These are implementation requirements, not optional capabilities. Four checks are independent: OAuth completes, the tool catalog is non-empty, the identity is the expected one, and the data matches HubSpot. Passing one does not prove the others.

| Gate | Owner | Required evidence / acceptance |
|---|---|---|
| G1 — Host availability | Delivery engineer / Power Platform admin | [VERIFY] The environment offers the agent card for the GitHub Copilot harness. The created agent has Build, Preview, Evaluate and Monitor, and Build shows Tools and Skills. An agent created with the other harness is not accepted, because agents cannot be transferred between harnesses. A named, approved model is recorded |
| G2 — Connector, OAuth and tool discovery | Delivery engineer / HubSpot account owner | [VERIFY] The Add MCP server form accepts the **Manual** OAuth fields in the [OAuth contract](Capability-matrix.md#oauth-configuration-contract). The generated callback is registered in HubSpot, authorization completes and a connection exists. The tool catalog is non-empty. `tools: []` with errors is not a pass. The field labels of the preview form are recorded as they appear |
| G3 — Read-only enforcement | Delivery engineer / security reviewer | [VERIFY] **Enable all tools** is off. Only the [allowlist](Capability-matrix.md#agent-tool-contracts) is enabled, and the state is confirmed by saving and reopening the configuration. No `manage_*`, `submit_feedback` or other non-allowlisted tool is enabled. A write request in Preview is refused and the trace shows no write tool call. Mutation tests are Not run until this gate is closed |
| G4 — Identity and sharing | Security reviewer / HubSpot account owner | [VERIFY] `get_user_details` returns the expected user and account. The connection owner, the authentication mode (**User**) and the sharing scope are recorded. Per-user access is shown only with two different identities, or the agent stays private and no security trimming is claimed. The HubSpot user has only the read permissions the use case needs |
| G5 — Data and answer correctness | Test owner | [VERIFY] A named company's details match its HubSpot record. Associated contacts and deals match. A nonexistent name gives "no accessible match". Totals use a supported aggregate or a complete bounded retrieval with the pagination, filters and currency stated. Activities and conversations are labeled unavailable when HubSpot Sensitive Data is on |
| G6 — Business configuration | Sales operations owner | [FILL] The target users, the decision a briefing supports, the success metric, the HubSpot account, the approved sample or real data scope, the pipeline and stage definitions, the account currency and timezone, and whether Sensitive Data is enabled |
| G7 — Test evidence | Test / privacy owner | [VERIFY] The [smoke path](../4.Sample-prompts.md#smoke-path) passes first, then every case in [Sample Prompts](../4.Sample-prompts.md#acceptance-cases) is run in isolation, then the composed run, with the result and activity-trace evidence recorded. With **End user preview** off, the trace shows a **Skill** node for `hubspot-crm-read` and Tool nodes for the HubSpot calls |
| G8 — Cost and capacity | Release / cost owner | [VERIFY] Copilot Credits for build, test, evaluation and runtime are budgeted. A free HubSpot developer account does not make the solution free. HubSpot rate limits are acceptable for the expected use |
| G9 — ALM and rollback | Power Platform engineer | [VERIFY] The instruction and skill versions are traceable. The retirement steps in [Runbook Step 4-3](../3.Runbook.md#step-4-3-retire-the-agent-and-connection) are rehearsed. The HubSpot grant, the connection and the connector registration have named owners |

Do not reduce a gate to an instruction telling the model to be careful. The read-only boundary is the disabled tool list. Instructions and the skill are an additional layer only.

## Official-source verification

**Documented** means the linked guidance supports the stated product behavior. **Design choice** and **scenario-defined contract** describe this scenario's required implementation. Neither establishes tenant availability or successful execution.

| Source | Documented claim used | Tenant check / owner |
|---|---|---|
| [Harnesses](https://learn.microsoft.com/en-us/microsoft-copilot-studio/harnesses-overview) and [agents powered by the GitHub Copilot harness](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/overview) | The harness organizes an agent around instructions, tools and skills, with Build, Preview, Evaluate and Monitor. Agents cannot be transferred between harnesses. Usage is billed in Copilot Credits | G1/G8, delivery engineer |
| [Create an agent](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/authoring-first-bot) | The agent is created from the card for the GitHub Copilot harness on the Home page. **Other ways to build** is the route for the standard harness | G1, delivery engineer |
| [Configure the agent's details and instructions](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/authoring-instructions) | Agent-wide instructions are edited in Build | G1, delivery engineer |
| [Add an MCP server](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/tools-add-mcp-server) | Build > Tools > Add > Model Context Protocol. The form takes a name, a description, the server URL and the authentication. Adding runs the handshake and lists the tools. The feature is in preview and has limits on MCP servers and tools | G2/G3, confirm the form fields and limits |
| [Manage tools](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/tools-manage) | Tools of an MCP server can be turned on and off individually. Removing a tool does not delete the underlying connector or MCP server | G3, delivery engineer |
| [Create a skill](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/skills-create) and [upload a skill](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/skills-add-existing) | Create from blank, Generate with AI, or upload a Markdown file with YAML name and description, or a ZIP. The name uses lowercase letters, numbers and hyphens | G1, delivery engineer |
| [Skills overview](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/skills-overview) | Skills carry task procedures. They do not create tools or connections | G1, delivery engineer |
| [Activity trace](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/authoring-activity-trace) | With **End user preview** off, the trace shows node types such as Skill and Tool nodes | G7, test owner |
| [Preview](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/preview-overview) and [Evaluate](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/analytics-agent-evaluation-intro) | Preview shows the agent's tool calls. Evaluate scores a test set | G7, add explicit checks against HubSpot records |
| [Share an agent](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/authoring-share-agent) and [publish](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/publication-fundamentals-publish-channels) | Sharing and publishing widen who can use the agent and its connection | G4/G9, security reviewer |
| [Credits and capacity](https://learn.microsoft.com/en-us/power-platform/admin/manage-copilot-studio-copilot-credits-capacity) | Monitoring and allocating Copilot Credits | G8, release / cost owner |
| [HubSpot remote MCP server](https://developers.hubspot.com/docs/apps/developer-platform/build-apps/integrate-with-the-remote-hubspot-mcp-server) and [overview](https://developers.hubspot.com/docs/build-with-ai/remote-mcp-server) | The endpoint `https://mcp.hubspot.com`. OAuth with PKCE. The tool catalog, including the read tools and the write-capable tools. Calls respect the HubSpot user's permissions. Connector permissions come from the tools available and the permissions granted. Activities and conversations are unavailable with Sensitive Data on. CRM search, not vector search. `get_user_details` verifies the connection | G2/G3/G4/G5, HubSpot account owner |
| [HubSpot OAuth metadata](https://mcp.hubspot.com/.well-known/oauth-authorization-server) | The authorization endpoint, the token endpoint, an empty `scopes_supported`, `client_secret_post` and S256 | G2, delivery engineer |
| [HubSpot account types](https://developers.hubspot.com/docs/getting-started/account-types) | The kinds of HubSpot accounts, including developer accounts | G6, HubSpot account owner |

## Troubleshooting by layer

Diagnose from the bottom of the stack up, and change only the layer that the evidence points at.

| Layer | Symptom | Check and response |
|---|---|---|
| Authoring access | The environment is missing from the picker or the harness card is missing | Ask the administrator for the minimum authoring and connection permissions. Do not use a standard-harness agent as a substitute |
| Connection access | Discovery fails with a missing connection access rule from the Power Platform connection service | Power Platform denied access to the connection. Use a correctly owned connection. Do not change HubSpot scopes or callbacks |
| OAuth: client | Invalid client | Use the client ID and secret of the HubSpot MCP connector, not those of another app |
| OAuth: redirect | Redirect mismatch | Register the exact callback that Copilot Studio generated, including a trailing slash and any literal encoded sequences. Inspect only the `redirect_uri` parameter and never share the whole authorization URL |
| OAuth: token | Token request fails, PKCE or scope error | Use the MCP-specific token URL. The connection must send `client_secret_post` and S256. Leave Scopes empty |
| OAuth: account | Sign-in used the wrong account | Sign in again and select the intended developer account, with the same Microsoft account as the agent |
| OAuth: expiry | The connection works, then fails later | Check the Refresh token URL value and reconnect |
| Routing / MCP | Bare 404, or discovery returns no tools | A 404 means the source is not established: inspect the request path, confirm the root server URL and that the connection belongs to the current definition. A 401 points to the token or endpoints, a 403 to the account or permissions, and a 200 with no tools to a protocol or catalog problem |
| Harness | No Skills panel on Build | The agent uses the wrong harness. Create the correct type |
| CRM permissions | A known record is not found | The user may lack access. Check the permissions of the authenticated HubSpot user |
| Data | No sample records, or a missing section | Check the account and the filters, whether Sensitive Data blocks activities and conversations, and whether tickets are accessible |
| Behavior: totals | A wrong total | Check pagination, filters and currency. A first page is not the population |
| Behavior: associations | Guessed contacts or a wrong company | The agent must take associations only from returned data. Tighten the test and the instructions |
| Behavior: skill | The skill never runs | Inspect the trace for a Skill node. Check the skill name and description, and that the instructions are saved |
| Behavior: writes | The agent tries a write tool | Disable the individual tool and confirm the toggle state after saving. A refusal in text is not enough |
| Behavior: freshness | The answer repeats an earlier response | The instructions require a new retrieval for current facts. Re-test with a changed record |
| Rate limit | Calls fail with a rate limit | Wait and reduce the request size. Do not loop |

### Escalation package

When a platform defect or a service problem needs escalation, collect the environment ID, the agent, connector and connection identifiers, the failing operation, a timestamp with its timezone, the outer and inner status codes, the correlation ID and a sanitized screenshot. Exclude tokens, cookies, secrets, HAR files and customer payloads.

## Design choices and operational records

- One agent, one skill, one MCP server. The agent is interactive. It has no workflow, no trigger and no email. This is a **scenario-defined contract**.
- The HubSpot MCP server is hosted and defined by HubSpot. This scenario does not wrap or proxy it. Its catalog includes write-capable tools, so enabling only the allowlist is a required configuration, not a courtesy.
- Free-text CRM fields are untrusted. A note that says "ignore previous instructions" is a record value, not a command. Record text cannot choose endpoints, change the identity or request outbound messages.
- The first acceptance target is a private Preview conversation. Sharing and channels come after the identity and policy decisions.
- Extensions are separate designs with their own gates: support briefings from tickets, per-user deployment shown with two identities, controlled writes with a separate capability and a confirmation, broader analytics through governed reporting, and additional channels. None of them is part of this scenario.
- Record the HubSpot account, the connector registration name, the connection owner, the tool allowlist state, the instruction and skill versions, the approved model, the test runs and the release approvals in a restricted delivery record. Store no credentials or customer data in this repository.
