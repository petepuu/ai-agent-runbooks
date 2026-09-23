---
name: hr-onboarding-checklist
description: |-
  Turns what's known about a new hire (role, start date, location, remote/office, equipment) into a
  formatted numbered onboarding checklist grouped into phases, posted directly in the chat reply. Use
  when asked for a "new employee onboarding checklist", "what does a new hire need to do", "onboarding
  steps for my new team member", or "checklist for someone starting next week".
  Do NOT use for emails — workflow HR replies use `hr-email-reply` instead. Do NOT use for
  general task lists unrelated to onboarding.
metadata:
  category: productivity
  icon: TaskListSquareLtr
---
## Purpose

A manager or HR agent describes a new hire in chat — role, start date, and whatever specifics are
known (remote or office, equipment needs, location). This skill turns that into a clear numbered
checklist, grouped into a few simple onboarding phases, and posts it directly as the chat reply. It is
not an email: nothing here is meant to be sent, only read in the conversation.

## When NOT to Use

- The request is a workflow HR email reply — use `hr-email-reply` with retrieved knowledge instead. Neither skill sends email.
- The request is a general to-do list or project task list with no onboarding context.
- The user wants an actual HR system ticket filed or a policy looked up — this skill only formats a checklist from what the user tells you.

## Sample chat input

```
New hire Maria starts Monday as a Sales rep in the Helsinki office, needs a laptop.
```

## Tools

None. This skill formats a plan; it does not retrieve policy or perform tasks.

## Inputs

The requested timeframe and supplied new-hire details: name, role, start date,
location, remote/office arrangement and equipment needs. Missing details stay unknown.

## Steps

1. **Read what's known.** Pull out role, start date, location, and remote/office/equipment details from the user's message.
2. **Group into standard phases.** Use a small, fixed set — e.g. Before Day 1, Day 1, First Week, First Month. Honor a requested timeframe and drop phases outside it or with nothing to say.
3. **Number the items within each phase sequentially**, starting at 1 in each phase, covering the standard onboarding areas (IT/accounts, workspace/badge, HR paperwork, manager intro, benefits, training, check-in).
4. **Fill placeholders for anything not provided** — e.g. exact tool names, portal links, or the manager's name — with neutral `[bracketed]` placeholders rather than guessing.

## Output format: the chat checklist

```
Onboarding checklist for <Name> — <Role>, starting <Start date>

**Before Day 1**
1. <task>
2. <task>

**Day 1**
1. <task>
2. <task>

**First Week**
1. <task>
2. <task>

**First Month**
1. <task>
```

## Worked example

Input: "New hire Maria starts Monday as a Sales rep in the Helsinki office, needs a laptop."

```
Onboarding checklist for Maria — Sales rep, starting Monday (Helsinki office)

**Before Day 1**
1. Order laptop and standard sales-team software accounts
2. Send Maria the office address and a [building access / badge] contact
3. [Manager name] confirms Day 1 meeting time

**Day 1**
1. Collect laptop and badge at [reception/IT desk]
2. Complete HR paperwork in [HR portal]
3. Meet [manager name] and the immediate team

**First Week**
1. Set up email, calendar and [CRM tool] access
2. Walk through Sales team processes and key contacts
3. Enroll in benefits via [benefits portal]

**First Month**
1. Complete [company] onboarding/training modules
2. First week/month check-in with [manager name]
```

## Guardrails

- Never invent company-specific system, tool or policy names the user didn't provide — use neutral `[bracketed]` placeholders instead.
- Keep phases and numbering clear: number sequentially within each phase, and drop phases with nothing to say.
- This is a chat answer only — never format or send it as an email.
- Label generic tasks as planning suggestions to confirm, not verified company requirements. Do not claim to order equipment, schedule meetings or complete tasks.

## Results and Failure Handling

Return the numbered plan in chat. Use placeholders for missing details; if the
request cannot be understood, ask one focused clarification rather than inventing
a plan. For policy questions, use the agent's normal knowledge-based behavior instead.
For requests to perform tasks, explain the planning-only scope without claiming success.
