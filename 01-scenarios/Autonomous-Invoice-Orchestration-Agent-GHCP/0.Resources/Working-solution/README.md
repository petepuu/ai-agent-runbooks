# Autonomous Invoice Orchestration Agent — Working Solution

This folder holds the **unmanaged** Power Platform solution **Autonomous Invoice Orchestration**, exported as `<solution unique name>_<version>.zip`. Keep only the current version here, so that [5. Working Solution](../../5.Working-solution.md) never points to two zip files.

## What the solution contains

| Component | Name |
|---|---|
| Dataverse table | **Auto Invoice Review**, with the 18 columns and the native row ID of the [data model](../Capability-matrix.md#dataverse-data-model) |
| Environment variables | `FinanceEmailAddress`, `ReviewsFolderPath`, `SourceFolderPath`, `SharePointDocumentLibrary` and `SharePointSiteURL` |
| Connection references | SharePoint, Microsoft Dataverse, Office 365 Outlook, Office 365 Users and Approvals |
| Workflows | **Invoice Intake** and **Invoice Approval** |
| Agent | **Invoice Processing Agent**, with its instructions, tools and the [invoice-extraction-review](../Skills/invoice-extraction-review/SKILL.md) skill |

## What it does not contain

- Connections, credentials and tokens. The import asks for a connection for each connection reference.
- Environment variable values that are specific to a tenant. Export no site URL, library name or email address. The import asks for the values.
- Table rows, SharePoint files, real invoices and test evidence.
- Closed [release gates](../README.md#release-gates). The solution is a starting point for development and test, not evidence that the gates pass.

## How to use it

Download the zip and follow [5. Working Solution](../../5.Working-solution.md) to import and test it. To build the same components by hand instead, use the [Runbook](../../3.Runbook.md).

## Updating the zip

1. Make the change in the development environment and test it with the [synthetic invoices](../Sample-invoices/README.md).
2. Check that the environment variables hold no values that should not be shared, as in [Runbook Step 2-3](../../3.Runbook.md#step-2-3-create-environment-variables).
3. In Power Apps, select the solution, then **Export solution**, publish the changes when asked and export it as **Unmanaged**.
4. Replace the zip in this folder.
