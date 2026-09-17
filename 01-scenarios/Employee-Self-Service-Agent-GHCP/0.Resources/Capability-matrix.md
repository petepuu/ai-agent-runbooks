# Capability and Integration Contracts

Four runtime skill definitions are supplied. No Studio agent, knowledge connection, backend implementation, vendor operation ID or live test result is supplied. **Baseline ON** below means intended enablement after configuration and acceptance, not a currently running capability. The status extension is **proposed/OFF**.

## Capability matrix

| Capability ID / linked SKILL.md | Behavior and boundary | Tools or knowledge | Actual implementation / operation ID | Dependencies | Profile / access / approval |
|---|---|---|---|---|---|
| [hr-policy-guidance](Skills/hr-policy-guidance/SKILL.md) | Explain applicable published HR policy, no personal values or case advice | Approved HR knowledge and trusted HR contact route | Instruction definition supplied, no external operation tool | Authoritative policy corpus, caller-permission and applicability tests | Baseline ON after gates, permitted evidence only, no transaction approval needed |
| [it-support-guidance](Skills/it-support-guidance/SKILL.md) | Explain safe KB troubleshooting, no device/account operations | Approved IT knowledge and trusted IT support route | Instruction definition supplied, no external operation tool | Applicable safe KB passages and permission tests | Baseline ON after gates, no credentials collected or changes executed |
| [request-catalog-navigation](Skills/request-catalog-navigation/SKILL.md) | Locate exact HR/IT request form, no order/submission | Approved ServiceNow Catalog or SharePoint request directory | Instruction definition supplied, no external operation tool | Permitted catalog descriptions, actual destination URLs and applicability | Baseline ON after gates, link only, portal independently authorizes employee actions |
| [own-request-status](Skills/own-request-status/SKILL.md) | Read caller's own allowlisted IT request status, no HR cases or other users | Proposed `ReadOwnRequests` custom contract, optional trusted portal route | No executable implementation or vendor operation ID, OFF | Caller-bound backend, implemented tool/auth binding, status gates S1-S3 | Optional status profile only, backend ownership checks on every read, no write/approval action |

## Profiles and dependency rules

| Profile | Enabled skills | External operation tools | Release boundary |
|---|---|---|---|
| Baseline | First three rows, each independently disableable | None | G1-G4 and D1-D2 before restricted pilot, G5 before broad release |
| Optional own-request status | First three plus fourth row | Only implemented and verified `ReadOwnRequests` binding | S1-S2 before authorized real-data pilot, S3 before broader extension release |

All unlisted actions are OFF. Each skill works alone with its declared dependencies; no skill calls or requires another skill. Global instructions apply even when the agent does not select a skill. For composed requests, do the allowed parts and explicitly stop the unavailable parts.

Gates are defined in the [Runbook](../3.Runbook.md#release-gates). Isolated synthetic testing is permitted before release gates close. A production pilot requires prior owner approval and limited access, not approval derived from its own future results.

Disabling a skill is not permission revocation. Coordinate trusted profile instructions, configured knowledge, tool exposure, backend policy, alternate paths, distribution and stale sessions. Retain shared knowledge connections needed by still-enabled capabilities.

## Knowledge and contact contract

The terms HR knowledge, IT knowledge and request directory are **logical source roles**, not exact connection names or search API names. The delivery owner records real connections and binds them through Build > Knowledge.

| Role | Required evidence | Boundary / owner |
|---|---|---|
| HR knowledge | Published title, actual source URL/ID, applicable population and effective period, supporting passages | HR owner resolves conflicts, no individual HR records |
| IT knowledge | Product/device context, approved troubleshooting steps, source title/link and support route | IT owner excludes unsafe or obsolete instructions |
| Request directory | Item title, description, actual destination URL, scope and documented prerequisites | Catalog owner validates permitted exact links, no submission |
| Contact / portal configuration | HR/IT route or portal URL approved by the owner, available to the intended caller | Trusted configuration or authorized evidence, never a guessed contact |

Use source URLs and identifiers only when provided by trusted configuration or retrieval. Show missing applicability or freshness metadata as a limitation, not a fabricated value. A source publication/index time is not automatically a policy effective date. When a lookup is empty and the system exposes no reason, do not claim the item does not exist or that access was denied.

## Proposed own-request read contract

`ReadOwnRequests` is a **proposed custom contract label**, not a built-in vendor action. An integration owner must implement it through a documented workflow or MCP route, record the real operation ID and validate the complete identity chain. Importing the status skill does none of this.

### Inputs

| Field / context | Proposed rule |
|---|---|
| Authenticated caller | Required trusted execution context, never a prompt-supplied ID/email. Backend validates token issuer/audience/tenant as applicable and resolves the service-system user |
| `requestRef` | Optional specific request locator, bounded format validated server-side, mutually exclusive with list filters |
| `stateFilter` | For list mode only: `open`, `completed` or `all`, default `open`. Backend owns state mapping |
| `pageSize` | Integer 1–20, default 10, server-enforced |
| `cursor` | Optional opaque continuation from this tool, bound to caller/filter/snapshot and expiry. Reject tampering or reuse by another caller |

There are no employee selectors, arbitrary query expressions, table names or requested-field inputs. The backend rejects unknown fields and malformed combinations.

### Outputs

| Field | Proposed rule |
|---|---|
| `outcome` | `ok`, `empty`, `denied`, `unavailable`, `partial`, `failed`, `pending` or `unknown`, only as observed |
| `snapshotAt` | Backend-supplied observation timestamp, do not invent one if omitted |
| `items` | Only caller-owned, allowlisted non-sensitive IT service requests, with fields below |
| `complete` | Whether all results for the authorized query are present, not whether the business requests are completed |
| `nextCursor` | Scoped continuation only, absent when no further page is available |
| `correlationRef` | Safe opaque diagnostic reference, no credentials or employee details |
| `retryAfterSeconds` | Optional bounded retry hint for a transient read, never a success indicator |

Each item contains only `requestRef`, an owner-approved static `catalogLabel`, `status`, `updatedAt` and `portalUrl` when actually available. `status` is the backend-approved display state; no model-inferred mapping to completion. `portalUrl` must be an approved navigation destination without credentials. Reject or safely suppress unexpected fields before returning data to the agent.

### Permitted records, identity and connection ownership

- Backend enforces **requester ownership**, tenant and record-type allowlists before fetching/returning fields. Being an approver, manager, assignee or maker does not grant this tool access.
- Exclude HR cases, grievance/disciplinary content, benefits claims, medical records, payroll, attachments, comments, descriptions and other free-text or personal fields.
- Denied or nonexistent specific locators must not reveal record existence, title or owner. Use an indistinguishable safe response where disclosure is a risk.
- The integration owner owns the connection and credentials in the approved secret store, the identity owner approves caller propagation, and IT/privacy owners approve record and field scope.
- A delegated caller identity is preferred where supported. Any service identity requires independent backend enforcement of the authenticated caller's scope before release. A shared connection by itself is insufficient.
- The employee's request initiates a read. There is no approval grant, status-change action or write confirmation in this contract. Missing mapping is denial/unavailable, not a fallback to the maker.

### Concurrency, retries and asynchronous results

This is a bounded read, not an atomic transaction. Return a snapshot time and item update times. Each page reauthorizes the caller. Bind cursors to the original query/snapshot where the backend supports it; if a consistent continuation cannot be established, return partial and request a fresh bounded read. Do not merge snapshots into a claim of a complete current list.

No business write occurs, so write idempotency and write reconciliation are not applicable. Correlation IDs support diagnosis, not approval. Permit at most one retry of an explicitly transient failed/unavailable read when the tool provides a retry hint and the host can honor it, retaining caller scope. Otherwise stop. Never retry a denial through a different API or identity.

The initial design expects synchronous reads and supplies no poll operation. Unexpected `pending` or `unknown` is reported as unfinished/unknown, never a completed read or business completion. If an implementation requires asynchronous jobs, S1 remains open until the integration owner specifies and tests a separately authorized status contract and updates the skill and matrix. Do not invent a polling tool at runtime.

## Observable outcomes

| Outcome | Required response |
|---|---|
| OFF | Explain capability is unavailable, zero operation calls, use trusted portal route if present |
| Denied | No restricted details, no alternate identity/tool, no record-existence confirmation |
| Missing / empty | Say no usable evidence or no own results for the stated authorized query, only claim complete emptiness when backend confirms complete |
| Unavailable / failed | State observed limitation and safe retry/support option, preserve already-supported independent answers |
| Conflicting / stale policy | Do not choose an unsupported rule, show permitted conflict or freshness gap and trusted owner route |
| Partial | Return only supported passages or allowed items, state missing coverage/page, never claim exhaustive results |
| Pending | Distinguish unfinished lookup from a request waiting for approval, no completion claim |
| Unknown | State uncertainty, no guessed cause, state mapping, URL or outcome |

## Acceptance ownership

The [Sample Prompts](../4.Sample-prompts.md#acceptance-cases) contain one positive and one boundary test per matrix row, failure cases and a composed sequence. Expected outcomes are not test results. The test owner records real source references, profile/version, execution identity, visible evidence and defects in an access-controlled result store. Security and scope boundaries must pass regardless of aggregate answer quality.
