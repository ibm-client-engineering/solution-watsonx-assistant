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
- In Kubernetes Services, Create a Namespace and Ingress
    - Create a Namespace, e.g. `watson-assistant-<clinent-name>` 
    - Ingress is a outside connection that takes you inside to a pod or a service
    - Ingress is like a load balancer and where we map DNS entry to URL, and will forward any matching
- URL and SSL certificate
    - Create a link in a domain that you own
    - Attach the SSL Certificate 
        - note: the certificate should be good for 90 days
    - Export the certificate as a secret into the Kubernetes Cluster
- Build Docker File
- Configure Github Repo hosting
- Build CI/CD Pipeline (Travis, Github Actions,...)
    - Travis File
    - set branches

### IBM Cloud CLI
- Refer to [Getting started with the IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cli-getting-started)
- 