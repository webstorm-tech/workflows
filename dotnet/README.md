# .NET Workflows

## Build Apps Workflow
This workflow builds, tests, and publishes artifacts for .NET applications.
You specify the version of .NET (which has to be a value supported by the `actions/setup-dotnet` action) and it will execute the install.
Once .NET is setup, it will perform `dotnet build` using `semVer` as the value for the `/p:Version` and `Release` for `--configuration` flags.
After a successful build and a value has been specified for both `unitTestProjectFile` and `unitTestProjectFile`, it will perform `dotnet test` on the specified unit test project.
Code coverage will be collected in `opencover` format and submitted to Codecov if specified too.
Next, the workflow will peform a `dotnet publish` on the specified web project.
It will then upload the published artifacts to GitHub using the specified name.
Lastly, the code coverage is published as an artifact to GitHub as well.

### Usage
```yaml
buildApplicationJob:
  name: CI - Build Workflow
  needs: gitVersionJob
  uses:  webstorm-tech/.github/workflows/dotnet-build-app-workflow.yml.yml@v5
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
    # If omitted, no tests will be run.
    # Default: ''
    # Required: no
    unitTestProjectFile: ''

    # The folder path that contains the project to run tests for. Ex. `./src/MyWebProject.Tests`
    # If omitted, no tests will be run.
    # Default: ''
    # Required: no
    unitTestProjectFolder: ''

    # A boolean value indicating if the code coverage results should be sent to Codecov. Defaults to `false`.
    # Default: false
    # Required: no
    sendToCodecov: false

    # The file name, including extension, of the project that will be published. Ex. `MyWebProject.csproj`
    # Required: yes
    publishProjectFile: ''

    # The folder path that contains the project that will be published. Ex. `./src/MyWebProject`
    publishProjectFolder: ''

  # This is required as the workflow needs access to the `CODECOV_TOKEN` secret
  # Which is needed to publish code coverate results to Codecov
  secrets:
    CODECOV_TOKEN: ${{ secrets.CODECOV_TOKEN }}
```

## Verify Code Style Workflow
This workflow will validating .NET code using the command `dotnet format --severity info --verify-no-changes`.
If there are not issues, the workflow will succeed, otherwise it will report a failure.
It is highly recommended that you use an `.editorconfig` to manage your configuration and settings for when `dotnet format` runs.
For more information on `dotnet format`, check out the [`dotnet/format`][dotnet-format] repository.
You can also read more about the `.editorconfig` file reading the Microsoft documentation [Code-style rule options][ms-code-style] for .NET.

### Usage
```yaml
verifyCodeStyleJob:
  name: PR - Verify Code Style Workflow
  uses: webstorm-tech/.github/workflows/dotnet-verify-code-style-workflow.yml@v5
  with:
    # The version of .NET to install onto the runner.
    # Default: 7.0.x
    dotnetVersion: ''

    # The path to the solution file to perform `dotnet build` on. Ex. `./src/MySolution.sln`
    solutionFile: ''
```

[dotnet-format]: https://github.com/dotnet/format "dotnet/format repo"
[ms-code-style]: https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/code-style-rule-options ".NET Code-style Rule Options"