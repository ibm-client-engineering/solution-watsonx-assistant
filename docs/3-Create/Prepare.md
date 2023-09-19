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



### To integrate Microsoft Teams  
1. Create Microsoft Teams custom extension
1. Export MS Teams App and import to MS Teams Platform


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

### Reference
- [Integrate NeuralSeek with Watson Assistant and Watson Discovery](https://developer.ibm.com/tutorials/integrate-neuralseek-with-watson-assistant-and-watson-discovery/)
