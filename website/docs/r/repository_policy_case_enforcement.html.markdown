---
layout: "azuredevops"
page_title: "AzureDevops: azuredevops_repository_policy_case_enforcement"
description: |- Manages a case enforcement repository policy within Azure DevOps project.
---

# azuredevops_repository_policy_case_enforcement

Manages a case enforcement repository policy within Azure DevOps project.   

~> If both project and project policy are enabled, the project policy has high priority.

## Example Usage

```hcl
resource "azuredevops_project" "p" {
  name               = "Sample Project"
  description        = "Managed by Terraform"
  visibility         = "private"
  version_control    = "Git"
  work_item_template = "Agile"
}

resource "azuredevops_git_repository" "r" {
  project_id = azuredevops_project.p.id
  name       = "Sample Repo"
  initialization {
    init_type = "Clean"
  }
}

resource "azuredevops_repository_policy_case_enforcement" "p" {
  project_id              = azuredevops_project.p.id
  enabled                 = true
  blocking                = true
  enforce_consistent_case = true
  repository_ids          = [azuredevops_git_repository.r.id]
}
```

# Set project level repository policy
```hcl
resource "azuredevops_repository_policy_case_enforcement" "p" {
  project_id              = azuredevops_project.p.id
  enabled                 = true
  blocking                = true
  enforce_consistent_case = true
}
```

## Argument Reference

The following arguments are supported:

- `project_id` - (Required) The ID of the project in which the policy will be created.
- `enabled` - (Optional) A flag indicating if the policy should be enabled. Defaults to `true`.
- `blocking` - (Optional) A flag indicating if the policy should be blocking. Defaults to `true`.
- `enforce_consistent_case` - (Required) Avoid case-sensitivity conflicts by blocking pushes that change name casing on files, folders, branches, and tags.
- `repository_ids` (Optional) Control whether the policy is enabled for the repository or the project. If `repository_ids` not configured, the policy will be set to the project.

## Attributes Reference

In addition to all arguments above, the following attributes are exported:

- `id` - The ID of the repository policy.

## Relevant Links

- [Azure DevOps Service REST API 5.1 - Policy Configurations](https://docs.microsoft.com/en-us/rest/api/azure/devops/policy/configurations/create?view=azure-devops-rest-5.1)

## Import

Azure DevOps repository policies can be imported using the projectID/policyID or projectName/policyID:

```sh
$ terraform import azuredevops_repository_policy_case_enforcement.p 00000000-0000-0000-0000-000000000000/0
```
