# Starter integration contracts

**Status:** binding specifications for the seven supplied skill procedures, not executable integrations. Tool names and Workday paths originate in those supplied definitions; their actual schemas, availability and delegated behavior have not been independently validated. Keep each capability OFF until the integration owner records a supported binding and evidence.

## Shared execution and result contract

The integration owner owns tool schemas, operation bindings, secure connections and per-environment deployment. System/identity owners own vendor permissions and immutable caller mapping. Every operation must execute as the authenticated employee, not a maker, shared elevated user or identity typed in chat. Knowledge ingestion is separate.

| Element | Required behavior |
|---|---|
| Inputs and scope | Typed/allowlisted fields, tables, paths, filters and record references only; no caller override or arbitrary query/endpoint execution |
| Read output | Authorized requested data and source reference/time, actual query scope, complete/partial coverage and supported continuation metadata; preserve vendor display labels |
| Denial and empty | Do not reveal hidden-record existence; distinguish an authorized complete empty result from denied, malformed, unavailable or partial data |
| Privacy | Filter fields before model delivery, not just in the final answer; no internal notes, unrelated records, secrets or bulk export |
| Links | Only configured HTTPS tenant roots and authorized returned references; validate/encode IDs and reject unexpected hosts |
| Failures | Safe error/correlation reference; never raw credentials or an invented success. Preserve successful independent steps |

Actual vendor schemas may differ from the supplied names. Record the verified mappings and adapt the affected skill before enablement; do not silently invent operation names or parameters. If a picker exposes a broad tool, restrict it through backend policy or a narrow approved wrapper. Instructions alone cannot enforce own-record access.

### Mutation controls

These skills use the supplied vendor-style calls, **not** the former scenario-owned prepare/commit aliases. Equivalent protections still belong in the integration:

1. Prepare an immutable summary of caller, target, payload, source state and expiry without a business write. Obtain backend-verifiable approval for that exact action/sequence through the configured trusted surface. Model-generated `confirmed=true`, generated proof or stale chat consent is insufficient.
2. Before each write, reauthorize caller and target, validate field/reference values and source state, and reject stale/changed confirmation. A changed target, payload or state requires a new preview and confirmation. Detect no-op phone updates before opening an event.
3. Keep a durable operation record keyed to caller, action and confirmed payload, with deduplication/concurrency controls. Retain every created incident, Workday event or request reference. Do not claim a vendor exactly-once guarantee.
4. On timeout/unknown, reconcile the same operation against the source before retrying. If the create may have succeeded but its reference is missing, do not create again. If reconciliation is unavailable, keep the capability OFF or report an unresolved in-flight operation; do not fabricate completion.
5. Provide an actual authorized event/request-state lookup or operation ledger in the deployed binding. Its name/schema must be recorded, not invented here. Poll within configured bounds; return pending when limits are reached. Accepted/submitted is not approved or effective.

The home-phone sequence creates and edits a **staged business-process event**, so approval for staging must precede the first write. Submission requires confirmation covering the exact final change; one valid approval may cover the unchanged full sequence. Declining submission leaves a disclosed staged event; cancellation is not an implicit action.

## Workday read contracts

| Binding | Permitted input and result |
|---|---|
| WD-REST-API Cost Center | `GET common/v1/{tenant}/workers/me/organizations`; retain literal `{tenant}` for connection substitution. Parse `data`, select exact `organizationType.descriptor="Cost Center"`, return all matching entry descriptors. Internal IDs only if requested. No other organizations/membership lookups |
| `find_staffing_workers` | `contextInstanceId="me"`; return allowlisted own core profile fields and `id` (WID) distinct from `workerId` (employee number). Downstream WIDs must be bound to the same caller in the backend |
| `list_worker_addresses`, `list_worker_phone_numbers` | Own home details only when specifically requested or needed for the phone update; approved minimal fields. No work-email inference from home contact tools |
| `find_absence_balances` | Resolved 32-character WID, not `"me"` or employee number; requested plan, quantity, unit and effective date, with distinct plans kept separate |

Core profile fields include approved job title, work phone/email, employee ID and source service dates only when actually returned and authorized. A general profile request does not automatically fetch home addresses, home phones or balances. Missing requested fields are reported unavailable; do not widen lookup. Compensation, national IDs, passports, visas, emergency contacts, performance and medical records remain excluded. Read gates cover pagination and partial/denied results as well as requested-field minimization.

## Workday home-phone contract

| Stage / binding | Required input and behavior |
|---|---|
| Current phones | Resolve own WID and call `list_worker_phone_numbers`; choose an existing home phone by source reference and attributes. No add/delete or work-phone edit |
| Reference lookups | Approved WD-REST-API device-type path `person/v4/{tenant}/values/commonPhone/phoneDeviceTypes/` and tenant-verified country-code values lookup; preserve connection-owned `{tenant}`. No guessed IDs or arbitrary endpoints |
| `create_worker_home_contact_change` | Open one event only after exact staging approval. Store event ID durably; reconcile an uncertain open before any repeat |
| `list_phone_numbers_for_home_contact_change` | Same event ID; match the previously selected phone, detect source conflicts/missing phone before editing |
| `update_phone_number_for_home_contact_change` | Same event/phone, resolved reference IDs and requested fields only. Carry forward untouched usage/primary flags, extension and other properties exactly; do not replace the whole contact collection |
| `submit_home_contact_change` | Submit that event once after exact submission approval and source-state recheck |
| Verification | Configured event-state path plus effective `list_worker_phone_numbers` read-back; pending routing/approval is not completed even if the staged value exists |

No effective date is required unless supplied/required by the actual supported process. Do not silently drop a supplied date that the binding cannot represent. If no existing home phone matches, explain that adding one is outside the starter scope.

## Workday time-off contract

| Binding | Required input and behavior |
|---|---|
| `find_staffing_workers` | Resolve own WID with `"me"`; no user-selected worker |
| `find_worker_eligible_absence_types` | Own WID, eligible Time Off / Absence Table types only; never Leave of Absence |
| `find_worker_valid_time_off_dates` | Own WID and selected type; authoritative valid dates, per-day quantities/units and required position/reference values |
| `find_worker_time_off_details` | Relevant balance and existing overlapping entries; preserve source units/dates and surface findings |
| `create_worker_time_off_request` | Once per confirmed request: own WID and validated daily date/type-ID/quantity entries, units/position only as required by the actual schema. Starts approval routing; no separate approve action |
| Verification | Authorized source request reference/status and operation reconciliation; report submitted/pending/failed truthfully. Never equate submission with approval |

Invalid dates or unresolved required fields block submission. Insufficient balance/overlaps must be disclosed and handled according to Workday/backend policy, not overridden by chat confirmation. Any edited request is revalidated and reconfirmed.

## ServiceNow operation contracts

| Binding / use | Inputs and permitted result |
|---|---|
| `GetIncidents` caller resolution | Fixed `tableType="sys_user"`; trusted email/UPN encoded into `email=<authenticated email>^ORuser_name=<authenticated email>`; fields `sys_id,user_name,email`. Backend must establish a unique mapping, detect duplicates and reject query injection; a limit of 1 cannot prove uniqueness |
| `CreateIncident` | Fixed `tableType="incident"`; only `short_description`, validated severity display label and authenticated resolved `caller_id`. `sysparm_input_display_value=true`, `sysparm_display_value=true`, `sysparm_exclude_reference_link=false`, fields `number,sys_id,short_description,severity,caller_id`. Return actual persisted fields/number/ID; no hidden full-description update, attachments or assignment change |
| `GetIncidents` active list | Fixed `tableType="incident"`; query `active=true^caller_id=javascript:gs.getUserID()^ORDERBYnumber`, fields `number,severity,short_description`, `sysparm_display_value=true`, limit `50`. `gs.getUserID()` must resolve the actual employee. No other-person name query, even for admins |
| `GetCases` | Backend-enforced own HR case visibility; supported requested state filter only. Return Number, State, HR Service and Short Description with display values and coverage metadata; no internal notes |

Validate that High/Medium/Low labels map to this instance's `1 - High`, `2 - Medium`, `3 - Low`. Severity and short description are the starter inputs; if the tenant requires extra fields or uses different semantics, resolve that binding gap before ON instead of guessing. A successful short-description incident is not a promise that a separate full-description field persisted.

For created incident links, validate HTTPS `caller_id.link` against the configured instance, extract the base before `/api/now/table/`, and use the returned **incident** `sys_id` in `<base>/nav_to.do?uri=incident.do?sys_id=<encoded incident sys_id>`. If the link cannot be established, report the actual ticket number without a link. List results do not request these IDs/link fields, so do not promise links unless the approved binding supplies them.

The incident-list table has exactly **Ticket Number | Severity | Short Description**; HR cases have exactly **Number | State | HR Service | Short Description**. Escape Markdown delimiters/newlines while preserving content. At the incident 50-row limit, disclose possible truncation. An empty HR response means no cases in the actual queried scope, not necessarily "no open cases".

## Minimum implementation evidence before ON

Record exact tool/schema/version, vendor bindings, endpoint/filter/field allowlists, employee caller trace, connection owner, confirmation implementation, durable deduplication/concurrency, timeout reconciliation, event/request status/read-back, error mapping and rollback procedure in the restricted manifest. A skill upload, connector picker entry or maker-only test is not sufficient.
