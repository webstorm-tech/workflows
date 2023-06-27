# Git Workflows

## GitVersion Workflow
This workflow uses GitVersion to generate a semantic version for the state of the repository as a whole.
It outputs various values that can be consumed by other actions and/or workflows.

### Usage
```yaml
gitVersionJob:
  name: GitVersion Workflow
  uses: webstorm-tech/.github/workflows/gitversion-workflow.yml@v5
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