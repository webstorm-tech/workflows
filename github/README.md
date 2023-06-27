# GitHub Workflows

## Tag Repo Workflow
This workflow simply creates a Git Tag using the GitHub API using the value passed into it.
It's assumed that the value is some sort of semantic version as it prefixes a `v` to the value of the tag.
This workflow will need repository write permissions and access to the `GITHUB_TOKEN` secret to create the tag.

### Usage
```yaml
tagRepoJob:
  name: GitHub - Tag Repo Workflow
  uses: webstorm-tech/workflows/.github/workflows/github-tag-repo-workflow.yml@v5
  with:
    # The a semantic version value to tag the repository with.
    # Required: yes
    semVer: ''

  # Needed to access the GITHUB_TOKEN
  secrets: inherit
```