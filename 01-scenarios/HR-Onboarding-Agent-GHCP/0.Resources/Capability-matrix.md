# Employee HR capability matrix

**Implementation status:** Both standalone runtime skill definitions belong to one standard scenario with interactive chat and autonomous user-mailbox entry paths. No executable workflow, backend, queue, connector configuration or tenant deployment is supplied. Operators implement and test these dependencies during setup, using isolated mocks before an explicitly authorized real-mail pilot. Included by default describes the design, not a claim of deployment.

## Capability inventory

The current [downloadable skills](Skills/README.md) replace the former folder-based definitions. The email attachment returns HTML directly. The version 2 JSON schema and backend contracts below describe the earlier design and require adaptation before they can be used with this replacement; they are not implemented by uploading these files.

| Capability ID / linked Markdown file | Behavior and boundary | Tools or knowledge | Actual implementation / operation ID | Dependencies | Standard entry path / access / approval |
|---|---|---|---|---|---|
| [hr-onboarding-checklist](Skills/hr-onboarding-checklist.md) | Numbered onboarding checklist grouped into phases, placeholders for unknown specifics | Supplied new-hire details, no tools or retrieval required | Supplied Markdown attachment | Role/start/location/equipment details when known | Chat only; no email composition or task writes |
| [hr-email-reply](Skills/hr-email-reply.md) | Format supplied ServiceNow article content as a short HTML email with a source table | Already retrieved ServiceNow article text, KB numbers and links | Supplied Markdown attachment; HTML output, not JSON | Agent retrieves applicable knowledge before composition; workflow owns sending | Workflow email preparation, no mailbox tool installed by this file |

## Standard configuration and entry-path boundaries

Ordinary HR policy answers and follow-up conversation are baseline agent behavior governed by global instructions and configured knowledge, not separately imported skills. They retain source, privacy and no-send boundaries.

Import both skills into the same HR agent. **Interactive chat** uses authenticated employee access to approved knowledge. Employees can ask ongoing HR questions, request onboarding guidance and continue with contextual follow-ups. Respond conversationally, not with email subjects, sign-offs or workflow JSON. Chat cannot create an authenticated email event or initiate sending.

**Autonomous email** uses trusted connected user-mailbox events calling `hr-email-reply`. Workflows invokes the same published GHCP agent and waits for its result, then deterministic backend logic validates and sends. The agent has **no send, queue or mailbox tools** on either entry path. The reply skill's independent capability is workflow reply preparation; consequential operations belong to the surrounding integration, not additional hidden skills.

Release owners approve the audience, low-risk topics, corpus, policy version and operational limits once per approved release/change. **Routine messages require no human approval step or `approvalId`.** Cases outside those limits are held for normal HR handling, not automatically mailed by the model. A chat "send this" never creates trusted intake.

Excluded actions are arbitrary recipients, Reply All, CC/BCC, attachments, forwarding, inbox enumeration, mailbox drafts, message edits/deletes, personal HR records, medical data, case creation and HR transactions. These are scope boundaries, not claimed Studio feature switches.

For an explicit maintenance pause or rollback, operator disablement must update skills and global scope, revoke server policy authorization, stop new intake/submission, block alternate paths, and reject old workflow/agent versions and stale sessions. Keep scoped operator status/reconciliation access and shared knowledge needed by enabled capabilities. An in-flight provider attempt cannot be recalled by disabling a skill.

## Knowledge dependency

HR owns authority, applicability, effective periods, conflict resolution and approved knowledge. M365 / ServiceNow administrators own ingestion, identity mapping, article/base ACLs and refresh behavior.

The [ServiceNow Knowledge Copilot connector](https://learn.microsoft.com/en-us/microsoft-365/copilot/connectors/servicenow-knowledge-overview) ingests KB articles and permission metadata into Microsoft 365. The agent retrieves indexed article information through its configured knowledge source using M365 search/semantic index; it does not query ServiceNow directly. Connector ingestion and runtime retrieval are separate paths, and index freshness depends on synchronization.

Chat retrieval uses the authenticated employee's validated permissions. **Workflow/application/connection-owner retrieval does not inherit the email sender's rights.** Use an explicitly HR-approved audience-safe corpus for the mapped employee population, or enforce recipient-entitlement filtering **before retrieval** and revalidate every source before send. A broad service result cannot be made safe merely by appending citations. If recipient-safe retrieval or current send-time entitlement cannot be enforced, hold for HR review; do not expose restricted evidence to the agent or recipient.

The agent writes a natural-language answer from current approved evidence; no response-block catalog is required. The workflow's versioned request rules limit routine unattended replies to approved low-risk topics and populations. Unknown wording, uncovered questions, sensitive content or ambiguous applicability is held. Model confidence cannot grant send permission.

The email starts with a greeting, uses readable paragraphs or lists with numbered citations, then includes a **References** table (**#**, **Source**, **Link**) containing every source used, deduplicated in citation order. It ends with **Kind regards,** and **HR Onboarding Agent**. The workflow independently validates factual support, full question coverage, source access and HTML before authorizing the exact payload. Schema-valid HTML and valid citations alone do not prove the answer is correct; unvalidated content must be held. Content-validation implementation and its acceptance evidence are release requirements, not capabilities delivered by this repository.

## Documented product operations and call direction

| Operation / surface | Direction and evidence | Scenario use / remaining gate |
|---|---|---|
| Office 365 Outlook **When a new email arrives** | Connected user-mailbox event → workflow, [connector reference](https://learn.microsoft.com/en-us/connectors/office365/) | Qualify connected account/folder, Subject filter **HR question** and event behavior in E1; trigger alone is not sender authentication; no shared mailbox |
| Workflows **Agent** node, **An existing agent**, **Message** | Workflow → published agent → returned response, [GHCP Workflows guidance](https://learn.microsoft.com/en-us/microsoft-copilot-studio/workflows-experience/agent-node-workflow#choose-an-existing-agent-for-the-agent-node) | Documented inbound pattern, verify exact published HR GHCP selection, skill execution and identity during E1 setup, no node operation ID invented |
| **When an agent calls the flow** / **Respond to the agent** | Agent → workflow tool → agent, [tool guidance](https://learn.microsoft.com/en-us/microsoft-copilot-studio/workflows-experience/flow-agent) | Opposite direction, not the HR incoming-email invocation path |
| Outlook **Reply to email (V3)**, `ReplyToV3` | Deterministic backend/workflow → provider, [connector reference](https://learn.microsoft.com/en-us/connectors/office365/#reply-to-email-%28v3%29) | Candidate transport only, bind `messageId`, `mailboxAddress`, exact single `To`, `ReplyAll=false`, no CC/BCC or attachments, qualify status/reconciliation in E1-E2 |

The connector catalog documents `ExecuteCopilot` and `ExecuteCopilotAsyncV2`, but does not establish GHCP-target compatibility. Neither these names, standard-harness trigger/SDK instructions nor workflow-as-tool support are a substitute for the E1 inbound evidence. See [invocation finding](README.md#workflow-invocation-finding).

## Email integration contracts

The following are **scenario-defined interface contracts implemented during setup, not delivered endpoints or verified built-in operation IDs**. All are called by the authenticated deterministic workflow/backend or scoped operator, **not by the agent**. The integration owner records concrete implementations, schemas, connection ownership and authentication as part of standard setup. No vendor API or assumed REST/MCP mapping is invented.

### Common envelope and enforcement

Authenticate workload callers independently of request text. Accept only the configured tenant, workflow version, mailbox and operation scope. Use separate least-privilege ingestion, invocation, validation, send and operator identities as appropriate. The Office 365 Outlook connector currently documents no service-principal authentication; use an approved delegated connection where that connector is chosen, not a fictional app-only option. Any alternative application-based transport requires separate supported implementation and permission evidence.

Every response includes `status`, `correlationId`, `observedAt` (timestamp with timezone), and display-safe `reasonCode` for non-success. Reject unknown fields, malformed types, expired references and unauthorized context lookups. Status fields absent or unparseable mean **unknown**, not allowed or sent. Neither caller-supplied context IDs nor a model-generated `complete` state confer authorization.

### AcceptHrEmailEvent

| Contract area | Requirement |
|---|---|
| Inputs | `eventRef`, `workflowVersion`, provided through authenticated configured intake, not a pasted message |
| Authoritative validation | Resolve original tenant/mailbox/provider message and version, transport-authenticated sender, employee directory mapping and intended single recipient, reject forwarded/untrusted From or Reply-To redirection, external/unmapped senders, automated mail, bounces, loops, attachments, protected/unreadable or oversized messages |
| Success outputs | `status=accepted`, `contextId`, `messageVersion`, `recipientBindingId`, sanitized subject/inquiry, permitted applicability, `policyVersion`, `requestKeys`, `retrievalScopeId`, permitted topics and response language, existing `operationId` and `replyState` if any |
| Scope / identity | One HR inquiry, integration-owned authenticated intake, no general mailbox browsing or employee HR record access |
| Concurrency / idempotency | Atomically deduplicate by tenant/mailbox/stable provider message identity, not merely event run ID, return same context/existing attempt for duplicate events, preserve immutable version and fail on conflicting changes |
| Other outcomes / async | `held`, `denied`, `not_found`, `unavailable`, `failed`, `unknown`, no send job, missing deterministic request match holds for HR |

Intake checks attachment metadata, not just an empty attachment array: Outlook Dynamic Delivery can trigger more than once before content arrives. Do not download/process attachments. When message stability cannot be established, hold. Configure inbound age limits, sender/mailbox rate limits, bounce/auto-response suppression and human-reply detection. If an HR member already handled the inquiry or concurrent manual activity cannot be excluded, reserve it for manual handling instead.

### Agent reply result

`hr-email-reply` returns a JSON object with exactly these fields. This is the scenario-defined application schema, not a promise that an existing-agent node exposes the inline agent's custom structured-output selector. Implement parsing and validation of the returned result server-side during setup.

| Field | Type / meaning |
|---|---|
| `schemaVersion` | String, `2` |
| `contextId`, `messageVersion`, `policyVersion` | Strings copied from the trusted invocation envelope, verified against server records |
| `answerState` | `complete` or `hold`, advisory only |
| `bodyHtml` | Complete HTML email with greeting, grounded answer, references table and agent sign-off; empty string for a hold |
| `evidenceRefs` | Array of objects with string `sourceId`, `title`, `url` and string or null `sourceVersion`; actual permitted evidence used, empty for a hold |
| `holdReasons` | Array of safe strings, empty for a complete reply; at least one reason for a hold, no protected details |

No recipient, mailbox, authorization, send command or eligibility boolean is accepted in this result. Unknown/invented source references fail closed. `complete` is necessary but not sufficient for sending. Version 1 block-selection results are not compatible: update the agent, parser and validator together before enabling version 2.

### ValidateHrEmailReply

| Contract area | Requirement |
|---|---|
| Inputs | `contextId`, `expectedMessageVersion`, `agentResult` using the schema above |
| Validation | Current enabled policy/workflow/agent version and required capability IDs, authenticated context, full inquiry coverage, low-risk topic and population, factual support for all claims, source authority/currency/effective period, non-conflict, recipient entitlement, no personal/sensitive information, no unresolved gaps, schema and size limits. Resolve null source versions against authoritative freshness evidence or hold |
| Canonical result | Validate and sanitize `bodyHtml` server-side with an HTML parser. Allow only `p`, `br`, `strong`, `h3`, `ul`, `ol`, `li`, `table`, `thead`, `tbody`, `tr`, `th`, `td`, `a`; only `href` attributes with verified HTTPS source URLs. Reject scripts, styles, images, forms, event handlers, tracking and unapproved links. Require one reference row per evidence entry, matching citation numbers, source titles and URLs, followed by the agent sign-off. Bind the sanitized body only after content validation; derive the reply subject from the trusted original message |
| Success outputs | `status=authorized`, server-issued `authorizationId`, `validatedResultId`, `contextId`, `messageVersion`, `recipientBindingId`, `policyVersion`, `payloadDigest`, `evidenceDigest`, `expiresAt` |
| Binding | Persist immutable canonical subject/rendered body, recipient, mailbox, original message/version, source versions, policy version, digests and expiry under these references |
| Identity / owner | Authenticated workflow only, HR owns policy/knowledge, security/identity owners own current recipient checks, agent cannot mint authorization |
| Concurrency / idempotency | Repeat same candidate against same current state returns same validation or a new bounded authorization record without sending, invalidate on any changed binding or policy revocation |
| Other outcomes / async | `held`, `denied`, `conflict`, `unavailable`, `failed`, `unknown`, no send job, no fallback "authorized" object |

### SubmitHrEmailReply

| Contract area | Requirement |
|---|---|
| Inputs | `contextId`, `expectedMessageVersion`, `authorizationId`, `validatedResultId`, stable backend-generated `idempotencyKey` |
| Scope | Send one stored validated reply from the bound connected user mailbox to the bound employee, no caller-supplied recipient/body or alternate headers |
| Identity / owner | Narrow integration send authorization, connection owned by mailbox integration owner, never exposed as an agent tool |
| Last-moment checks | Revalidate enabled policy/version, authorization expiry and digests, context/message version, source currency and recipient entitlement, manual handling/reply state and rate limits, fail closed if checks unavailable |
| Concurrency | Atomically reserve one durable reply operation per inbound message across keys, workflow versions, retries and manual-handling claims, persist attempt before provider call |
| Idempotency | Same key/bindings returns existing operation, changed bindings with same key gives `conflict`, different keys cannot create a second reply for the same inbound message |
| Outputs | `operationId`, `status` (`pending`, `sent`, `denied`, `conflict`, `failed`, `unknown`), `payloadDigest`, provider reference and `sentAt` only when confirmed |
| Async / reconciliation | Lost or ambiguous provider result becomes `unknown`, no automatic retry until provider evidence establishes a definitive non-send and backend grants a safe retry under current authorization |

**Exactly-once is not supplied by a connector.** An atomic outbox prevents duplicate local attempts, not an unknown remote side effect. E2 must prove provider-side reconciliation using a durable correlation/receipt or another supported, narrowly scoped mechanism. Do not assume `ReplyToV3` returns a sent-message ID or honors a custom idempotency header. If that transport cannot reconcile an uncertain outcome, keep it unknown and prevent resend; operational inability to safely meet required reliability blocks release. No blind connector retry after a timeout.

### GetHrEmailReplyStatus

| Contract area | Requirement |
|---|---|
| Inputs | `contextId`, optional `operationId`, context-only lookup recovers a lost submission response |
| Outputs | Current `replyState` (`not_submitted`, `pending`, `sent`, `failed`, `unknown`), actual operation ID and payload digest when present, confirmed provider reference/time if available, backend `retryAfterSeconds` / `retryAllowed` only when established |
| Scope / identity | Authorized workflow or scoped operator, no general mail tracking, operator access can survive send disablement |
| Concurrency / idempotency | Read-only durable state, repeated reads never send, `not_submitted` is not itself retry permission |
| Failure / async | Envelope status distinguishes `ok`, `denied`, `not_found`, `unavailable`, `failed`, `unknown`; a failed status lookup does not overwrite a known pending attempt, provider-confirmed sent is not recipient delivery |

### QueueHrEmailException

| Contract area | Requirement |
|---|---|
| Inputs | Authorized `contextId` or trusted intake `eventRef` when no context exists, safe `reasonCode`, optional `operationId` |
| Scope / identity | Workflow/backend to a fixed restricted HR exception queue, not arbitrary email or an HRIS case, no agent-selected destination or generated outbound explanation |
| Persisted record | Event/context reference, safe reason, current attempt state, assigned HR handling group, timestamps and minimal authorized evidence references, full body omitted unless retention policy explicitly permits |
| Success / failure | `status=queued` with real `queueItemId`, otherwise `pending`, `denied`, `unavailable`, `failed`, `unknown`, never claim handoff without confirmed queue persistence |
| Concurrency / idempotency | Upsert one item per event/context, serialize manual claim with autonomous reservation, pending/unknown provider attempts cannot be manually resent until reconciled |
| Failure route / owner | Persist a durable failed-handoff/dead-letter record and alert the integration operator through an approved operational mechanism, retain original mailbox item for normal HR handling, no auto-reply fallback; queue/alert implementation is an E1-E2 blocker |

Exception handling is an operational requirement of this single reply capability, not a separately enabled general notification/case-creation skill. HR handles exceptions through its ordinary authorized process. It cannot override denied recipient access or unresolved provider uncertainty.

## Integration acceptance and ownership

E1 requires exact HR GHCP invocation and identity evidence, concrete implementations for all five backend contracts, response parsing, delegated mailbox permissions and the fixed exception queue. E2 requires deterministic request/corpus validation, server authorization, revocation, payload binding, rate/loop controls, duplicate/concurrent-event and manual-claim tests, and provider reconciliation. E3 requires release-level authorization and restricted live-mail pilot evidence, retention, monitoring and an exercised kill switch. G1-G5 cover environment, knowledge, chat acceptance and publishing. These are required deployment checks for the whole standard scenario, with D1-D3 recording its configuration. All remain to be completed by the deployment team in the [runbook](../3.Runbook.md#release-gates).

[Resources](README.md) | [Skill import index](Skills/README.md) | [Architecture](../2.Architecture.md)
