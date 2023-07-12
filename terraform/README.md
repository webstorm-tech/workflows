# Terraform Workflows

## Terraform Apply Workflow
This is a simple workflow that uses Terraform Cloud to run `terraform apply`.

### Usage
```yaml
terraformApplyJob:
  name: Terraform - Apply Workflow
  uses: webstorm-tech/workflows/.github/workflows/terraform-apply-workflow.yml@v5
  with:
    # The path that contains the Terraform to run apply for.
    # Required: yes
    workingDirectory: ''

  # The Terraform API token to connect to Terraform Cloud with
  secrets:
    TF_API_TOKEN: ''
```

## Terraform Formatting Check Workflow
This workflow will check to see if the Terraform meets the style standards using the `terraform fmt -check` command.
If there are not issues, the workflow will succeed, otherwise it will report a failure.

### Usage
```yaml
verifyCodeStyleJob:
  name: Terraform - Check Formatting
  uses: webstorm-tech/workflows/.github/workflows/terraform-formatting-check-workflow.yml@v5
  with:
    # The path that contains the Terraform to check.
    # Required: yes
    workingDirectory: ''
```

## Terraform Plan Workflow
This workflow uses Terraform Cloud to generate a Terraform plan.
After it generates the plan, the details are then updated as part of the step summary in GitHub Actions.
If the plan is being ran as part of a pull request, it will add a comment to the PR with the details of the plan.

### Usage
```yaml
terraformPlanJob:
  name: Terraform - Plan Workflow
  uses: webstorm-tech/workflows/.github/workflows/terraform-plan-workflow.yml@v5
  with:
    # The path that contains the Terraform to create a plan for.
    # Required: yes
    workingDirectory: ''

  # The Terraform API token to connect to Terraform Cloud with
  secrets:
    TF_API_TOKEN: ''
```

## Terraform Validate Workflow
This workflow validates the Terraform files in the specified working directory.
It peforms a `terraform init -backend=false` to initialize the directory but without accessing any backend or state resources.
This is intended to be used as part of CI workflow or any other workflow where you need to validate the Terraform first.

### Usage
```yaml
terraformPlanJob:
  name: Terraform - Validate Workflow
  uses: webstorm-tech/workflows/.github/workflows/terraform-validate-workflow.yml@v5
  with:
    # The path that contains the Terraform to create a plan for.
    # Required: yes
    workingDirectory: ''
```
