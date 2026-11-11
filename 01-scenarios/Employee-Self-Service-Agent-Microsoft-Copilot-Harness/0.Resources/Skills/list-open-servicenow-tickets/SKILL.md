---
name: list-open-servicenow-tickets
description: "Use when the signed-in user asks to list their own open or active ServiceNow IT incidents. Show Ticket Number, Severity and Short Description. Not for HR cases, other people, closed/resolved tickets, counts-only reporting or mutations."
---
# List Open ServiceNow Tickets

## Scope

Read only the signed-in user's active IT incidents. A request naming another person does not authorize a broader search.

## Tools

Use the approved employee-delegated `GetIncidents` binding. The integration must enforce the same authenticated caller even if the vendor account has wider access.

## Inputs

Authenticated employee context; no arbitrary caller, table or query from chat.

## Procedure

1. Call `GetIncidents` with `tableType="incident"`, `sysparm_query="active=true^caller_id=javascript:gs.getUserID()^ORDERBYnumber"`, `sysparm_fields="number,severity,short_description"`, `sysparm_display_value=true` and `sysparm_limit=50`. This binding is permitted only when `gs.getUserID()` is validated to resolve the actual employee, not a shared connection owner.
2. Never replace the own-caller filter with a name search, including for an administrator. Refuse other-person/team scope without looking it up.
3. If an authorized complete response is empty, state that no open incidents were returned. Distinguish failure, denied access and incomplete coverage from no records. At the 50-record limit disclose possible truncation; do not imply a complete list or total.
4. Render records as a Markdown table with exactly these columns: **Ticket Number | Severity | Short Description**. Preserve returned descriptions and severity labels; do not summarize or truncate them. Escape Markdown table characters/newlines for safe rendering without dropping content. Link a number only if the binding returns a validated incident URL or an authorized ID plus configured portal root; otherwise keep it plain text.

## Results and Failure Handling

On failure, state that the lookup did not succeed; never invent rows or use another identity. Show authorized partial results with their coverage limit. Do not fetch extra fields solely to guess links, disclose internal notes, or infer service-desk trends from one employee's incidents. Route HR cases to their separately enabled capability.
