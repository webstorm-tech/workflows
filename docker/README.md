# Docker Workflows

## Build Container Workflow
This workflow performs a `docker build` on the specified Dockerfile and pushes it to the specified registry.
It will tag the image wih the sematic version (ex. `3.10.1`), the major and minor (ex. `3.10`), and the major (ex. `3`).
If the `pushContainerImage` flag is set to `true`, it will push these tags to the container registry.
In addition, the build will tag the image with `latest` and if the `pushLatestTag` flag is set to `true` it will push it as well.

### Usage
```yaml
buildDockerContainerJob:
  name: Build Docker Container
  uses: webstorm-tech/.github/workflows/docker-build-container-workflow.yml@v5
  with:
    # The path to the Docker file
    # Required: yes
    dockerfile: ''

    # The the path within the repository to push the container image too.
    # Required: yes
    dockerNamespace: ''

    # The Docker registry to push the container image to. Ex. `ghcr.io`
    # Required: yes
    dockerRegistry: ''

    # The repository with in the registry to push the container image too.
    # Required: yes
    dockerRepository: ''

    # The context or working directory to be used with `docker build`.
    # Required: yes
    dockerWorkingDirectory: ''
    
    # The major part of the semantic version. Ex. `3`
    # Required: yes
    majorVersion: ''

    # The major and minor parts of the semantic version. Ex. `3.1`
    # Required: yes
    majorMinorVersion: ''

    # A flag to indicate if the container image should be pushed or not.
    # Defaults: true
    pushContainerImage: true

    # A flag to indicate if the `latest` should be pushed or not.
    # Defaults: true
    pushLatestTag: true

semVer:
      description: The full semantic version of. Ex. `3.1.2`
  # This is required as the workflow needs access to the `REGISTRY_TOKEN` secret
  # Which is the credential needed to log into the container registroy
  secrets: inherit
```