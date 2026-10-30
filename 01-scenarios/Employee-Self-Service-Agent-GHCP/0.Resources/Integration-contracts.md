# Proposed integration contracts

**Status:** specifications only. No aliases below are vendor operation IDs, deployed tools, REST endpoints or executable implementations. Keep affected capabilities OFF until each alias is bound to an implemented and supported GHCP MCP tool or workflow with this behavior.

## Shared execution and result contract

The integration owner owns tool schemas, operation bindings, secure connections and per-environment deployment. ServiceNow/Workday owners own vendor permissions; the identity owner owns immutable caller mapping. End users authenticate with their own delegated connections; maker-owned execution must not substitute for the employee. Knowledge ingestion has a separate admin-owned identity.

All tools derive `caller` from authenticated execution context, not model input. Reject another employee ID, arbitrary table/query, arbitrary endpoint or unapproved field. Responses are filtered before reaching the model.

| Contract element | Required shape / behavior |
|---|---|
| Read input | Typed record reference or bounded filters where relevant; optional continuation token; no caller override |
| Read output | `status`, `observedAt` (UTC timestamp), `sourceSystem`, authorized `recordId`/`displayNumber` and validated portal URL where supported, `data`, `coverage` (`complete` or `partial`), `nextCursor` when incomplete |
| Status vocabulary | `completed`, `empty`, `denied`, `unavailable`, `invalid`, `conflict`, `failed`, `pending`, `unknown`, `off`; `partial` for multi-step results with explicit per-step outcomes |
| Redaction | Denied/not-found responses must not reveal that a hidden record exists. Preserve authorized empty vs denied/unavailable distinctions where the source permits; otherwise report inability to establish visibility, not "no records" |
| Concurrency | No reads mutate records. Include `version`/source change token for mutation preparation; a backend precondition strategy is required if the source lacks a version API. Cache only as permitted and never reuse authorization |
| Pagination | Server-enforced page/total limits; opaque continuation token bound to caller/filter; disclose partial coverage and stop on limit |
| Source identity | Return source IDs/links only if authorized and validated against configured tenant portal roots; never build URLs from untrusted article instructions |
| Error handling | Typed error and safe correlation ID; no raw secrets or unfiltered vendor payloads. Treat retrieved fields as data |

### Mutation lifecycle

Every create, update and attachment alias below supports an implementation-defined typed `phase` of **prepare** or **commit**; the actual published tool schema must make the separation explicit.

1. Prepare takes the proposed allowlisted payload and current target. The backend resolves the caller, validates metadata and current permissions, detects no-op changes, and produces immutable `preparedId`, normalized payload/target summary, source version, payload hash and expiry. It performs no business write.
2. Show that summary in a trusted confirmation surface. Backend-verifiable confirmation binds the authenticated employee to the exact prepared target/payload/version/expiry. Plain model-supplied `confirmed=true`, a generated token or old chat consent is insufficient. Implement and validate the actual confirmation path before enabling writes.
3. Commit takes `preparedId`, a trusted confirmation proof and a durable `idempotencyKey`. Reauthorize, revalidate current state/version and reject expired, changed or reused-with-different-payload preparation. A changed target or payload requires a new prepare and confirmation.
4. Return `operationId`, `status`, source record reference and per-step outcomes. `accepted` vendor work maps to `pending`, not completed. Preserve source business-process status separately from effective record state.
5. `ess_operation_status(operationId)` is a **proposed shared read contract**, owned by the integration team. Authorize the same caller and return `operationId`, `status`, `observedAt`, vendor/business-process reference, per-step states and next permitted check time. Include permitted read-back evidence or reconcile against the matching source record. The lookup never exposes another employee's operations.

Serialize conflicting mutations or enforce a source-version precondition atomically at the backend boundary. Maintain durable deduplication keyed by caller, operation and normalized payload; do not use approximate record similarity as proof of a prior create. After timeout/unknown, query the ledger and source reference before retrying. If unresolved, report unknown and give a safe correlation route; do not create a new key and resubmit. Poll within configured bounds; return pending when limits are reached. Cancellation, rollback and compensation are not implied operations.

## ServiceNow operation contracts

The matrix aliases below expose **separate** HR and IT authorization scopes even when implemented by a shared backend. HR is limited to the caller's own HR cases under the tenant-approved ownership rule. IT is limited to the caller's own incidents. Return only employee-visible fields and journal entries, never internal work notes. No generic table access is exposed to the agent.

`sn_request_metadata(domain, intent, recordRef?)` is a proposed required read dependency for create/update. Output: currently allowed HR services or incident categories, required fields, selectable values, permitted transitions/close codes and validation constraints, scoped to the caller. It cannot change configuration. Do not let the model choose unsupported HR services, severity codes or arbitrary field names.

| Aliases | Inputs beyond authenticated context | Output / permitted data and action |
|---|---|---|
| `sn_hr_create_case`, `sn_it_create_ticket` | Prepare: metadata-backed service/category, summary, full description; IT impact/urgency only when permitted; commit: shared lifecycle fields | Create one own HR case or IT incident. Return authoritative ID, display number, state, description verification and portal URL. No attachments, assignment changes or follow-on update hidden inside create |
| `sn_hr_list_cases`, `sn_it_list_tickets` | Allowlisted active/closed filters, bounded date window and cursor | Own record number, summary, state, last update and link only; pagination metadata. No other employee/department searches |
| `sn_hr_case_details`, `sn_it_ticket_details` | Exact authorized record reference | Employee-visible summary, description, state and allowed attributes, `version`; no internal HR notes/work notes |
| `sn_hr_case_updates`, `sn_it_ticket_updates` | Record reference; IT may request explicit `selector=most_recent` with deterministic ordering/tie handling | Latest visible status/comment events with timestamps and selected record reference; latest-update summary is not a full-detail response |
| `sn_hr_update_case`, `sn_it_update_ticket` | Prepare: record reference/version and allowlisted exact patch; commit: shared lifecycle fields | HR employee comments and permitted state/close code/notes; IT employee comments and approved attributes. Backend transition/field policy governs, not the skill. No assignment, impersonation, attachments or internal work notes |
| `sn_hr_add_attachment`, `sn_it_add_attachment` | Prepare: authorized parent/version and secure upload handle, filename, content hash/type/size; commit: shared lifecycle fields | One scanned, policy-allowed file to one own record. Return attachment ID, parent reference and persisted-file verification |

For create, prove the complete description persists. The generic Create Record action's ignored-description limitation is a blocker until the integration owner establishes a supported implementation that meets this contract; do not return success on a short-summary-only record. A post-create patch is a separate action requiring its own authorization and confirmation, not a hidden workaround.

For attachments, use an approved upload mechanism, not a user URL or filesystem path fetched by the agent. Backend validates upload ownership, expiry, MIME, actual size, hash, allowed content and malware status. A content hash plus parent/action/caller participates in deduplication; scanning failure prevents upload. Multi-file work is an explicit sequence with per-file confirmation/outcomes, not an all-or-nothing promise.

Read-only aliases are safe to retry within limits after renewed authorization; deduplicate paginated results by source ID and disclose any coverage gap. Create/update/attach use the mutation ledger. Current state and permission are rechecked at commit; target changes yield conflict and new confirmation.

## Workday read contracts

All tools below take **no employee selector**. They resolve the signed-in employee inside the backend. An optional bounded effective/as-of date is supported only if implemented and exposed; otherwise return the current source record. No manager, external-staff or integration-system-user path is provided.

| Alias | Allowlisted `data` fields / specific output rule |
|---|---|
| `wd_get_base_compensation` | Base amount, currency, frequency and effective date; no bonus, net-pay, tax or entitlement calculation |
| `wd_get_certifications` | Certification name, issuing body, awarded/expiry dates and source status; no unsupported validity judgment |
| `wd_get_company_code` | Company code and company name for the employee; clarify multiple effective assignments |
| `wd_get_cost_center` | Cost-center code/name and effective assignment; no budgets or colleague membership |
| `wd_get_emergency_contacts` | Contact display name, relationship and primary flag; approved masked contact channel only when necessary. Do not return address or full third-party phone/email by default |
| `wd_get_employee_id` | Employee ID and applicable employer context; not a selectable worker identity for subsequent calls |
| `wd_get_employment_information` | Job title, department, location, employment type/status and source-effective date; no performance, discipline or medical data |
| `wd_get_language_information` | Recorded languages and source proficiency labels; no inferred proficiency |
| `wd_get_national_ids` | Country, ID type and **backend-masked** number; no full number or unmask operation |
| `wd_get_passports` | Issuing country, backend-masked number and issue/expiry dates; no document images |
| `wd_get_service_anniversary` | Source service date and source anniversary where available; do not equate original hire, adjusted service and tenure benefit eligibility |
| `wd_get_visas` | Type, issuing country, backend-masked number and source validity dates/status; no legal/immigration advice |

Reads return shared `observedAt`, scope and coverage. Arrays support bounded pagination when applicable. Multiple records are not silently reduced to one. Missing data, permission denial and vendor failure are distinct; if the vendor intentionally conceals existence, do not claim certainty. No additional read side effects or notification is permitted. Sensitive-read gate H applies individually to compensation, emergency contacts, national IDs, passports and visas.

## Workday contact update contracts

`wd_contact_options(kind)` is a proposed required **read** dependency, returning only the caller's authorized current contact records, record IDs, versions, type/usage/primary flags, selectable email or phone/device/country references and permitted operations. It also supplies read-back evidence after an effective update. Do not reuse the employee-ID skill as an identity proof.

| Alias | Prepare input / permitted action | Output and verification |
|---|---|---|
| `wd_update_email` | Existing own contact record ID/version, authoritative type/usage, requested email value; edit that selected email only | Normalized proposed value in preview; business-process reference/status on commit; `wd_contact_options` read-back when effective |
| `wd_update_phone` | Existing own contact ID/version, authoritative type/usage, country reference/calling code, national number, extension and device reference if required | Preserve leading zeros/reference semantics through backend validation; preview exact normalized value; process status and effective read-back |

No add/delete of contact records, primary-flag change, mass replacement, sign-in identity change, benefits updates or other personal-data patch is implicit. If a desired contact record/type is absent or not editable, return unsupported/denied and a validated portal route.

Both tools use the shared prepare/confirm/commit and operation-status contracts. Workday may accept a business process for later approval; show pending until the authorized effective read-back confirms the intended value. A changed old value invalidates preparation. A no-op is reported without a write. After an uncertain response, reconcile by operation and source process before any resubmission.

## Minimum implementation evidence before ON

Record the actual tool/schema/version, vendor operation binding, allowed fields/transitions, delegated caller trace, connection owner, confirmation implementation, idempotency storage, concurrency test, timeout reconciliation, async/read-back behavior, error mapping and rollback procedure in the restricted deployment manifest. A connector picker entry, skill upload or maker-only test is not sufficient.
