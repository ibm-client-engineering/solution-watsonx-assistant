---
id: solution-prepare-optional-neuralseek
sidebar_position: 1
title: NeuralSeek
---
## Pre-Requisites
- Access to a NeuralSeek plan
- [Setup Watson Assistant and Watson Discovery](/docs/3-Create/Prepare/Core%20Components.md)
## High level Steps
- [Lite](#setup-NS-lite)
- [Lite-WatsonX](#setup-NS-watsonx)
- [Create NeuralSeek custom extension](#create-neuralseek-custom-extension)
- [Create WA action to trigger NeuralSeek Search](#create-wa-action-to-trigger-neuralseek-search)

### Setup NeuralSeek and integrate with Watson Discovery<a name="setup-NS-lite"></a>

If you initiated a **Lite** plan
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

### For NeuralSeek + WatsonX.ai version <a name="setup-NS-watsonx"></a>
If you initiated a **Lite-WatsonX** plan
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
    
### Create NeuralSeek custom extension <a name="create-neuralseek-custom-extension"></a>
1. In Watson Assistant, on the "Integrations" tab of Watson Assistant, click "Build Custom Extension" then "Next".
2. Name the extension "NeuralSeek" and give a brief description. Click "Next".
3. Open another browser tab and navigate to NeuralSeek -> "Integrate" tab -> Download "Custom Extension OpenApi File".
4. Navigate to Watson Assitant browser tab. Upload NeuralSeek OpenApi file into Waston Assiatant. Click "Next" then "Finish".
5. On the new "NeuralSeek" extension tile that appears, click "Add", "Add", then "Next".
6. On the authentication screen, select "API key auth", and enter your api key as shown in NeuralSeek "Integrate" page.
7. Click "Next", "Finish", then "Close".



### Create WA action to trigger NeuralSeek Search <a name="create-wa-action-to-trigger-neuralseek-search"></a>
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

### References
- [Integrate NeuralSeek with Watson Assistant and Watson Discovery](https://developer.ibm.com/tutorials/integrate-neuralseek-with-watson-assistant-and-watson-discovery/)