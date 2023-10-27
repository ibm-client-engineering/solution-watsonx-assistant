---
id: solution-prepare-optional-service-now
sidebar_position: 1
title: ServiceNow
---

## Integrate ServiceNow with Watson Assistant

### Prerequisites
* Service Now Developer Instance
    * Follow steps [here](https://developer.servicenow.com/dev.do#!/learn/learning-plans/tokyo/new_to_servicenow/app_store_learnv2_buildmyfirstapp_tokyo_personal_developer_instances)

### Get Developer Instance Credentials and OpenAPI spec

1. Login into the developer [site](https://developer.servicenow.com/dev.do)
2. Click on the drop down arrow near your profile in top right corner and select "Manage Instance Password"
    <img src="https://zenhub.ibm.com/images/64b6ea16cb0d621557d315d9/58004866-1119-4ce4-8b8e-f2b00b25ba1b" alt="drawing" width="500"/>
3. Make note of the "username" and "password" values (this will be used later)
4. Exit out of the window and select "Start Building"
5. Press "All" in the header and search "REST API Explorer"
6. Press "Export OpenAPI Specification (YAML/JSON)"

### Edit Service Now OpenAPI spec
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
### Build Custom Extenstion
1. Within WatsonX Assistant, navigate to the sidebar and select "Integrations"
2. Select "Build Custom Extension"
3. For the "Basic Information" page fill out all appropriate fields and click "Next"
4. Upload the Service Now OpenAPI spec, click "Next" and then "Finish"
5. Within the extensions in Watson Assistant click "Add+" on the recently made Service Now custom extension
6. On the Authentication page fill out the username and password fields with the values saved from "Get Developer Instance Credentials and OpenAPI spec" step 3
7. Click "Next" and then "Finish"

