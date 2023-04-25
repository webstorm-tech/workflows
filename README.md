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

## GitVersion Workflow

## Tag Repo Workflow

## Terraform Apply Workflow

## Terraform Plan Workflow

## Verify Code Style Workflow

# License

All materinal in this repository/project are released under the [MIT License](LICENSE)