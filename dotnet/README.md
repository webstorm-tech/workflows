# .NET Workflows

## Build Apps Workflow
This workflow builds, tests, and publishes artifacts for .NET applications.
You specify the version of .NET (which has to be a value supported by the `actions/setup-dotnet` action) and it will execute the install.
Once .NET is setup, it will attempt to add any NuGet sources specified in the `NUGET_REGISTRIES_JSON` secret.
It uses the [Add NuGet Regisries](https://github.com/marketplace/actions/add-nuget-registries) action to safely add private sources or sources that require authentication.
This is an optional step, so if no value is passed in, it will be skipped.
Next, it will perform `dotnet build` using `semVer` as the value for the `/p:Version` and `Release` for `--configuration` flags.
After a successful build and a value has been specified for both `unitTestProjectFile` and `unitTestProjectFile`, it will perform `dotnet test` on the specified unit test project.
Code coverage will be collected in `opencover` format and submitted to Codecov if specified too.
Next, the workflow will peform a `dotnet publish` on the specified web project.
It will then upload the published artifacts to GitHub using the specified name.
Lastly, the code coverage is published as an artifact to GitHub as well.

### Usage
```yaml
buildApplicationJob:
  name: .NET - Build App Workflow
  needs: gitVersionJob
  uses:  webstorm-tech/workflows/.github/workflows/dotnet-build-app-workflow.yml@v5
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

    # The runner to use when executing the workflow.
    # Default: ubuntu-latest
    # Required: no
    runner: ''

  secrets:
    # This is required as the workflow needs access to the token for Codecov
    # Which is needed to publish code coverate results to Codecov
    CODECOV_TOKEN: ${{ secrets.CODECOV_TOKEN }}

    # This is optional secret which contains all of the private NuGet soruces
    # That need to be added to the runner so the dotnet restore is successful
    NUGET_REGISTRIES_JSON: ${{ secrets.NUGET_REGISTRIES_JSON }}
```

## Verify Code Style Workflow
This workflow will validating .NET code using the command `dotnet format --severity info --verify-no-changes`.
If there are not issues, the workflow will succeed, otherwise it will report a failure.
It is highly recommended that you use an `.editorconfig` to manage your configuration and settings for when `dotnet format` runs.
For more information on `dotnet format`, check out the [`dotnet/format`][dotnet-format] repository.
You can also read more about the `.editorconfig` file reading the Microsoft documentation [Code-style rule options][ms-code-style] for .NET.

Part of the internals for `dotnet format`, it will attempt to restore dependencies during the exeuction.
If you have private NuGet sources, this will result in an error while running this workflow.
The `restore` flag and the `NUGET_REGISTRIES_JSON` secret are provided to help you determine the appropriate path.
If you simply want to turn of restore by passing the `--no-restore` flag into `dotnet format`, set the `restore` input to `false`.
If you want to add your private NuGet sources to the runner, pass a secret into the `NUGET_REGISTRIES_JSON` that conforms to the schema as described by the [Add NuGet Registries](add-nuget-registries) action.

### Usage
```yaml
verifyCodeStyleJob:
  name: PR - Verify Code Style Workflow
  uses: webstorm-tech/workflows/.github/workflows/dotnet-verify-code-style-workflow.yml@v5
  with:
    # The version of .NET to install onto the runner.
    # Default: 7.0.x
    dotnetVersion: ''

    # If false, the `--no-restore` flag will be passed to `dotnet format`.
    # Default: true
    restore: true

    # The runner to use when executing the workflow.
    # Default: ubuntu-latest
    # Required: no
    runner: ''

    # The path to the solution file to perform `dotnet build` on. Ex. `./src/MySolution.sln`
    solutionFile: ''

  secrets:
    # This is optional secret which contains all of the private NuGet soruces
    # That need to be added to the runner so the restore is successful
    NUGET_REGISTRIES_JSON: ${{ secrets.NUGET_REGISTRIES_JSON }}
```

[add-nuget-registries]: https://github.com/marketplace/actions/add-nuget-registries
[dotnet-format]: https://github.com/dotnet/format "dotnet/format repo"
[ms-code-style]: https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/code-style-rule-options ".NET Code-style Rule Options"