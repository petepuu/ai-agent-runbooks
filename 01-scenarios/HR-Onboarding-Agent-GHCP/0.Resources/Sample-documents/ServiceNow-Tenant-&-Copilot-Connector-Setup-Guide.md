# HR Onboarding Agent (GHCP) - ServiceNow Knowledge Setup

> **Purpose:** Configure sample ServiceNow knowledge for the GHCP agent.
>
> **Documentation reviewed:** September 15, 2026
>
> **Return to:** [Runbook](../../3.Runbook.md)

## Sample Knowledge

The included [Contoso Employee Handbook](./Contoso%20Employee%20Handbook.docx) and [Wellness Benefits Sample Knowledge](./Wellness%20Benefits%20Sample%20Knowledge.docx) documents are demonstration source material. Publish their content as ServiceNow knowledge articles for the lab; do not treat their benefits or policies as real employer commitments.

The `../Images` directory contains historical reference assets. Use this guide and the linked product documentation for current setup instructions.

## Prerequisites

| Requirement | Owner |
|---|---|
| ServiceNow instance and authority to publish test articles | ServiceNow administrator / HR content owner |
| Microsoft 365 connector administration and current connector licensing/eligibility | Microsoft 365 administrator |
| Correct ServiceNow crawl identity, roles, and user-criteria access | ServiceNow administrator |
| Test users with both permitted and denied article access | Identity / HR administrator |
| A GHCP agent using Microsoft authentication | Copilot Studio maker |

## 1. Prepare the Source

1. Use an existing approved test instance or obtain a developer instance through [ServiceNow Developer](https://developer.servicenow.com).
2. Follow organizational sign-in and MFA requirements. Do not weaken authentication for the lab.
3. Create or select a test knowledge base appropriate for HR demonstration content, with access controls approved by the HR content owner.
4. Create **Contoso Employee Handbook** and **Comprehensive Guide to Health and Wellness Benefits** articles from the sample documents.
5. Publish using the appropriate approval workflow, set validity/applicability information, and verify source permissions with test users.

## 2. Configure the Microsoft Copilot Connector

Follow the current [ServiceNow Knowledge deployment guide](https://learn.microsoft.com/en-us/microsoft-365/copilot/connectors/servicenow-knowledge-deployment).

1. In Microsoft 365 admin center, open **Copilot > Connectors > Gallery > ServiceNow Knowledge**.
2. Configure the instance URL, authentication, content filters, and crawler permissions using the supported deployment method.
3. Choose **Only people with access to this data source**, not Everyone, when indexing permission-restricted content.
4. Map ServiceNow identities to Microsoft Entra identities and validate allowed/denied article access.
5. For HR Service Delivery (`sn_hr_core`) articles, verify the required `sn_hr_core.content_reader` or `sn_hr_core.admin` role per the current deployment guide. Missing HR permissions can lead to incorrectly interpreted restrictions; choose the least-privileged supported role.
6. If ServiceNow user criteria contain scripts, use the documented Advanced permission flow. The Simple flow cannot evaluate scripted criteria correctly for all access cases.
7. Create the connection, wait for **Ready**, and verify indexed items and a completed crawl. Run a full crawl after changing the sample content when appropriate.

Indexing is asynchronous. Content and identity synchronization have separate schedules; do not promise that a just-published article or access change is immediately reflected in agent answers.

## 3. Connect and Validate the GHCP Agent

1. Use **Build > Knowledge > Add knowledge** and select the ServiceNow/Copilot-connector connection.
2. Confirm actual access with the GHCP agent's configured **Authenticate with Microsoft** setting.
3. Ask a known handbook question as an authorized non-maker user and confirm a valid article citation.
4. Repeat with a denied user and verify that the article content and restricted source links do not leak.
5. Change a test article or permission, wait for the appropriate synchronization, and repeat the check.
6. For the optional email workflow, independently verify the effective retrieval identity and recipient-safe content scope. Sender email metadata does not establish delegated access.

Do not proceed to production email replies if source permission mapping, identity synchronization, or the workflow retrieval boundary is unresolved.

## References

- [ServiceNow Knowledge deployment](https://learn.microsoft.com/en-us/microsoft-365/copilot/connectors/servicenow-knowledge-deployment)
- [GHCP knowledge sources](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/knowledge-sources-overview)
- [Add knowledge to GHCP agents](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/knowledge-add-existing-copilot)
- [GHCP authentication settings](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/settings-overview)
