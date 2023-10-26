---
id: solution-prepare-core
sidebar_position: 1
title: Core Components
---

## Pre-Requisites
- Access to any Watson Assistant instance on a Lite, Plus Trial, Plus, or Enterprise plan
- Access to an instance of Watson Discovery
- Access to WatsonX platform
- Access to an instance of Watson Machine Learning

## **Core Compontents**
1. [Setup Watson Discovery](#setup-watson-discovery)
1. [Setup WatsonX and integrate with Watson Discovery](#setup-WX-WD)
1. [Setup Watson Assistant](#3-setup-watson-assistant)

## [Optional Components](/category/optional)
1. Integrate Watson Assistant with [NeuralSeek](/docs/3-Create/Prepare/Optional/NeuralSeek.md)
1. Integrate Watson Assistant with [Microsoft Teams](/docs/3-Create/Prepare/Optional/Microsoft%20Teams.md)
1. Integrate Watson Assistant with [ServiceNow](/docs/3-Create/Prepare/Optional/ServiceNow.md)

## Step-by-Step Instructions

### 1. Setup Watson Discovery<a name="setup-watson-discovery"></a>
1. New projects, input Project Name, select an option "None of the above — I’m working on a custom project", click "Next"
1. select "Web Crawl", click "Next"
1. Input Collection Name
1. Input starting URLs, click "Add" to add. Repeat for all domains.
1. Upper left Hamburger icon -> Manage Collections -> New collections
1. Select data source
1. If webcrawl, input url links to "Starting URLs" and click "Add" -> Finish

### 2. Setup WatsonX and integrate with Watson Discovery <a name="setup-WX-WD"></a>
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





