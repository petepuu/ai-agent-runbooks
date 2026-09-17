# ServiceNow Tenant & Copilot Connector Setup Guide

## Overview

This guide walks through two main tasks:

1. **Create a free ServiceNow Developer Instance** — required as the external
   knowledge source for the HR Onboarding Agent
2. **Install and configure the ServiceNow Knowledge Copilot Connector** — indexes
   ServiceNow knowledge base articles into M365 so they can be used as agent knowledge

> **Before You Start**
> - Use an approved isolated developer instance for sample articles; production needs HR-approved content and a supported governed environment.
> - Verify tenant connector availability, licensing, admin roles and data policies rather than assuming defaults.
> - Follow [ServiceNow prerequisites](https://learn.microsoft.com/en-us/microsoft-365/copilot/connectors/servicenow-knowledge-admin-setup) and [connector deployment](https://learn.microsoft.com/en-us/microsoft-365/copilot/connectors/servicenow-knowledge-deployment), including source permissions and identity mapping.
> - Screenshots are editing placeholders, not revalidated UI. Verify current screens; follow the secure configuration steps, not superseded screenshot selections.

---

## Prerequisites

| Requirement | Details |
|---|---|
| Email address | Required to sign up for a ServiceNow Developer account |
| M365 Admin access | Required to install and configure the Copilot Connector |
| Microsoft 365 Copilot license | Required for the tenant where the connector will be installed |

---

## Part 1: Create a Free ServiceNow Developer Instance

### Step 1-1-1. Sign Up for a ServiceNow Developer Account

1. Open your browser and go to https://developer.servicenow.com

> ![ServiceNow Developer Program home page](../Images/009.png)

2. Click **"Sign In"** in the top right corner
3. Click **"New user? Get a ServiceNow ID"**

> ![Get a ServiceNow ID button](../Images/010.png)

4. Fill out the registration form with the following details:
   - Email
   - First Name
   - Last Name
   - Country
   - Password
   - Confirm Password
   - Check the reCAPTCHA box
   - Accept the Terms of Use

5. Click **Sign Up**

> ![ServiceNow ID registration](../Images/011.png)
>

---

### Step 1-1-2. Verify Your Email

1. Check your inbox for a verification email from `signon@service-now.com`
2. Click **"Verify Email"** in the email

> ![Verification email](../Images/012.png)

> **Screenshot to replace:** verification-link token redacted. Verify through your own account's email; never reuse or publish verification links.

3. After verification, log in to your ServiceNow account
4. On first login, MFA setup will appear on the next screen.

> ![MFA setup screen](../Images/013.png)
>

   > **Security requirement:** complete the organization's required MFA enrollment; do not skip MFA for convenience.
   > **Screenshot to replace:** show completed MFA enrollment, not the Skip path.

---

### Step 1-1-3. Complete Initial Setup

1. In the **"Getting Started"** dialog, click **"No"** (I need a guided experience)

> ![Getting Started screen](../Images/014.png)
>

2. In the next window, select your preferred options, check the **Terms of Use** checkbox, and click **"Finish Setup"**

> ![Set-up screen](../Images/015.png)
>


---

### Step 1-1-4. Request a ServiceNow Instance

1. You should now be in your **ServiceNow Developer Dashboard**
2. Click **"Request Instance"** in the top right corner

> ![Request Instance screen](../Images/016.png)
>

3. Choose an available supported release approved for the test and click **"Request"**

> ![Request an Instance screen](../Images/017.png)
>

   > Wait for the instance to report ready; provisioning time varies.

4. Once the instance is ready, a Manage my instance screen will appear with your instance details:
   - **Instance URL**: `https://dev[XXXXXX].service-now.com`
   - **Username**: `admin`
   - **Current password**: *(auto-generated)*

> ![Manage my instance screen](../Images/018.png)
>

   > Use admin access only for authorized instance setup. The connector must use its own scoped integration identity, not this administrator.

---

### Step 1-1-5. Switch to Admin Role (Skip this step if you have the 'Admin' user role assigned.)

1. Click **"Actions"** in the top right corner and **"Change User Role"** from the dropdown.

> ![Change user role screen](../Images/019.png)

2. In the **"Change User Role"** dialog, select **"Admin"**

>![Change user role - admin selection screen](../Images/020.png)

   > ⚠️ **Important**: Admin access is required to configure the ServiceNow instance for the Copilot Connector.

3. Click **"Change"** to confirm
4. Click **"Done"** once the change is confirmed

>![Role change screen](../Images/021.png)

5. Click **"Cancel"** to exit the dialog

---

### Step 1-1-6. Secure Admin Credentials

1. On **"Manage my instance"**, retrieve setup credentials only into your approved secret manager for Part 2. Do not reveal passwords for a screenshot or store them in documentation.

>![Manage my instance page](../Images/022.png)

   - **Instance URL**: `https://dev[XXXXXX].service-now.com`
   - **Username**: `admin`
   - **Password**: *(shown in the manage password screen)*

   > ⚠️ **Important Notes on Instance Availability**:
   > - ServiceNow instances will **hibernate** after a period of inactivity
   > - Attempts to access the instance via Graph or Power Platform connectors will **fail** when the instance is hibernating
   > - Validate the instance is **awake** before testing
   > - Follow current ServiceNow Developer Program inactivity/reclamation rules; connector access alone does not establish instance availability

---

## Part 2: Configure Admin Access in ServiceNow Instance

### Step 1-1-7. Log In to Your ServiceNow Instance as Admin

1. Open your browser and go to your **Instance URL** from Step 1-1-6
   `https://dev[XXXXXX].service-now.com`
2. Log in with:
   - **Username**: `admin`
   - **Password**: *(from Step 1-1-6)*

> ![ServiceNow instance login page](../Images/023.png)

---

### Step 1-1-8. Verify Knowledge Bases are Available

1. Click the **"All"** tab in the top navigation
2. Type **"knowledge bases"** in the search field
3. Select **"Knowledge Bases"** under **Knowledge > Administration**

> ![Knowledge Bases selection screen](../Images/024.png)

4. Confirm the following four default Knowledge Bases are listed:

   | Knowledge Base | Description |
   |---|---|
   | KCS Knowledge Base (demo data) | KCS Demo KB |
   | Known Error | Default knowledge base for Known Errors |
   | IT | The ACME North America IT Service Desk Knowledge Base |
   | Knowledge | Knowledge Base for general Knowledge users |

> ![Knowledge Bases list](../Images/025.png)

   > These are example default bases. Restrict connector ingestion to the approved HR test corpus rather than indexing all demo content. Confirm the actual base inventory, article/base user criteria and required REST/table ACLs.

---

## Part 3: Install the ServiceNow Knowledge Copilot Connector

> Use a separate **scoped integration identity** for ingestion, with privileges required by the selected supported authentication method. Setup admin credentials are not the runtime connector identity. Follow the official prerequisite/authentication sections before creating the connection.

### Step 1-1-9. Add a New Connection in M365 Admin Center

1. Log in to **M365 Admin Center** → https://admin.microsoft.com
2. Navigate to **Copilot** → **Connectors** → **Connectors** → **Gallery**
3. Browse the connector gallery

> ![M365 Admin Center - Copilot Connectors page](../Images/026.png)

4. Select **ServiceNow Knowledge**
5. Open its connection setup

> ![ServiceNow Knowledge - Add screen](../Images/027.png)

---

### Step 1-1-10. Configure the Connection Settings

1. Click **"Custom setup"** in the upper right of the screen

> ![Custom setup screen](../Images/028.png)

2. Fill in the following fields in the **Setup** tab:

   | Required Field | Value |
   |---|---|
   | Display name | `ServiceNow` *(or a unique name, e.g., `ServiceNowKB5`)* |
   | ServiceNow URL | `https://dev[XXXXXX].service-now.com` *(your instance URL from Step 1-1-6)* |
   | Authentication type | Security-approved method from the current deployment guide; **Federated Auth** is recommended there |
   | Integration identity | Dedicated identity with only the required connector roles and approved corpus access |
   | Authentication material | Configure through the approved connection/secret mechanism; never use shared admin credentials or copy values into this guide |
   | Notice | Review and acknowledge the actual notice after verifying permissions |

   > Complete the selected method's prerequisites in [connector deployment](https://learn.microsoft.com/en-us/microsoft-365/copilot/connectors/servicenow-knowledge-deployment#choose-authentication-type). Federated Auth, OAuth 2.0 and Microsoft Entra ID OpenID Connect have distinct setup requirements. Do not infer authentication from the Basic/admin screenshot.

> **Screenshot to replace:** approved authentication method and scoped integration identity.
> ![Custom setup screen](../Images/029.png)

---

### Step 1-1-11. Authenticate the Connection

1. Complete the selected method's authorization/sign-in process and wait for authentication to complete

> **Screenshot to replace:** the selected authentication method, not shared admin Basic authentication.
> ![Authentication screen](../Images/030.png)

2. Confirm the connection's successful authentication state. For methods with field validation, inspect its check marks; success alone does not prove correctly scoped data access.

> **Screenshot to replace:** authentication success for the scoped identity.
> ![Green Check Marks](../Images/031.png)


---

### Step 1-1-12. Configure User Access Permissions

1. Click the **"Users"** tab at the top of the setup panel

2. Under **Access Permissions**, select **Only people with access to this data source**. Preserve source permissions, not Everyone.

> **Screenshot to replace:** select source-based permissions instead of Everyone.
> ![Users - Access Permissions screen](../Images/032.png)

3. Configure employee identity mapping and verify knowledge-base and article user criteria. If any advanced scripts are present, configure **Advanced** flow and its required ServiceNow REST setup; Simple flow does not evaluate them correctly. Validate HRSD criteria where applicable and restrict the corpus before ingestion.
4. Test source access as permitted, denied and revoked employees after identity/content synchronization. Broad ingestion identity access must not become employee access.

---

### Step 1-1-13. Create the Connection

1. Go back to the **"Setup"** tab
2. Check all required values, scoped authentication, corpus filters, mapping and source-permission settings
3. Click **"Create"**

> **Screenshot to replace:** approved authentication and source-permission configuration before Create.
> ![Connection creation screen](../Images/033.png)
>

4. The button will display **"Creating connection"** while the process runs

---

### Step 1-1-14. Add a Connector Description

1. Once the connection is created, a success screen will appear:
   **"Created connection — ServiceNow (ServiceNowKB[X])"**

> ![Created connection screen](../Images/034.png)

2. Click **"Auto suggestion"** to have Copilot generate a description automatically

> ![Auto suggestion screen](../Images/035.png)

   > 💡 **If "Auto suggestion" does not work**, you can add the description later:
   > - Wait until the connection status shows **"Ready"**
   > - Click the connection in the list → Click **"Edit description"**
   > - Paste the following sample description:

   ```
   Approved HR policy, benefits, wellness, workplace and onboarding articles
   for employees. Use applicable current articles with source citations.
   This index does not provide personal HR records or live transactions.
   ```

3. Click **"Save"** and **"Done"**

   > Wait until the connection is **Ready** and both content and identity processing have completed. Inspect actual status and errors; a fixed wait does not establish readiness.

> ![Save and Done screen](../Images/036.png)

---

## Part 4: Verify the Connection

### Step 1-1-15. Verify Indexed Content via Microsoft Search

1. Navigate to https://microsoft365.com and log in as a permitted **non-maker employee** in the test group
2. After clicking **`Search*`** on the navigation, type **`KB0*`** in the search, and press **Enter**

> ![Microsoft 365 search results showing KB articles from ServiceNow](../Images/037.png)

3. Click **"All Sources"** filter → Select your ServiceNow connector (e.g., `ServiceNow-KB`)
4. Confirm that KB articles from ServiceNow are listed in the results

>![Search results filtered by ServiceNow-KB showing knowledge articles](../Images/038.png)

5. Verify known HR article IDs, source URLs, applicability and versions. Repeat as denied and revoked employees, including parent-base changes. For workflow use, prove recipient-safe filtering before retrieval and current entitlement before send; the workflow connection owner's rights do not establish the email sender's rights.

---

### Step 1-1-16. Verify via M365 Copilot Prompts

Use the following prompts in **M365 Copilot (Teams)** to verify the connector is working:

> Restrict the test to the configured HR source and inspect citations and access. Do not treat a web-content switch as proof of grounding; GHCP has no general-knowledge toggle.

| Scenario | Prompt |
|---|---|
| Test basic retrieval | `Find the approved employee handbook and wellness articles. List their titles, source links and applicable populations.` |
| Test thematic analysis | `What do the approved HR articles say about onboarding? Cite each supported step and mark missing details.` |
| Test content drafting | `Draft a reply about the published wellness benefit. Cite the applicable source; do not send or save an email.` |

---

## Summary Checklist

| Step | Task | Status |
|---|---|---|
| 1-1-1 | ServiceNow Developer account created | ☐ |
| 1-1-2 | Email verified and account activated | ☐ |
| 1-1-3 | Initial setup completed | ☐ |
| 1-1-4 | ServiceNow Dev instance requested and ready | ☐ |
| 1-1-5 | User role changed to Admin | ☐ |
| 1-1-6 | Admin setup credentials secured in the approved secret manager | ☐ |
| 1-1-7 | Logged in to ServiceNow instance as Admin | ☐ |
| 1-1-8 | Four default Knowledge Bases confirmed | ☐ |
| 1-1-9 | New connection added in M365 Admin Center | ☐ |
| 1-1-10 | Scoped connection identity, authentication and corpus configured | ☐ |
| 1-1-11 | Authentication completed and scoped access verified | ☐ |
| 1-1-12 | Source permissions, identity mapping and advanced criteria validated | ☐ |
| 1-1-13 | Connection created successfully | ☐ |
| 1-1-14 | Connector description added | ☐ |
| 1-1-15 | Indexed content verified via Microsoft Search | ☐ |
| 1-1-16 | M365 Copilot prompts and permitted/denied/revoked identities tested | ☐ |

---

## Troubleshooting

| Issue | Solution |
|---|---|
| "Sign in" button not appearing | Check the current authentication flow, browser/session state and service status; report a persistent issue to the administrator. |
| "Create" button remains disabled | Review required fields, authentication, permission configuration and validation errors. |
| "Auto suggestion" does nothing | Add description manually after connection reaches "Ready" state. |
| Connector returns errors when querying | Verify the ServiceNow instance is not hibernating — log in directly first. |
| Instance reclaimed by ServiceNow | Re-request a new instance and reconfigure the connector. |
| Items indexed count is 0 after sync | Inspect content and identity processing status, corpus filters and errors; run an authorized full crawl after fixing configuration. |

For additional troubleshooting, refer to:
[Troubleshooting the ServiceNow Knowledge Microsoft Copilot connector — Microsoft Learn](https://learn.microsoft.com/en-us/microsoftsearch/troubleshoot-servicenow-knowledge-connector)

---

## Related Resources

- [Step-by-step runbook](../../3.Runbook.md)
- [Add HR documents to ServiceNow](../../3.Runbook.md#step-1-2-add-hr-documents-to-servicenow-knowledge-base)
- [Release readiness](../Release-readiness.md)
- [ServiceNow Developer Program](https://developer.servicenow.com)
- [Microsoft 365 Admin Center](https://admin.microsoft.com)
- [ServiceNow Knowledge connector deployment](https://learn.microsoft.com/en-us/microsoft-365/copilot/connectors/servicenow-knowledge-deployment)
