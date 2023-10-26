---
id: solution-prepare-optional-msteams
sidebar_position: 1
title: Microsoft Teams
---
## Integrate Watson Assistant with Microsoft Teams
#### Pre-Requisites to integrate Microsoft Teams
- Microsoft admin account
#### High level Steps
1. [Create Microsoft Teams custom extension](#create-microsoft-teams-custom-extension)
    1. App registration
    1. Create your bot
    1. Create your Teams app
1. [Export MS Teams App and import to MS Teams Platform](#import-bot-into-microsoft-teams)

### Create Microsoft Teams custom extension <a name="create-microsoft-teams-custom-extension"></a>
1. Navigate to "Integrations" tab. Within Channels section, find "Microsoft Teams" and click "Add", and "Add".
1. Click "Next" and stay on the "App registration" step
- **App registration**
    1. Open a new tab, and go to [Microsoft Azure App Registration](https://portal.azure.com/#view/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) and log in with your credentials.
    1. On the App registrations page, click "New registration". In the "Register an application" page, type a name, select the ```Accounts in any organizational directory (Any Microsoft Entra ID tenant - Multitenant)``` , then click "Register".
    1. Copy the Application (client) ID and paste it back to Watson Assistant's "App registration" step 3
    1. Navigate back to Azure App Registration tab, navigate to Overview. In the Essentials section, next to Client credentials, click "Add a certificate or secret".
    1. Click "New client secret"
    1. Complete the form with the recommended 180-day expiration date, or whichever selection best fits your use case, then click "Add". 
    1. Copy the string under the value section of the table and paste it back to Watson Assistant's "App registration" step 5.
- **Create your bot**
    1. In Watson Assistant, Click "Next" to "Create your bot" step.
    1. Go to [Microsoft's Bot Framework developer portal](https://dev.botframework.com/bots/new) and log in with your credentials.
    1. In the "Tell us about your bot" form, fill out your bot profile. 
    1. Under the Configuration section, in the Messaging endpoint field, navigate to watson assistant page and copy the Generated endpoint and paste into **Messaging endpoint** in the "Tell us about your bot" form.
    1. Under the Configuration section, make sure your app type is Multi-Tenant.
    1. In the **Microsoft App ID** field, paste your Application (client) ID. Agree the terms and Click "Register" to finish.
    1. Click "Ok" and it will show the "Connect to channels" page. 
    1. Under Add a featured channel, click **"Configure Microsoft Teams Channel"**. Make the necessary selections under Messaging and Publish sections that best fit your bot needs. Click "Save" to finish. Agree the terms and click "Agree".
- **Create your Teams app**
    1. In Watson Assistant, Click "Next" to "Create your Teams app" step.
    1. Open a new tab and go to the [Developer Portal](https://dev.teams.microsoft.com/home) and log in with your credentials.
    1. Click on the "Apps" tab, and click "New App". 
    1. Enter your app name and click Add. 
    1. Under **Configure -> Basic information**, complete the required information 
        - App names
        - Descriptions
        - Developer or company name: `IBM Client Engineering`
        - Website (must be a valid HTTPS URL): `https://www.ibm.com/client-engineering`
        - Privacy policy: `https://www.ibm.com/us-en/privacy`
        - Terms of use: `https://www.ibm.com/legal`
        - Application (client) ID 
    1. Click "Save".
    1. Under **Configure -> App Features**, click "Bot"
    1. In the Select an existing bot dropdown, select the new bot you created in the previous step.
    1. Select the following scopes of use: **Personal, Team, and Group Chat**. Click Save.
    1. Click Publish in the upper-right corner to publish your bot. Select **"Download the app package"** or the option that best fits your use case. A zip file will be downloaded.
    1. Navigate back to Watson Assistant page and click "Finish".

### Import Bot into Microsoft Teams <a name="import-bot-into-microsoft-teams"></a>
1. Open a new tab and go to  [Microsoft Teams](https://teams.microsoft.com/).
1. On the left hand side list, click "Apps".
1. Click "Manage your apps"
1. Click "upload an app"
1. Click "upload a custom app", and select the zip file downloaded from the previous step. Click "Add".
1. Start chatting with the Watson Assistant Bot in Microsoft Teams!
