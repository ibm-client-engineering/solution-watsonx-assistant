---
id: solution-prepare-optional-lendyr
sidebar_position: 1
title: Lendyr
---
### Update serviceInstanceID and integrationID to connect to Watson Assistant
- In Watson Assistant, navigate to Integrations, and click "Open" for Web Chat, click "Confirm".
- Navigate to Embed tab and find the IDs for the following changes
    - Use serviceInstanceID in `demoConstants.ts`. Change the `serviceInstanceID` for const DEMO_ASSISTANT
    - Use integrationID in `enviornmentVariables.ts`. Change the `DEMO_ASSISTANT_INTEGRATION_ID` for case EnvironmentType.DEVELOPMENT

### Steps to Deployment
- In IBM Cloud, initiate a Kubernetes Services
    - VPC
- URL and SSL certificate
    - Create a link in a domain
    - Enable the SSL Certificate
- Build Docker File
- Configure Github Repo hosting
- Build CI/CD Pipeline (Travis, Github Actions,...)
    - set branches
