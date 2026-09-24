# Capability and Integration Contracts

## Implementation status

All ten capabilities below are included in the standard **Copilot Licence Lifecycle
Agent** configuration. The runtime skill definitions are authored; **no MCP server,
backend code, flow export, Dataverse schema or tenant connection is delivered**.
The `lifecycle_*` names are scenario-defined tool contracts to implement during
setup, not Microsoft operation IDs. Missing implementation blocks execution,
not inclusion in the design.

The standard entry is authenticated licence-team chat. Power Automate schedules
perform deterministic synchronization, detection and approved campaign processing;
they do not invoke the agent or need another skill. Employees and department
managers participate through existing request/contact routes and the licence team.

## Capability matrix

| Capability ID / linked SKILL.md | Behavior and boundary | Tools or knowledge | Actual implementation / operation ID | Dependencies | Standard inclusion / access / authorization |
|---|---|---|---|---|---|
| [copilot-licence-inventory](Skills/copilot-licence-inventory/SKILL.md) | Inventory, departments, capacity and known assignment dates; no writes | `lifecycle_inventory` | Scenario-defined read adapter over Graph and Dataverse | SKU mapping, complete directory paging, assignment provenance | Included; licence-team caller |
| [copilot-usage-insight](Skills/copilot-usage-insight/SKILL.md) | Usage, dormancy candidates, trends and reclaim history; no detection writes or speculation | `lifecycle_usage` | Scenario-defined report adapter and snapshot queries | Usage identity mapping, freshness, policy, exclusions, retained history | Included; licence-team caller; individual data never shared outside group |
| [copilot-waitlist-status](Skills/copilot-waitlist-status/SKILL.md) | Queue, request status and historical waits; no guaranteed allocation date | `lifecycle_waitlist` | Scenario-defined scoped Dataverse read | Approved queue policy and entry history | Included; administrator can relay only appropriately scoped status |
| [copilot-waitlist-intake](Skills/copilot-waitlist-intake/SKILL.md) | Add one verified request without approving or assigning | `lifecycle_context`, `lifecycle_prepare`, `lifecycle_waitlist_add`, `lifecycle_status` | Scenario-defined intake handler | Verified user/request reference, duplicate check, confirmation | Included; licence-team caller acting for a documented requester |
| [copilot-waitlist-review](Skills/copilot-waitlist-review/SKILL.md) | Approve, reject or reprioritize requests under queue policy; no licence assignment | `lifecycle_context`, `lifecycle_prepare`, `lifecycle_waitlist_review`, `lifecycle_status` | Scenario-defined allowlisted decision handler | Reviewer role, reason, row versions, policy limits | Included; authorized queue reviewer, exact-payload confirmation |
| [copilot-dormancy-notify](Skills/copilot-dormancy-notify/SKILL.md) | Initial warning and scheduled reminder campaign for at most 25 named holders | `lifecycle_context`, `lifecycle_prepare`, `lifecycle_notify`, `lifecycle_status` | Scenario-defined wrapper over backend Teams/Outlook actions | Eligible case, approved templates, recipients, grace rule, campaign approval | Included; confirmed campaign with bounded recipients and reminder offsets |
| [copilot-licence-reclaim](Skills/copilot-licence-reclaim/SKILL.md) | Remove only eligible direct Copilot assignments; no group edits or automatic reassignment | `lifecycle_context`, `lifecycle_prepare`, `lifecycle_reclaim`, `lifecycle_status` | Scenario-defined handler using Graph `POST /users/{id}/assignLicense` | Completed grace, latest evidence, exclusion check, approval, effective-state readback | Included; authorized licence approver, maximum 25 targets |
| [copilot-licence-assign](Skills/copilot-licence-assign/SKILL.md) | Assign to the next approved eligible request and send its confirmation | `lifecycle_context`, `lifecycle_prepare`, `lifecycle_assign`, `lifecycle_status` | Scenario-defined handler using Graph `POST /users/{id}/assignLicense` | Queue lock, capacity, eligibility, exact recipient/SKU approval | Included; authorized licence approver; assignment and notice states returned separately |
| [copilot-reclaim-cancel](Skills/copilot-reclaim-cancel/SKILL.md) | Cancel an open case with a reason; never restore a removed licence | `lifecycle_context`, `lifecycle_prepare`, `lifecycle_cancel`, `lifecycle_status` | Scenario-defined case transition | Case version, no unresolved in-flight removal, confirmation | Included; licence-team caller with case-maintenance permission |
| [copilot-reclaim-dispute](Skills/copilot-reclaim-dispute/SKILL.md) | Record a dispute and route it to the configured escalation owner; no automatic reversal | `lifecycle_context`, `lifecycle_prepare`, `lifecycle_dispute`, `lifecycle_status` | Scenario-defined case handler and notification routing | Source request reference, reason, trusted contact, confirmation | Included; licence-team caller, minimal dispute details, no medical information |

## Five-table storage contract

Use only `LicenseHolder`, `WaitlistEntry`, `ReclaimCase`, `Configuration` and
`AuditEvent`, with the 26 base columns and five lookup columns listed in
[Architecture](../2.Architecture.md#data-model-dataverse): 31 custom columns.
No other custom columns, relationships, alternate keys or auxiliary tables
are part of this model.
Use existing listed text columns as primary names and native Dataverse record
IDs/ETags as system metadata.

The required `ReclaimCase.LicenseHolder` and `ReclaimCase.Configuration` lookups
identify the affected holder and the selected settings. The optional audit lookups
`LicenseHolder`, `WaitlistEntry` and `ReclaimCase` correlate business history.
All use Restrict Delete and no other supported cascade actions. Do not make
waitlist intake depend on an existing holder. Resolve missing historical required
links through reviewed backfill; never fabricate a holder or attach today's
configuration to an older case without evidence.

Validate the case's UserId against its holder, deployment scope against connected
records, and consistency when multiple audit links are present. Assignment audit
links the fulfilled request and resulting holder when both exist; case actions
link their reclaim case and holder. General job events need no such target.
Populate these links in the same Dataverse transaction as the related state/audit
write where applicable. Preserve referenced rows; configuration updates create
a new settings row rather than changing values used by existing cases.

`LicenseHolder.Status` is the activity classification, `WaitlistEntry.Status`
is the queue state, and `ReclaimCase.Outcome` is the confirmed case state.
`ActionedBy` identifies the verified human decision-maker for the latest completed
case action. `AuditEvent` records its timestamp, actor, action, target and details;
it is not an approval system or an operation ledger.

Queue order uses approved `Position`, then `RequestedOn` and native entry ID for
deterministic ties. Pending/rejected entries are not allocation candidates.
A missing/invalid position holds allocation until corrected by an authorized
review. Reprioritization changes positions under the approved queue policy;
there is no separate priority column. Concurrent reorder/allocation protection
must be implemented in the backend, not inferred from a numeric field.

Use one configured tenant and Copilot SKU per deployment, bound to authenticated
connections and approved deployment settings. API fields such as `SkuId`,
source references, evidence dates and `policyVersion` are integration contracts,
not instructions to add columns. The four `Configuration` fields contain only
the stated thresholds, automatic-reclaim switch and exclusion group. Additional
policy settings/revision evidence belong to the approved integration configuration.

`EntryId` and `CaseId` use native business-record IDs. Proposal, operation,
approval and snapshot references come from the responsible backend components;
do not map them to invented Dataverse columns or assume they are audit row IDs.
Historical waits require verified fulfilment events correlated to requests;
current status alone supplies no fulfilment timestamp. Usage trends require
retained evidence beyond the holder's current activity date.

**Integration gap:** this minimal schema does not itself supply immutable
proposals, trusted approval receipts, idempotency bindings, atomic reservations,
per-channel notification receipts or historical usage snapshots. Demonstrate
these using existing approved integration components, or mark the dependent
capability blocked/unavailable and return a concrete owner-owned design task.
Do not create schema/storage beyond the approved columns and lookups, hide an expanded data model in `Details`,
or weaken action controls to make the build appear complete. Any new storage
design requires separate approval. Current reports may proceed with verified
sources; absent history must be reported as a gap, not fabricated.

All subsequent operation contracts remain requirements on the backend, not
features that table creation implements. Policy approval, exact-payload human
confirmation, active-entry/case uniqueness, concurrency, audit integrity and
recovery must pass the release gates before consequential actions run.

## Common operation contract

Every tool checks caller authentication, tenant, current role, allowed SKU, operation
enablement and field/record scope server-side. The server derives caller identity;
no tool accepts a caller-supplied identity as authorization. Query text, names and
requester references help resolve records but never grant access.

| Operation | Inputs | Outputs | Allowed records / actions |
|---|---|---|---|
| `lifecycle_inventory` | Department/user filter, allowed SKU, page token, requested grouping | Counts, holders, capacity, assignment source/date provenance, continuation and evidence envelope | Read directory/licence fields and approved holder snapshots only |
| `lifecycle_usage` | Department/user scope, time window, view `current`, `trend` or `reclaim-history` | Active/dormant/excluded/unknown buckets, reasons, coverage, case outcomes and evidence envelope | Read usage fields, policy, exclusions, snapshots and reclaim history |
| `lifecycle_waitlist` | User/entry filter or ordered queue, page token, requested historical interval | Entry ID, status, computed position, request date, historical wait sample/coverage | Read waitlist and fulfilment history, no individual usage data |
| `lifecycle_context` | Lookup kind `user`, `case`, `entry` or `next-assignment`, target reference | Authoritative IDs, current versions, relevant policy, eligibility, permitted actions and safe source references | Read only fields needed for the requested action, resolve ambiguous names without choosing silently |
| `lifecycle_prepare` | Allowlisted action, exact resolved targets, expected versions, action-specific fields | ProposalId, payload hash, normalized human summary, expiry, blockers, approval reference/URL | Create a proposal/approval request in the ledger only; never send business notices or change licences/cases/queue |
| `lifecycle_status` | OperationId or ProposalId | Approval state, execution status, per-target/per-step outcomes, evidence IDs, last check and reconciliation requirement | Read only caller-authorized operations; no hidden re-execution |

All read outputs include `status`, `asOfUtc`, `sourceIds`, `policyVersion` when
applicable, `complete`, `nextPageToken`, and `gaps`. Usage also includes
`reportRefreshDate`, `coverageStart`, `coverageEnd` and `identityMappingState`.
Counts state whether they cover the full set or only the returned subset.
Denied responses do not echo protected record details.

All execute tools below accept only `proposalId`, `approvalId` and
`idempotencyKey`. The backend retrieves the prepared payload and verified
approval rather than accepting a second mutable payload. A request changing
target, SKU, timing, recipients, queue position or policy version needs a fresh proposal
and confirmation. A preparation result is not permission to execute.

### Confirmation and authorization

1. The backend normalizes targets, checks policy and stores an expiring proposal.
2. The agent displays action, named target IDs, SKU, effective time, exact notices
   or queue/case changes, reason, current evidence and relevant residual report lag.
3. An authenticated approval adapter captures the human's explicit confirmation
   bound to the proposal/hash/version. It can capture a conversational response
   only if the channel supplies verifiable actor/event binding. Otherwise provide
   the returned authenticated approval link. A pasted approval or model-generated
   boolean is invalid. Do not fabricate a portal URL.
4. The execute handler verifies the approver's current authority, expiry and exact
   payload, rechecks state and atomically reserves the operation.
5. If another approver is required by policy, confirmation remains `pending`
   until that authenticated decision is recorded. Queue approval is separate from
   the later approval to assign a specific licence.

Never change `AutoReclaimEnabled` through this interface. A request over 25 targets
is rejected by both prepare and execute, counting distinct users across the
campaign. Propose separate explicit requests, but never automatically partition
and execute an oversized request. No wildcard recipients or arbitrary Graph URLs.

## Action-specific contracts

These rows add required fields to `lifecycle_prepare`; each names its one
authorized execute tool. All actions use the common identity, ledger, approval
and outcome requirements.

| Prepare action / execute tool | Prepared fields | Required checks and permitted mutation | Returned evidence and side effects |
|---|---|---|---|
| `waitlist-add` / `lifecycle_waitlist_add` | UserId, SkuId, SourceRequestRef, requester reference, justification | Resolve requester/target, validate request route and membership, unique active entry; create `PendingApproval` only | EntryId, status, request timestamp, audit ID; no approval or assignment |
| `waitlist-review` / `lifecycle_waitlist_review` | EntryId, decision `approve`, `reject` or `reprioritize`, policy-permitted position, reason, row version | Verify queue-reviewer role and allowed transition; reordering only within policy with a reason | Updated entry/version, decision receipt and queue position; no Graph write |
| `notify` / `lifecycle_notify` | CaseIds, template version, resolved recipients, both channels, initial time, reminder offsets, grace duration | Recheck dormancy/exclusions, <=25 targets, exact campaign approval; send approved warning and bounded reminders only | Per-channel acceptance/failure/unknown, run receipts, grace start/end, audit; no arbitrary message sending |
| `reclaim` / `lifecycle_reclaim` | CaseIds, UserIds, SkuId, effective time, policy/evidence versions | `Notified`, grace elapsed, no dispute, latest report covers grace end, adequate freshness, no exclusion, direct-only entitlement; remove only allowlisted SKU | Effective assignment readback, case state and audit; no automatic assignment |
| `assign` / `lifecycle_assign` | Selected EntryId/UserId, SkuId, queue version, effective time, confirmation template/recipient | Approved eligible queue head, current prerequisites/usage location, capacity and no existing entitlement; reserve queue/seat and add only specified SKU | Assignment readback, fulfilled entry, audit and separate confirmation-send status |
| `cancel` / `lifecycle_cancel` | CaseId, reason, row version | Cancel `Detected`, `Notified` or pre-removal `Disputed` case, invalidate pending proposals/reminders; serialize against executing reclaim | `Cancelled`, audit; completed removal is not reversed |
| `dispute` / `lifecycle_dispute` | CaseId, SourceRequestRef, reason, configured escalation recipient, case version | Record dispute and hold future removal if not executed; preserve completed/in-flight outcome rather than overwriting it | Dispute ID, case/operation state, routing status and audit; no automatic restore |

### Notification and case timing

Both Teams and Outlook initial notices are required in this design. Resolve the
recipient from the authoritative user record and use versioned templates that
state evidence dates, grace end, how to retain the licence and the approved dispute
route. Never include other holders' activity.

Record acceptance separately for each transport; acceptance is not proof of
delivery or reading. Set `NotifiedOn` to the later successful initial acceptance
and `GraceEndsOn` from that time. Before both succeed the case is `NotificationPending`
and cannot be reclaimed. Bounces, unknown sends or failed required reminders
hold reclaim for operator review; do not repeatedly resend accepted steps.
The Outlook connector may not return a message ID, so store the flow run/action
receipt and timestamp, not a fabricated provider ID.

The approved campaign binds the exact recipients, initial message, template
version and reminder schedule. Scheduled reminders need no new approval when
unchanged and still valid, but cancellation, new activity, exclusions or disputes
suppress them. Extending recipients or altering content requires new confirmation.

Assignment confirmations and dispute routing are explicit included side effects
of those approved actions. If notification fails after the state change, report
partial completion and recover the notification step only.

## Evidence and Microsoft Graph boundaries

| Need | Documented interface | Design and tenant requirements |
|---|---|---|
| Directory | [List users](https://learn.microsoft.com/en-us/graph/api/user-list?view=graph-rest-1.0), `GET /v1.0/users` | Select required fields, follow all continuation links, resolve IDs and allowlist SKUs after retrieval |
| Capacity | [List subscribedSkus](https://learn.microsoft.com/en-us/graph/api/subscribedsku-list?view=graph-rest-1.0), `GET /v1.0/subscribedSkus` | Resolve SKU from tenant subscriptions; this endpoint does not support `$filter` |
| Usage | [Copilot usage user detail](https://learn.microsoft.com/en-us/microsoft-365/copilot/extensibility/api/admin-settings/reports/copilotreportroot-getmicrosoft365copilotusageuserdetail), `GET /v1.0/copilot/reports/getMicrosoft365CopilotUsageUserDetail(period='D90')` | v1.0 returns CSV stream; explicitly pin/test report version, parse headers and dates, use report refresh/coverage, not collection time |
| Assignment provenance | [licenseAssignmentState](https://learn.microsoft.com/en-us/graph/api/resources/licenseassignmentstate?view=graph-rest-1.0) | `assignedByGroup=null` denotes direct assignment; hold inherited/mixed assignments, and treat `lastUpdatedDateTime` as state update time, not first assignment |
| Licence changes | [assignLicense](https://learn.microsoft.com/en-us/graph/api/user-assignlicense?view=graph-rest-1.0), `POST /v1.0/users/{id}/assignLicense` | Reclaim: `addLicenses=[]`, `removeLicenses=[allowedSkuId]`; assign: one allowlisted SKU in `addLicenses`, `removeLicenses=[]`; never modify unrelated plans |

The usage API covers licensed Microsoft 365 Copilot users, not unlicensed Copilot
Chat usage, and its documented cloud coverage is the global service. Validate
the target cloud before selecting this scenario. Do not substitute a beta API.

Usage is delayed. The [usage report guidance](https://learn.microsoft.com/en-us/microsoft-365/admin/activity-reports/microsoft-365-copilot-usage?view=o365-worldwide)
describes typical availability within 48 hours after a UTC day ends; this is not
a freshness guarantee. The business must set a maximum acceptable evidence age
and acknowledge the remaining unobserved interval before approving a reclaim.

Derive dormancy using complete UTC days between a trustworthy non-null last
activity date and report coverage end, with coverage spanning the configured
threshold. A candidate meets the threshold at `observedInactiveDays >= DormancyDays`.
Evidence age must be `<= MaxEvidenceAgeHours`, and reclaim timing must satisfy
`nowUtc >= GraceEndsOn`; all other eligibility and approval checks still apply.
An anonymized identifier, missing user row, null activity, incomplete
pagination, insufficient window or failed exclusion lookup is `unknown`/`held`,
not dormant. Do not de-anonymize or change tenant privacy settings through the
agent. Newly observed users without adequate history cannot become reclaimable
merely by having a blank date.

Trends require retained snapshots from an approved integration. If unavailable,
report the history gap rather than expanding the Dataverse schema. Current
activity reports and assignment state do not establish a three-month trend or
an original assignment timestamp.
Revalidate latest available usage, live licence provenance, current exclusion
membership and case state immediately before a write. This still is not real-time
proof that a user has not just used Copilot.

## Execution identities and connection ownership

| Operation group | Execution identity / owner | Scope |
|---|---|---|
| MCP entry and all tool authorization | Validated user identity through approved MCP authentication; integration owner | Authenticate tenant/caller and enforce current licence-team role; no shared-secret-only attribution |
| Inventory, usage, context and scheduled reads | Owned read service principal; M365 data owner | `User.Read.All`, `LicenseAssignment.Read.All`, `Reports.Read.All` as needed; minimal exclusion-group membership access approved separately |
| Reclaim / assign | Owned write principal; M365 licensing owner | `LicenseAssignment.ReadWrite.All`, with backend SKU/target limits; no direct credential exposure to the agent |
| Dataverse handlers | Application user with custom table roles; Power Platform owner | Scoped table/field access, append audit, no delete privilege for historical audit |
| Notification and dispute transport | Tested connector-supported account; messaging owner | Approved sender/poster, target users and escalation route; not an arbitrary mail/chat tool |
| Human approval | Authenticated approver; licence policy owner | Receipt records actual decision-maker, authorized role, payload hash and expiry |

Validate group-membership permissions, supported connector authentication, caller
binding and non-maker attribution in G2/G4. An execution principal is not the
approver, and a human name in a prompt is not proof of either identity.

## Concurrency, idempotency and result tracking

Reads have no business mutation and can be retried with bounded backoff. Return
snapshot IDs and pagination consistency; never combine incomplete pages into
an authoritative total. Preparations can be deduplicated by normalized request
key and expire without business effects.

Every action has a durable idempotency key bound to action, target set and proposal.
The same key/payload returns the existing operation; reusing it with a different
payload fails. Use [Dataverse version checks](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/webapi/perform-conditional-operations-using-web-api)
and atomic backend reservations for cases, queue entries and SKU capacity.
ETags alone do not make Graph and Dataverse transactional. Out-of-band admin
changes can still race; recheck and reconcile effective state.

Persist intent before the external write, then each observed step and final audit.
If Graph succeeds but persistence fails, retain the recoverable operation and
block further dependent work until reconciliation. Never undo successful removal
or assignment automatically to make a batch look atomic.

| Status | Meaning and required response |
|---|---|
| `succeeded` / `no-op` | Verified result or already-satisfied state; return evidence, do not duplicate effects |
| `empty` | Authorized read found no matching records; not a backend failure |
| `held` | Ineligible or insufficient evidence; explain the safe blocker without executing |
| `operator-paused` | Capability intentionally disabled; no alternate tool/identity path |
| `denied` | Authorization refused; do not reveal protected data or retry with a broader identity |
| `unavailable` / `failed` | Dependency absent or a known operation failure; report actual cause and operator route |
| `partial` | Some steps/targets succeeded; return every outcome and preserve completed work |
| `pending` | Approval, asynchronous execution or provisioning incomplete; use status lookup |
| `unknown` | Effect may have occurred but cannot be proven; reconcile before any retry |

Execution responses include `operationId`, `status`, `perTargetResults`,
`perStepResults`, `auditIds`, `observedAtUtc` and `nextCheckAfterUtc` when pending.
After a timeout use `lifecycle_status` with the existing ID/key; the backend
reconciles Graph, case, queue and transport run evidence before authorizing retry.
Use bounded retries for throttled reads, not blind retries of consequential sends.
No exactly-once transport guarantee is claimed.

## Disablement and release

All capabilities are standard. During maintenance an operator can pause a specific
action server-side; also remove/disable its tool exposure, stop matching scheduled
work and invalidate stale proposals. Removing only a skill is not a permission
revocation. Retain read/status access and shared connections needed for recovery.

G1-G8 in [Resources](README.md#release-gates) cover implementation, identity,
data quality, notifications, policy, testing, costs and deployment. Runtime skill
imports must remain blocked from live writes until those gates are satisfied.
