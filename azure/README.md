# Azure Workflows

## Deploy Azure Web App Workflow
This workflow simply downloads the specified artifact from GitHub, logs into Azure, and deploys the artifacts to the specified web application in Azure.

### Usage
```yaml
deployApplicationJob:
  name: Azure - Deploy Web App Workflow
  uses: webstorm-tech/workflows/.github/workflows/azure-deploy-web-app-workflow.yml@v5
  with:
    # The name of the GitHub artifact to download and deploy.
    # Required: yes
    artifactName: ''

    # The name of the Azure App Service the application should be deployed to.
    # Required: yes
    azureAppName: ''

    # The name of the environment the deploy the application should be deployed to.
    # Required: yes
    environment: ''

  # This is required as the workflow needs access to the `AZURE_CREDENTIALS` secret
  # Which must be in a format that the `azure/login` task can use as credentials
  secrets: inherit
```