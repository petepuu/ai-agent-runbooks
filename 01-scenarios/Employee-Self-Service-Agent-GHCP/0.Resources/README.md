# Employee Self-Service Agent (GHCP) - Resources

## Included Assets

| Asset | Purpose |
|---|---|
| [ess-hr-policy/SKILL.md](./Skills/ess-hr-policy/SKILL.md) | Grounded HR policy guidance with personal-data and sensitive-case boundaries |
| [ess-it-guidance/SKILL.md](./Skills/ess-it-guidance/SKILL.md) | Source-based IT how-to and safe troubleshooting |
| [ess-catalog-routing/SKILL.md](./Skills/ess-catalog-routing/SKILL.md) | Exact verified request-form/catalog links without submission |
| [ess-own-request-status/SKILL.md](./Skills/ess-own-request-status/SKILL.md) | Optional live, authorized read-only request status; honest fallback when unavailable |

Each skill is independently uploadable. No source policies, employee records, credentials, binaries, screenshots, connector implementations, or exported agent solutions are included. The original scenario's resource file was an asset placeholder; this copy adds actual skill definitions and integration guidance.

## Optional Live Request-Status Contract

The following is a **proposed integration contract**, not a claim that these named operations exist in a built-in connector. Implement it through a supported, approved connector action, MCP server, or workflow-backed integration; map the logical names to actual tools in the agent instructions. Validate the implementation in the target tenant before enabling it.

| Logical operation | Allowed model-supplied input | Required behavior |
|---|---|---|
| `ListMyRequests` | Optional supported status filter, bounded page size, opaque continuation token | Return only the authenticated caller's permitted IT/catalog requests; support pending and completed items according to the configured filters |
| `GetMyRequest` | One validated request reference | Return the caller's authorized IT/catalog item, or a combined not-found/not-authorized result |

**No model-supplied identity fields:** user ID, email, employee number, requested-for, owner, tenant, or role must not establish authorization. The integration derives the caller from a validated authenticated context and enforces row-level ownership and request-type restrictions in the backend. Do not expose a broad ServiceNow table query and ask the model to add an ownership filter.

### Response Fields

| Field | Contract |
|---|---|
| `outcome` | `success`, `not_found_or_not_authorized`, `authentication_required`, or `service_error`; real tool failures must be surfaced, not converted into empty success |
| `retrievedAt` | Actual UTC retrieval timestamp from the integration |
| `items` | Authorized items only; empty only for a successful list with no matching results or a non-success outcome |
| Item `reference` | Stable portal-visible request reference |
| Item `title` | Sanitized short title suitable for employee display |
| Item `status` | Source system's status, not a model-inferred lifecycle state |
| Item `updatedAt` | Source timestamp, or null if not provided |
| Item `url` | Validated source-portal URL for the same authorized item |
| `nextToken` | Opaque continuation token for an incomplete list; null when complete |
| `message` | Safe error/limitation text with no internal payloads or other employees' details |

`GetMyRequest` returns one item on success. The backend must not return internal work notes, attachments, payroll/benefit records, medical information, or personal HR case data. Approval, cancellation, reassignment, comments, and request creation are not part of this contract.

### Integration Acceptance

1. Authenticate with a normal employee account, not only the maker or administrator.
2. List the employee's own pending and completed requests and verify source parity and pagination.
3. Request another employee's known reference; confirm that no record, title, URL, or existence detail is exposed.
4. Try spoofing identity in chat and manipulating a request reference or continuation token; the backend must still enforce the caller boundary.
5. Exercise expired authentication, access denial, throttling, timeout, and malformed output. None may look like a successful empty list.
6. Confirm read-only scopes/operations and that individual HR cases cannot be queried through the tool.

If any requirement is unmet, leave live status disabled and use the approved portal link. A working skill cannot compensate for a missing or overprivileged backend integration.

## Delivery Evidence to Maintain Privately

The deflection baseline remains the most important business artifact. Keep real ticket data, tenant configuration, source-access evidence, and pilot results in an approved restricted location, not in this public repository or skill packages.

| Artifact | Minimum contents |
|---|---|
| Qualification | Audience, licence/cloud/language matrix, owners, source systems, GHCP eligibility |
| Top-25 baseline | Category, monthly volume, authoritative answer location, owner, measurement period |
| Content audit | Domain, country/entity/category applicability, effective date, source, duplicates, owner, ACLs, remediation date |
| Tool mapping | Actual operation, logical contract, authentication mode, backend authorization, scopes, error behavior |
| Evaluation | Prompt, applicable source, identity/population, expected behavior, observed result over repeated runs |
| Pilot report | Adoption, correctly resolved questions, normalized ticket volumes, escalation, latency, credits, safety outcomes |
| Release record | Published model/configuration and skill revisions, approval, rollout groups, rollback owner |

## Maintaining Skills

Use **Build > Skills > Upload a skill** for each `SKILL.md`. After editing an uploaded skill, choose **... > Replace** and upload the new file; re-evaluate and republish.

Keep YAML names unique, lowercase, and aligned to folder names; descriptions should distinguish activation cases. Save UTF-8 without a byte-order mark. The included skills have no external file dependencies.

For a future ZIP skill, include `SKILL.md` and all referenced relative resources. Package stable templates/procedures rather than live policy copies or employee data. ZIP one skill at a time, not the whole scenario.

[Return to the runbook](../3.Runbook.md)
