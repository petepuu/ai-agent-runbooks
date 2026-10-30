# Employee Self-Service GHCP capability matrix

**Implementation status:** Four scenario documents, this manifest, integration specifications and 28 runtime skills are supplied. **No live tool implementation, workflow export, tenant configuration or deployment is supplied.** All source connections are unvalidated; all live capabilities are OFF.

## Profiles and enabling rules

**Baseline:** only `ess-knowledge-answer` and `ess-request-navigation` may be ON after the knowledge gate passes. No action tools are exposed. A planned extended deployment may enable individual rows, not a blanket "ServiceNow/Workday enabled" switch. Unlisted operations are OFF.

Every tool name below is a **proposed custom contract alias**, not a verified vendor operation ID. Implement through an approved GHCP-compatible MCP service or workflow, record the actual binding and schemas in the restricted deployment manifest, then pass the row's dependencies and acceptance tests. Import only the skills approved for that deployed manifest. Disabling also revokes backend/tool access, alternate paths and stale-session access without breaking shared connections.

## Capability inventory and import order

| Capability ID / linked SKILL.md | Source behavior / disposition | Tools or knowledge | Actual implementation / operation ID | Dependencies | Profile / access / approval |
|---|---|---|---|---|---|
| [ess-knowledge-answer](Skills/ess-knowledge-answer/SKILL.md) | Original grounded HR/IT answers; retained | Configured SharePoint + ServiceNow KB retrieval | No external operation tool | K | Baseline candidate; currently OFF pending K; employee source ACLs |
| [ess-request-navigation](Skills/ess-request-navigation/SKILL.md) | Original portal/form links; retained | Same approved knowledge | No external operation tool | K | Baseline candidate; currently OFF pending K; navigation only |
| [sn-hr-create-case](Skills/sn-hr-create-case/SKILL.md) | ESS HRSD Create Case; re-expressed | `sn_hr_create_case` | Proposed custom contract; unimplemented | S, M, W | OFF; own case, confirmed create |
| [sn-hr-list-cases](Skills/sn-hr-list-cases/SKILL.md) | ESS HRSD Get User Cases; re-expressed | `sn_hr_list_cases` | Proposed custom contract; unimplemented | S | OFF; own cases, bounded pagination |
| [sn-hr-case-details](Skills/sn-hr-case-details/SKILL.md) | ESS HRSD Get Case Details; re-expressed | `sn_hr_case_details` | Proposed custom contract; unimplemented | S | OFF; own case, employee-visible fields |
| [sn-hr-case-updates](Skills/sn-hr-case-updates/SKILL.md) | ESS HRSD Get Case Updates; re-expressed | `sn_hr_case_updates` | Proposed custom contract; unimplemented | S | OFF; own case, latest visible updates |
| [sn-hr-update-case](Skills/sn-hr-update-case/SKILL.md) | ESS HRSD Update Case; re-expressed | `sn_hr_case_details`, `sn_hr_update_case` | Proposed custom contracts; unimplemented | S, M, W | OFF; exact comment/state/close payload, source transition permission |
| [sn-hr-add-attachment](Skills/sn-hr-add-attachment/SKILL.md) | ESS HRSD Add Attachments To Case; re-expressed | `sn_hr_case_details`, `sn_hr_add_attachment` | Proposed custom contracts; unimplemented | S, F, W | OFF; confirmed file and own case |
| [sn-it-create-ticket](Skills/sn-it-create-ticket/SKILL.md) | ESS ITSM Create Ticket; re-expressed | `sn_it_create_ticket` | Proposed custom contract; unimplemented | S, M, W | OFF; own incident; attachment action separate |
| [sn-it-list-tickets](Skills/sn-it-list-tickets/SKILL.md) | ESS ITSM Get User Tickets; re-expressed | `sn_it_list_tickets` | Proposed custom contract; unimplemented | S | OFF; own incidents, bounded pagination |
| [sn-it-ticket-details](Skills/sn-it-ticket-details/SKILL.md) | ESS ITSM Get Ticket Details; re-expressed | `sn_it_ticket_details` | Proposed custom contract; unimplemented | S | OFF; own incident, employee-visible fields |
| [sn-it-ticket-updates](Skills/sn-it-ticket-updates/SKILL.md) | ESS ITSM Get Ticket Updates; re-expressed | `sn_it_ticket_updates` | Proposed custom contract; unimplemented | S; latest-target resolution in tool | OFF; latest visible update, no guessed record |
| [sn-it-update-ticket](Skills/sn-it-update-ticket/SKILL.md) | ESS ITSM Update Ticket; re-expressed | `sn_it_ticket_details`, `sn_it_update_ticket` | Proposed custom contracts; unimplemented | S, M, W | OFF; own incident, allowlisted patch; no internal work notes |
| [sn-it-add-attachment](Skills/sn-it-add-attachment/SKILL.md) | Attachments within ESS ITSM create/update; extracted | `sn_it_ticket_details`, `sn_it_add_attachment` | Proposed custom contracts; unimplemented | S, F, W | OFF; independent permission, no implicit upload during create |
| [wd-get-base-compensation](Skills/wd-get-base-compensation/SKILL.md) | Workday Get BaseCompensation; re-expressed | `wd_get_base_compensation` | Proposed custom contract; unimplemented | D, H | OFF; own base amount/currency/frequency/effective date only |
| [wd-get-certifications](Skills/wd-get-certifications/SKILL.md) | Workday Get Certifications; re-expressed | `wd_get_certifications` | Proposed custom contract; unimplemented | D | OFF; own certification summary |
| [wd-get-company-code](Skills/wd-get-company-code/SKILL.md) | Workday Get CompanyCode; re-expressed | `wd_get_company_code` | Proposed custom contract; unimplemented | D | OFF; own company code/name |
| [wd-get-cost-center](Skills/wd-get-cost-center/SKILL.md) | Workday Get CostCenter; re-expressed | `wd_get_cost_center` | Proposed custom contract; unimplemented | D | OFF; own cost-center code/name |
| [wd-get-emergency-contacts](Skills/wd-get-emergency-contacts/SKILL.md) | Workday Get EmergencyContact; re-expressed | `wd_get_emergency_contacts` | Proposed custom contract; unimplemented | D, H | OFF; minimized own-record contact summary; no edits |
| [wd-get-employee-id](Skills/wd-get-employee-id/SKILL.md) | Workday Get EmployeeId; re-expressed | `wd_get_employee_id` | Proposed custom contract; unimplemented | D | OFF; own employee ID |
| [wd-get-employment-information](Skills/wd-get-employment-information/SKILL.md) | Workday Get EmploymentInformation; re-expressed | `wd_get_employment_information` | Proposed custom contract; unimplemented | D | OFF; allowlisted own employment details |
| [wd-get-language-information](Skills/wd-get-language-information/SKILL.md) | Workday Get LanguageInformation; re-expressed | `wd_get_language_information` | Proposed custom contract; unimplemented | D | OFF; recorded own language/proficiency |
| [wd-get-national-ids](Skills/wd-get-national-ids/SKILL.md) | Workday Get NationalIDs; re-expressed | `wd_get_national_ids` | Proposed custom contract; unimplemented | D, H | OFF; backend-masked identifiers, no full-number reveal |
| [wd-get-passports](Skills/wd-get-passports/SKILL.md) | Workday Get Passports; re-expressed | `wd_get_passports` | Proposed custom contract; unimplemented | D, H | OFF; masked own passport summary |
| [wd-get-service-anniversary](Skills/wd-get-service-anniversary/SKILL.md) | Workday Get ServiceAnniversary; re-expressed | `wd_get_service_anniversary` | Proposed custom contract; unimplemented | D | OFF; source-provided service date/anniversary |
| [wd-get-visas](Skills/wd-get-visas/SKILL.md) | Workday Get Visas; re-expressed | `wd_get_visas` | Proposed custom contract; unimplemented | D, H | OFF; masked own visa summary; no immigration advice |
| [wd-update-email](Skills/wd-update-email/SKILL.md) | Workday Update Email; re-expressed | `wd_contact_options`, `wd_update_email` | Proposed custom contracts; unimplemented | D, W; contact options/read-back | OFF; own selected contact record/type, exact-payload confirmation |
| [wd-update-phone](Skills/wd-update-phone/SKILL.md) | Workday Update PhoneNumber; re-expressed | `wd_contact_options`, `wd_update_phone` | Proposed custom contracts; unimplemented | D, W; contact options/read-back | OFF; own selected phone/device/country code, exact-payload confirmation |

Dependencies are **tools/backend services**, not an assumption that another runtime skill is installed:

| Key | Required dependency and exit criterion | Owner |
|---|---|---|
| K | Approved SharePoint and ServiceNow KB; exact source names/IDs; ACL, scripted user-criteria, citation, freshness and conflict tests | Knowledge + ServiceNow owners |
| S | Supported GHCP ServiceNow binding, delegated user mapping, own-case/incident authorization, field visibility, portal URL validation | ServiceNow + identity owners |
| M | `sn_request_metadata`: readable service/category/field/transition metadata; server validation and full-description fidelity | ServiceNow integration owner |
| D | Supported GHCP Workday binding; delegated employee resolution, REST/SOAP/network and domain-security validation; no automatic ESS package reuse | Workday + identity owners |
| H | HR/privacy sign-off on minimization, masking, audience, channel rendering, transcripts and retention for each sensitive read | HR + privacy owners |
| W | Trusted prepare/confirm/commit flow plus shared `ess_operation_status`; durable ledger, idempotency, concurrency, bounded tracking and authorized read-back | Integration + security owners |
| F | Secure upload handle, file policy, malware scanning, explicit target/file confirmation and deduplication | Integration + security owners |

## Contract and source coverage

[Integration contracts](Integration-contracts.md) defines inputs, outputs, record/field allowlists, identity, connection ownership, concurrency, retry/reconciliation and async tracking for every row. The [skills index](Skills/README.md) and [acceptance cases](../4.Sample-prompts.md#capability-tests) have one-to-one coverage.

Original source behaviors outside this inventory are accounted for in the [architecture mapping](../2.Architecture.md#source-to-ghcp-mapping). SuccessFactors, population analytics, autonomous delivery, template cache/system-topic implementation, and catalog checkout are excluded or deferred, not hidden capabilities. Template setup topics become deployment configuration; hard user permission checks and transaction guarantees remain backend requirements.
