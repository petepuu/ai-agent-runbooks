# Runtime skill index

Import only approved capabilities into the **GitHub Copilot harness in Microsoft Copilot Studio**, in this matrix order. Each file is standalone; no sibling file or asset is required at runtime. Tool dependencies must still be configured separately. All live aliases are proposed/unimplemented and OFF in this authoring package.

| Order | Capability / standalone upload |
|---|---|
| 1 | [ess-knowledge-answer](ess-knowledge-answer/SKILL.md) |
| 2 | [ess-request-navigation](ess-request-navigation/SKILL.md) |
| 3 | [sn-hr-create-case](sn-hr-create-case/SKILL.md) |
| 4 | [sn-hr-list-cases](sn-hr-list-cases/SKILL.md) |
| 5 | [sn-hr-case-details](sn-hr-case-details/SKILL.md) |
| 6 | [sn-hr-case-updates](sn-hr-case-updates/SKILL.md) |
| 7 | [sn-hr-update-case](sn-hr-update-case/SKILL.md) |
| 8 | [sn-hr-add-attachment](sn-hr-add-attachment/SKILL.md) |
| 9 | [sn-it-create-ticket](sn-it-create-ticket/SKILL.md) |
| 10 | [sn-it-list-tickets](sn-it-list-tickets/SKILL.md) |
| 11 | [sn-it-ticket-details](sn-it-ticket-details/SKILL.md) |
| 12 | [sn-it-ticket-updates](sn-it-ticket-updates/SKILL.md) |
| 13 | [sn-it-update-ticket](sn-it-update-ticket/SKILL.md) |
| 14 | [sn-it-add-attachment](sn-it-add-attachment/SKILL.md) |
| 15 | [wd-get-base-compensation](wd-get-base-compensation/SKILL.md) |
| 16 | [wd-get-certifications](wd-get-certifications/SKILL.md) |
| 17 | [wd-get-company-code](wd-get-company-code/SKILL.md) |
| 18 | [wd-get-cost-center](wd-get-cost-center/SKILL.md) |
| 19 | [wd-get-emergency-contacts](wd-get-emergency-contacts/SKILL.md) |
| 20 | [wd-get-employee-id](wd-get-employee-id/SKILL.md) |
| 21 | [wd-get-employment-information](wd-get-employment-information/SKILL.md) |
| 22 | [wd-get-language-information](wd-get-language-information/SKILL.md) |
| 23 | [wd-get-national-ids](wd-get-national-ids/SKILL.md) |
| 24 | [wd-get-passports](wd-get-passports/SKILL.md) |
| 25 | [wd-get-service-anniversary](wd-get-service-anniversary/SKILL.md) |
| 26 | [wd-get-visas](wd-get-visas/SKILL.md) |
| 27 | [wd-update-email](wd-update-email/SKILL.md) |
| 28 | [wd-update-phone](wd-update-phone/SKILL.md) |

The baseline uses the first two after source validation. Enabling or deleting a skill alone does not implement or revoke a tool. Coordinate the [capability matrix](../Capability-matrix.md), actual tool exposure, backend permissions and active sessions.

Upload through **Build > Skills > Upload a skill**. If assets are added later, use a ZIP containing `SKILL.md` and those assets. Do not import these files as authoring-assistant skills or run ESS Developer Kit commands as if they were GHCP deployment steps.
