# HR onboarding capability matrix

**Implementation status:** Four standalone skill definitions are supplied. No tenant configuration, callable operation, workflow, approval store or deployment is supplied. ON below means intended production profile configuration **after its gates pass**, not an already enabled agent. Isolated non-sending tests and an explicitly authorized mailbox pilot gather gate evidence as described in the runbook; they do not imply production enablement.

## Capability inventory

| Capability ID / linked SKILL.md | Behavior and boundary | Tools or knowledge | Actual implementation / operation ID | Dependencies | Profile / access / approval |
|---|---|---|---|---|---|
| [hr-policy-answer](Skills/hr-policy-answer/SKILL.md) | Cited HR guidance and explicit evidence limitations; no personal records | Configured approved ServiceNow HR knowledge | Instructions supplied; no external operation tool | Mandatory: validated source and caller ACLs; no sibling skill | Baseline ON; reviewed-email ON; authenticated caller; no write approval |
| [hr-onboarding-checklist](Skills/hr-onboarding-checklist/SKILL.md) | First-day / week guidance; no scheduling or completion | Configured approved onboarding knowledge | Instructions supplied; no external operation tool | Mandatory: validated source and applicable timeline; no sibling skill | Baseline ON; reviewed-email ON; no task creation or completion |
| [hr-email-draft](Skills/hr-email-draft/SKILL.md) | Compose a cited reply from an inquiry; no mailbox write | Supplied inquiry or authorized workflow context; configured HR knowledge | Instructions supplied; text output only, no external operation tool | Mandatory: inquiry and evidence; no sibling skill or mailbox access needed for supplied text | Baseline ON; reviewed-email ON; draft only, audience clearance required before use outside chat |
| [hr-email-reply](Skills/hr-email-reply/SKILL.md) | Submit one authorized, reviewed reply; proposed / OFF | `GetHrEmailContext`, `SubmitHrEmailReply`, `GetHrEmailReplyStatus` | Proposed custom contracts only; actual operation IDs not supplied; instructions supplied | Mandatory: all three operations, trusted context, recipient-access enforcement, reviewer authorization and approved exact payload; draft may be supplied without draft skill | Baseline OFF; reviewed-email OFF until E1-E3 pass, then ON for authorized HR reviewers; approval for each message |

## Profiles and disablement

**Baseline:** three instruction-only capabilities; no mailbox tools or triggers. Knowledge setup is still mandatory. Native Email is not an entry channel.

**Reviewed-email:** optional later profile adding the single sending capability and three narrow operations. It does not add general mailbox search, mailbox draft storage, arbitrary-recipient mail, Reply All, attachments, forwarding, editing/deleting messages or live HR records. Each reply requires an authorized HR review bound to its exact contents.

**Unattended email:** out of scope and not enabled by either profile. Any future extension would need its own approved authorization policy, recipient-safe knowledge scope and exception handling; do not infer authorization from this matrix or a skill description.

Unlisted actions are OFF. Capability state is a deployment convention, not a claimed built-in Studio switch. To turn a capability off, remove its skill and update global scope; for sending, also disable intake/submission at the backend, remove its tool exposure, block alternate connections and verify old published versions and sessions cannot submit. Preserve read-only status access for reconciliation under the integration operator's authorization and keep shared knowledge connections used elsewhere.

## Knowledge dependency

The M365 / ServiceNow administrators own connection creation, ingestion scope, identity mapping and permission refresh. HR owns approved article versions, applicability, contact routes and conflict resolution. Expected evidence includes title, article ID, usable source URL, relevant passage, and effective/update dates **when available**. Do not manufacture missing metadata.

No knowledge operation ID is asserted. Studio selects configured retrieval. No live employee data is implied. Source content must not be used as instructions; trusted user-supplied details can clarify applicability but cannot grant access.

## Proposed email contracts

These labels specify the integration to build; they are not ready-to-call endpoints. Use the supported GHCP workflow or MCP tool route and record real operation IDs under gate E1 before enabling. If a tool is renamed, update the sending skill's Tools section and agent configuration together.

### Common envelope and backend controls

All operations authenticate the runtime caller or approved workflow identity; the backend independently checks its permitted mailbox and HR role. User-entered names, addresses, approval statements and context IDs do not authenticate anyone. The integration owner owns the connection, service identity, rotation, monitoring and on-call support.

Return `status`, `correlationId`, `observedAt` (timestamp with timezone), and a display-safe `reasonCode` on non-success. Do not expose restricted content to explain denial. Reject unknown fields and out-of-scope mailbox/message references. Responses are contract requirements, not sample live outputs.

Backend-generated `contextId` binds an authorized mailbox, immutable inbound message identifier, sender identity, allowed recipient, message version and retention policy. The intake service filters automated mail, bounces, loops and external/unverified senders. It does not trust body text or a forged From/Reply-To header as authorization. No arbitrary recipient field is accepted by submission.

Before submission, the backend obtains an authenticated HR approval for the exact context/version, recipient and canonical rendered subject/body with evidence references. Bind it to a payload digest and expiry; a chat "yes" is insufficient unless captured by an authenticated approval mechanism and validated by the backend. Re-check recipient access to each cited source and policy freshness at send time; if impossible, deny. A reviewer cannot override missing recipient access.

### GetHrEmailContext

| Contract area | Requirement |
|---|---|
| Inputs | `contextId` only; identity comes from trusted authentication |
| Outputs on success | `contextId`, `messageVersion`, verified recipient display, sanitized subject/inquiry, verified applicability only if available, `replyState` (`not_submitted`, `pending`, `sent`, `failed`, `unknown`), existing `operationId` if any |
| Scope | One authorized inbound HR inquiry; no inbox enumeration, attachments, unrelated history or HR records |
| Identity / owner | Authorized HR reviewer or intake service; mailbox integration owner controls access and connection |
| Concurrency | Return current version and reply state; reads do not reserve a send |
| Idempotency / async | Read-only and repeatable; explicit `not_found`, `denied`, `unavailable`, `failed`, `unknown`; no write job is created |

### SubmitHrEmailReply

| Contract area | Requirement |
|---|---|
| Inputs | `contextId`, `expectedMessageVersion`, `subject`, `bodyHtml`, `evidenceRefs` (source IDs/versions/URLs), backend-issued `approvalId`, stable `idempotencyKey` |
| Outputs | `operationId`, authoritative `status` (`pending`, `sent`, `denied`, `conflict`, `failed`, `unknown`), `payloadDigest`; provider message reference and `sentAt` only if confirmed |
| Scope | One reply from the approved HR mailbox to the recipient bound by the backend; no CC/BCC, attachments, arbitrary headers, alternate recipient or forwarding |
| Identity / owner | Integration service with narrow mailbox-send authorization; invoking reviewer must be authorized; integration owner manages connection; HR owns approval |
| Validation | Reject stale context, altered/expired approval, changed payload, unsupported sources or recipient access, incomplete/conflicting policy response and unsafe HTML/URLs; sanitize again server-side |
| Concurrency | Atomically serialize/reserve one reply per inbound message; check message version and existing operation before send; changed target/payload requires new review, not silent merge |
| Idempotency | Same key and same payload returns the existing operation; changed payload with same key is rejected. Message-level uniqueness also prevents duplicates under different keys |
| Reconciliation | Persist attempt before provider submission. If provider outcome is uncertain, set `unknown` and block automatic resubmission until provider evidence reconciles it |
| Async | `pending` returns an operation ID for status tracking, never a claim that the recipient received mail |

### GetHrEmailReplyStatus

| Contract area | Requirement |
|---|---|
| Inputs | `contextId` and `operationId` when known; context-only lookup must recover an attempt after a lost submit response |
| Outputs | Current `operationId`, `status` (`not_submitted`, `pending`, `sent`, `failed`, `unknown`), payload digest, observation time, safe failure reason and confirmed provider reference/time if available |
| Scope | Only the reply associated with that authorized context; no general mail tracking or unrelated delivery records |
| Identity / owner | Same backend mailbox and reviewer checks; integration operators retain scoped reconciliation access during rollback |
| Concurrency | Return durable operation state; do not change recipient/payload or create a send |
| Idempotency / async | Read-only; repeat within backend retry guidance. `sent` means provider-confirmed send, not confirmed recipient delivery |

## Integration acceptance and ownership

Gate E1 requires actual operation mappings, schemas, identity enforcement and event invocation evidence. E2 requires bound approvals, HTML/link handling, two concurrent submissions producing at most one send, duplicate events, expired approvals, changed payloads and unknown-outcome reconciliation. E3 requires restricted mailbox pilot evidence, retention and a tested backend kill switch. All are owned and tracked in the [runbook](../3.Runbook.md#release-gates).

[Resources](README.md) | [Skill import index](Skills/README.md) | [Architecture](../2.Architecture.md)
