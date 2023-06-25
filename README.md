# Webstorm Technologies Workflows
This repository contains reusable GitHub workflows to help abstract common steps performed by various applications and/or workloads.
The workflows in this repository tend to perform specific tasks, such as building a .NET application or applying Terraform, and are meant to stand on their own.
The goal is to limit rediction that can occur through abstraction while still harnessing the power of reusablity that abstraction offers.

## .NET Workflows
All reusable workflows that target .NET applications can be found in the [`dotnet`](./dotnet/) folder of this repository.
The currently available reusable workflows are:
- [Build App Workflow](./dotnet/build-app-workflow.yml): A workflow that builds, tests, and publishes artifacts for a .NET application
- [Verify Code Style Workflow](./dotnet/verify-code-style-workflow.yml): A workflow for validating .NET code styles

## Azure Workflows
All reusable workflows that target Azure can be found in the [`azure`](./azure/) folder of this repository.
The currently available reusable workflows are:
- [Deploy Web App Workflow](./azure/deploy-web-app-workflow.yml): A workflow for deploying a GitHub artifact to an Azure Web App Service

## Docker Workflows
All reusable workflows that target Docker can be found in the [`docker`](./docker/) folder of this repository.
The currently available reusable workflows are:
- [Build Container Workflow](./docker/build-container-workflow.yml): A workflow for deploying a GitHub artifact to an Azure Web App Service

## Git Workflows
All reusable workflows that interact with the local Git repository (not GitHub) can be found in the [`git`](./git/) folder of this repository.
The currently available reusable workflows are:
- [GitVersion Workflow](./git/gitversion-workflow.yml): A workflow for running GitVersion and to obtain a semantic version for the repository

## GitHub Workflows
All reusable workflows that interact with GitHub (commonly the API) can be found in the [`github`](./github/) folder of this repository.
The currently available reusable workflows are:
- [Tag Repo Workflow](./git/gitversion-workflow.yml): A workflow for creating a Git Tag using the GitHub API

## Terraform Workflows
All reusable workflows that perform a Terraform action and/or command can be found in the [`terraform`](./terraform/) folder of this repository.
The currently available reusable workflows are:
- [Terraform Apply Workflow](./terraform/terraform-apply-workflow.yml): A workflow for executing `terraform apply`
- [Terraform Plan Workflow](./terraform/terraform-plan-workflow.yml): A workflow for generating a Terrform plan via `terraform plan`
- [Verify Terraform Formatting Workflow](./terraform/terraform-apply-workflow.yml): A workflow for verifing the formatting of the Terraform in the repository

# Contributing, Bug Fixes, and Enhancements
Instead of boiling the ocean, so to speak, these workflows have started off in the most basic and simpilest fashion.
Meaning that they dervied from rather basic use case scenarios and are slowly evolving as the use of them increases and advanced or complex situations are encountered.
This means that if you have suggestion for enhancements or have run into a bug, we welcome the feedback!
We ask that you create an issue so that we can review and investigate the bug fix and/or enhancement.
We will do our best to get each request in a timely fashion.

We also welcome anyone to openly contribute to this project as well!
If you have the time and are willing to help out, simply fork the repo and make the changes yourself.
You can then open up a PR to merge the changes back into this repository for all to enjoy your hard work and efforts.

At this time, we do not have a formal process for reviewing and merging code back into this repository, but rest assured that we are working on it.
Stay tuned for more details!

# License
All materinal in this repository/project are released under the [MIT License](LICENSE)
