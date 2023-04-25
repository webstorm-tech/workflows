# Webstorm Technologies Workflows
This repository contains reusable GitHub workflows to help abstract common actions performed by various applications and/or workloads.

## Build .NET Web Apps Workflow
This workflow builds, tests, and publishes artifacts for .NET applications.
You specify the version of .NET (which has to be a value supported by the `actions/setup-dotnet` action) and it will execute the install.
Once .NET is setup, it will perform `dotnet build` using `semVer` as the value for the `/p:Version` and `Release` for `--configuration` flags.
After a successful build, it will perform `dotnet test` on the specified unit test project.
Code coverage will be collected in `opencover` format and submitted to Codecov.
Next, the workflow will peform a `dotnet publish` on the specified web project.
It will then upload the published artifacts to GitHub using the specified name.
Lastly, the code coverage is published as an artifact to GitHub as well.

### Usage
```yaml
buildApplicationJob:
  name: CI - Build Workflow
  needs: gitVersionJob
  uses:  webstorm-tech/workflows/.github/workflows/dotnet-build-web-apps-workflow.yml.yml@v1
  with:
    # The name to be used when publishing the build artifacts
    # Required: yes
    artifactName: ''

    # The version of .NET to install onto the runner.
    # Default: 7.0.x
    dotnetVersion: ''

    # The version of the application being built. It must be in semantic versioning format
    # Required: yes
    semVer: ''

    # The path to the solution file to perform `dotnet build` on. Ex. `./src/MySolution.sln`
    # Required: yes
    solutionFile: ''

    # The test category or trait to use when running `dotnet test`.
    # Default: UnitTest
    testCategory: ''

    # The file name, including extension, of the project to execute tests for. Ex. `MyWebProject.Tests.proj`
    # Required: yes
    unitTestProjectFile: ''

    # The folder path that contains the project to run tests for. Ex. `./src/MyWebProject.Tests`
    # Required: yes
    unitTestProjectFolder: ''

    # The file name, including extension, of the web project that will be published. Ex. `MyWebProject.proj`
    # Required: yes
    webProjectFile: ''

    # The folder path that contains the web project that will be published. Ex. `./src/MyWebProject`
    webProjectFolder: ''

  # This is required as the workflow needs access to the `CODECOV_TOKEN` secret
  # Which is needed to publish code coverate results to Codecov
  secrets: inherit
```

## Deploy Azure Web App Workflow
This workflow simply downloads the specified artifact from GitHub, logs into Azure, and deploys the artifacts to the specified web application.

### Usage
```yaml
deployApplicationJob:
  name: CD - Deploy Workflow
  uses: webstorm-tech/workflows/.github/workflows/deploy-azure-web-app-workflow.yml@v1
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

## GitVersion Workflow
This workflow uses GitVersion to generate a semantic version for the state of the repository as a whole.
It outputs various values that can be consumed by other actions and/or workflows.

### Usage
```yaml
gitVersionJob:
  name: CD - GitVersion Workflow
  uses: webstorm-tech/workflows/.github/workflows/gitversion-reusable-workflow.yml@v1
```

### Outputs
| Variable Name          | Description                                                 | Example                   |
|:----------------------:|-------------------------------------------------------------|:-------------------------:|
|`major`                 |The major version. Should be incremented on breaking changes.|`3`                        |
|`minor`                 |The minor version. Should be incremented on new features.    |`10`                       |
|`patch`                 |The patch version. Should be incremented on bug fixes.       |`11`                       |
|`majorMinor`            |The major and minor version together.                        |`3.10`                     |
|`releaseLabel`          |The major, minor, patch, and the pre-release label (if any)  |`3.10.11-beta`<br>`3.10.11`|
|`majorMinorReleaseLabel`|The major, minor, and the pre-release label (if any)         |`3.10-beta`<br>`3.10`      |
|`majorReleaseLabel`     |The major and the pre-release label (if any)                 |`3-beta`<br>`3`            |
|`semVer`                |The major, minor, and patch in semantic version format       |`3.10.11`                  |
|`shortSha`              |The first 7 characters of the Git commit SHA                 |`28c8531`                  |

## Tag Repo Workflow
This workflow simply creates a Git Tag using the GitHub API using the value passed into it.
It's assumed that the value is some sort of semantic version as it prefixes a `v` to the value of the tag.
This workflow will need repository write permissions and access to the `GITHUB_TOKEN` secret to create the tag.

### Usage
```yaml
tagRepoJob:
  name: CD - Tag Repo Workflow
  uses: webstorm-tech/workflows/.github/workflows/tag-repo-workflow.yml@v1
  with:
    # The a semantic version value to tag the repository with.
    # Required: yes
    semVer: ''

  # Needed to access the GITHUB_TOKEN
  secrets: inherit
```

## Terraform Apply Workflow
This is a simple workflow that uses Terraform Cloud to run `terraform apply`.

### Usage
```yaml
terraformApplyJob:
  name: CD - Terraform Apply Workflow
  uses: webstorm-tech/workflows/.github/workflows/terraform-apply-workflow.yml@v1
  with:
    # The path that contains the Terraform to run apply for.
    # Required: yes
    workingDirectory: ''

  # Needed to access the TF_API_TOKEN secret
  secrets: inherit
```

## Terraform Plan Workflow
This workflow uses Terraform Cloud to generate a Terraform plan.
After it generates the plan, the details are then updated as part of the step summary in GitHub Actions.
If the plan is being ran as part of a pull request, it will add a comment to the PR with the details of the plan.

### Usage
```yaml
terraformPlanJob:
  name: PR - Terraform Plan Workflow
  uses: webstorm-tech/workflows/.github/workflows/terraform-plan-workflow.yml@v1
  with:
    # The path that contains the Terraform to create a plan for.
    # Required: yes
    workingDirectory: ''

  # Needed to access the TF_API_TOKEN secret
  secrets: inherit
```

## Verify Code Style Workflow
This workflow will validating both .NET source code and Terraform code.
.NET Code is validated using `dotnet format` and Terraform is validated using `terraform fmt -check`.
You can set flags to turn either checks off depending on individual needs.

### Usage
```yaml
verifyCodeStyleJob:
  name: PR - Verify Code Style Workflow
  uses: webstorm-tech/workflows/.github/workflows/verify-code-style-workflow.yml@v1
  with:
    # The version of .NET to install onto the runner.
    # Default: 7.0.x
    dotnetVersion: ''

    # The path to the solution file to perform `dotnet build` on. Ex. `./src/MySolution.sln`
    # Required: yes
    solutionFile: ''

    # The path that contains the Terraform to validate
    # Required: yes
    terraformWorkingDirectory: ''

    # A flag to indicate whether ot not to verify source code formatting.
    # Default: true
    verifyCode: true

    # A flag to indicate whether ot not to verify Terraform formatting.
    # Default: true
    verifyTerraform: true
```

# License

All materinal in this repository/project are released under the [MIT License](LICENSE)