---
id: solution-prepare
sidebar_position: 1
title: Prepare
---

## Pre-Requisites
- Access to any Watson Assistant instance on a Lite, Plus Trial, Plus, or Enterprise plan
- Access to an instance of Watson Discovery
- Access to WatsonX platform
- Access to an instance of Watson Machine Learning
- Access to a NeuralSeek plan (for NeuralSeek integration only)

## High-level Steps to develop working solution
1. [Setup Watson Discovery](#setup-watson-discovery)
1. Options
    1. [Setup NeuralSeek Lite and integrate with Watson Discovery](#setup-NS-WD)
    1. [Setup NeuralSeek WatsonX version](#setup-NS-WX)
    1. [**Setup WatsonX and integrate with Watson Discovery**](#setup-WX-WD)
3. [Setup Watson Assistant](#3-setup-watson-assistant)
4. [Create NeuralSeek custom extension](#4-create-neuralseek-custom-extension)
5. [Create WA action to trigger NeuralSeek Search](#5-create-wa-action-to-trigger-neuralseek-search)



### Integrate Watson Assistant with Microsoft Teams 
#### Pre-Requisites to integrate Microsoft Teams
- Microsoft admin account
#### High level Steps
1. [Create Microsoft Teams custom extension](#create-microsoft-teams-custom-extension)
1. [Export MS Teams App and import to MS Teams Platform](#import-bot-into-microsoft-teams)


## Step-by-Step Instructions

### 1. Setup Watson Discovery<a name="setup-watson-discovery"></a>
1. New projects, input Project Name, select an option "None of the above — I’m working on a custom project", click "Next"
1. select "Web Crawl", click "Next"
1. Input Collection Name
1. Input starting URLs, click "Add" to add. Repeat for all domains.
1. Upper left Hamburger icon -> Manage Collections -> New collections
1. Select data source
1. If webcrawl, input url links to "Starting URLs" and click "Add" -> Finish

### 2.1 Setup NeuralSeek and integrate with Watson Discovery<a name="setup-NS-WD"></a>

1. Click on the "Launch NeuralSeek" button.
1. You'll land on the "Configure" tab. 
1. Enter the name of the company or organization that NeuralSeek will be generating answers for. Click "Next"
1. Input Watson Discovery KnowledgeBase details. Test connection by clicking "Test". Once tested, click "Next".
    :::info
    **Where to get credentials**
    - **Discovery Service Url**: within IBM Cloud -> resource list -> Watson Discovery Instance -> Manage -> Credentials
    - **Discovery API Key**: within IBM Cloud -> resource list -> Watson Discovery Instance -> Manage -> Credentials
    - **Discovery Project ID**: within Watson Discovery: Upper left Hamburger icon -> Integrate and deploy -> 
    API Information
    :::
1. For Virtual Agent Type, select "Watson Assistant Type". Click "Next". Click "Next". 
1. Navigate to "Seek" tab. Test NeuralSeek with e.g. "What products or services do you offer?"

### 2.1.1 For NeuralSeek + WatsonX.ai version <a name="setup-NS-WX"></a>
credentials for LLM
- LLM API Key: 
    - Go to [IBM Cloud API Keys](https://cloud.ibm.com/iam/apikeys)
    - Create -> Enter Name -> Copy
- LLM Endpoint
    - Go to [WatsonX Platform](https://dataplatform.cloud.ibm.com/wx/home?context=wx)
    - Prompt Lab -> View code (Right hand side, to the right of Model) -> Copy the API endpoint after `curl`
        ```
        https://us-south.ml.cloud.ibm.com/ml/v1-beta/generation/text?version=2023-05-29
        ```
- LLM Project ID: 
    - Go to [WatsonX Platform](https://dataplatform.cloud.ibm.com/wx/home?context=wx)
    - Projects -> Manage -> General -> Details -> Project ID
    - If project doesn't exist:
        - New Project -> Create an empty project -> Input Name -> Select storage service -> Create
        - Navigate within Project -> Manage -> Services & integrations
        - Click "Associate Service" -> Select services with type "Watson Machine Learning" -> click "Associate"
        - Navigate to General tab -> Details -> copy Project ID
    

### 2.2 Setup WatsonX and integrate with Watson Discovery <a name="setup-WX-WD"></a>
#### High-level Steps
1. Create a new Watson Assistant
1. [Create Watson Discovery custom extension](#2-create-watson-discovery-custom-extension)
1. [Create WatsonX custom extension](#3-create-watsonx-custom-extension)
1. [Configure WA action to integrate WatsonX Search using Watson Discovery](#4-configure-wa-action-to-integrate-watsonx-search-using-watson-discovery)
1. [Customize WatsonX actions](#5-customize-watsonx-actions)

#### 2. Create Watson Discovery custom extension<a name="2-create-watson-discovery-custom-extension"></a>
1. In your assistant, navigate to "Integrations" page. 
1. Click "Build custom extensions" -> click "Next" -> Input Extension name `Watson Discovery` -> click "Next"
1. download json file: [watson-discovery-query-openapi.json](https://github.com/watson-developer-cloud/assistant-toolkit/blob/master/integrations/extensions/starter-kits/watson-discovery/watson-discovery-query-openapi.json) and import file to WA
1. click "Next" -> click "Finish"
1. Lower Right corner of the Watson Disovery extension, click "Add" -> click "Add" -> click "Next"
1. In Authentication page, in the Authentication type dropdown, select "Basic auth"
    1. For Username enter `apikey`
    1. For password, create and copy a new API key from [API key](https://cloud.ibm.com/iam/apikeys)
    1. For discovery_url, within IBM Cloud -> resource list -> Watson Discovery Instance -> Manage -> Credentials -> URL
    1. Paste URL into discovery_url and remove `https://` from the beginning of the string
1. Click "Next", click "Finish", click "Close"

- Reference: [starter kit for accessing the IBM Watson Discovery v2 search API via a custom extension to IBM Watson Assistant](https://github.com/watson-developer-cloud/assistant-toolkit/tree/master/integrations/extensions/starter-kits/watson-discovery)

#### 3. Create WatsonX custom extension <a name="3-create-watsonx-custom-extension"></a>
1.  In your assistant, navigate to Integrations page, click "Build custom extension" -> click "Next" -> Input Extension name `watsonx` -> click "Next" .
1. download json file: [watsonx-openapi.json](https://github.com/watson-developer-cloud/assistant-toolkit/blob/master/integrations/extensions/starter-kits/language-model-watsonx/watsonx-openapi.json) and import file to WA
1. click "Next" -> click "Finish"
1. Lower Right corner of the watsonx extension, click "Add" -> click "Add" -> click "Next"
1. In Authentication page, in the Authentication type dropdown, select "OAuth 2.0"
    1. For Apikey, create and copy a new API key from [API key](https://cloud.ibm.com/iam/apikeys)
1. Click "Next", click "Finish", click "Close"

#### 4. Configure WA action to integrate WatsonX Search using Watson Discovery <a name="4-configure-wa-action-to-integrate-watsonx-search-using-watson-discovery"></a>
Upload Actions:
1. Download [discovery-watsonx-actions.json](https://github.com/watson-developer-cloud/assistant-toolkit/blob/master/integrations/extensions/starter-kits/language-model-conversational-search/discovery-watsonx-actions.json)
1. Navigate to "Actions" page, click "Global Settings" icon on the upper right corner
1. Navigate to Upload/Download tab, upload the downloaded JSON file [discovery-watsonx-actions.json](https://github.com/watson-developer-cloud/assistant-toolkit/blob/master/integrations/extensions/starter-kits/language-model-conversational-search/discovery-watsonx-actions.json) onto the tab or click to select a file from your local system, then click "Upload", and "Uplaod and replace".
3. within the Actions page, navigate to "Actions / Variables / Created by you". Set `discovery_project_id` and `watsonx_project_id` session variable
    :::info
    **Where to get credentials**
    - **discovery_project_id**: within Watson Discovery: Upper left Hamburger icon -> Integrate and deploy -> API Information
    - **watsonx_project_id**: 
        - Go to [WatsonX Platform](https://dataplatform.cloud.ibm.com/wx/home?context=wx)
        - Projects (click on project)-> Manage -> General -> Details -> Project ID
    :::
4. You're all set. Navigate to "Preview" to test the integration!

#### 5. Customize WatsonX actions
##### Add source link to response
1. To add source link to the response, We need to configure two actions.
    1.  Navigate to "Seach" action
        - step 5, click "set new value" within the set variable values section.
        - click "New session variable" from the dropdown, input Name `source_url`, select `Any` for Type, click "Apply".
        - in the text box after 'To', select "Expression" from dropdown, and input `${step_474_result_2}.body.results[0].metadata.source.url`. Click "Apply".
        - Save, and Close.
    2.  Navigate to "Generate Answer" action.
        - In step 10, in bottom of the 'Assistant says' text box, type
        `For more information, click $source_url`, Enter. 
        - Save, and Close.
##### Configure model response by customizing prompt
1. When a user prompt Watson Assistant with keywords such as "answer in bullet points", we would like the model to output in bullet points format. This requires additional configuration in the "Generate Answer" actions.
    1. Navigate to "Generate Answer" action
        - Add a new step after step 5
        - Select "Is taken ``with conditions``" from dropdown
        - Underneath "If All of this is true", click on most left box, and select ``"Expression"`` from dropdown
        - Enter `${query_text}.toLowerCase().contains("answer in bullet points")`
        - Click "Set variable" button to the right of Is taken with conditions
        - Click "Set new value", and type `model_input` and select variable
        - In the box after "To", select "Expression" from dropdown. 
        - Enter the follwing text
        ```
        ("<s>[INST] <<SYS>>\nYou are a helpful, respectful and honest assistant. Always answer as helpfully as possible, while being safe. Be brief in your answers. Your answers should not include any harmful, unethical, racist, sexist, toxic, dangerous, or illegal content. Please ensure that your responses are socially unbiased and positive in nature.\n\nIf a question does not make any sense, or is not factually coherent, explain why instead of answering something not correct. If you don’t know the answer to a question, please don’t share false information.\n<</SYS>>\n\nGenerate the next agent response in bullet points by answering the question. You are provided several documents with titles. If the answer comes from different documents please mention all possibilities and use the titles of documents to separate between topics or domains. If you cannot base your answer on the given documents, please state that you do not have an answer.\n\n").concat(${passages}).concat("\n\n").concat(${query_text}).concat("[/INST]")```
        - ![Customize WatsonX Prompt](https://github.com/ibm-client-engineering/solution-ithelpdesk-watsonx/tree/main/docs/3-Create/Customize_WatsonX_Prompt.png)
        

- Reference : [Language Model Conversational Search starter kit](https://github.com/watson-developer-cloud/assistant-toolkit/tree/master/integrations/extensions/starter-kits/language-model-conversational-search)

### 3. Setup Watson Assistant <a name="3-setup-watson-assistant"></a>
1. Within IBM Cloud, click "Launch Watson Assistant".
1. We could customize the Assistant Interface. In "Preview" tab -> Customize web chat. 
    - Style: can change the color theme of Watson Assistant
    - Launcher: edit greeting message
    - Home screen: cusomize conversation starters
1. We can customize the preview background to match any organization homepage by "Change background" -> "Enter Url" option -> "Continue" -> Enter website url.


### 4. Create NeuralSeek custom extension <a name="4-create-neuralseek-custom-extension"></a>
1. In Watson Assistant, on the "Integrations" tab of Watson Assistant, click "Build Custom Extension" then "Next".
2. Name the extension "NeuralSeek" and give a brief description. Click "Next".
3. Open another browser tab and navigate to NeuralSeek -> "Integrate" tab -> Download "Custom Extension OpenApi File".
4. Navigate to Watson Assitant browser tab. Upload NeuralSeek OpenApi file into Waston Assiatant. Click "Next" then "Finish".
5. On the new "NeuralSeek" extension tile that appears, click "Add", "Add", then "Next".
6. On the authentication screen, select "API key auth", and enter your api key as shown in NeuralSeek "Integrate" page.
7. Click "Next", "Finish", then "Close".



### 5. Create WA action to trigger NeuralSeek Search <a name="5-create-wa-action-to-trigger-neuralseek-search"></a>
1. On the "Actions" tab of Watson Assistant, click "Create Action". Choose "Quick Start with templates", then select  "NeuralSeek Starter Kit" -> "Select this starter kit" -> "Add templatess".
1. Open the "NeuralSeek Search" action.
1. In step 3, in the "And then" section, click "edit extension", 
    - in the Extension dropdown select "NeuralSeek"
    - in the Operation dropdown select "Seek an answer from NeuralSeek".
1. Set parameters. 
    - Set `question` To `query_text`. 
    - Set `options.language` to Enter text ->  `"en"` -> "Apply"
1. "Save" and "Close" action
#### No action matches Setup
1. Navigate to "All items" -> "Set by assistant" -> "No action matches".
1. Click on the "No action matches" action and delete the existing step 1 and step 2. 
1. "New Step". In the "And then" section, select "go to a subaction"  -> select "NeuralSeek Search" in the dropdown options -> "Apply".
1. "Save" and "Close"

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










### Service Now Configuration

#### Prerequisites
* Service Now Developer Instance
    * Follow steps [here](https://developer.servicenow.com/dev.do#!/learn/learning-plans/tokyo/new_to_servicenow/app_store_learnv2_buildmyfirstapp_tokyo_personal_developer_instances)

#### Get Developer Instance Credentials and OpenAPI spec

1. Login into the developer [site](https://developer.servicenow.com/dev.do)
2. Click on the drop down arrow near your profile in top right corner and select "Manage Instance Password"
    <img src="https://zenhub.ibm.com/images/64b6ea16cb0d621557d315d9/58004866-1119-4ce4-8b8e-f2b00b25ba1b" alt="drawing" width="500"/>
3. Make note of the "username" and "password" values (this will be used later)
4. Exit out of the window and select "Start Building"
5. Press "All" in the header and search "REST API Explorer"
6. Press "Export OpenAPI Specification (YAML/JSON)"

#### Edit Service Now OpenAPI spec
1. Open the downloaded API spec
2. Remove the forward slash at the end of the url string within the "servers" block to look like this:
     <img src="https://zenhub.ibm.com/images/64b6ea16cb0d621557d315d9/32a80241-3fa6-4659-9fb2-9faaf71f4f04" alt="drawing" width="300"/>
3. Add BasicAuth Component to the OpenAPI spec (make sure each block is comma delimited):
    ```sh
    "components":{
        "securitySchemes": {
            "basicAuth": {
                "type": "http",
                "scheme": "basic"
            }
        }
    }
    ```
4. Save file
#### Build Custom Extenstion
1. Within WatsonX Assistant, navigate to the sidebar and select "Integrations"
2. Select "Build Custom Extension"
3. For the "Basic Information" page fill out all appropriate fields and click "Next"
4. Upload the Service Now OpenAPI spec, click "Next" and then "Finish"
5. Within the extensions in Watson Assistant click "Add+" on the recently made Service Now custom extension
6. On the Authentication page fill out the username and password fields with the values saved from "Get Developer Instance Credentials and OpenAPI spec" step 3
7. Click "Next" and then "Finish"



### Reference
- [Integrate NeuralSeek with Watson Assistant and Watson Discovery](https://developer.ibm.com/tutorials/integrate-neuralseek-with-watson-assistant-and-watson-discovery/)
