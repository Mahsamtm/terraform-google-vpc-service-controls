# Access Perimeter Submodule

This module handles opiniated configuration and deployment of a [access_context_manager_service_perimeter](https://www.terraform.io/docs/providers/google/r/access_context_manager_service_perimeter.html) resource for regular service perimeter types.

## Usage
```hcl
provider "google" {
  version     = "~> 2.5.0"
}

module "org_policy" {
  source      = "terraform-google-modules/vpc-service-controls/google/modules/policy"
  parent_id   = var.parent_id
  policy_name = var.policy_name
}

module "regular_service_perimeter_1" {
  source         = "terraform-google-modules/vpc-service-controls/google/modules/regular_service_perimeter"
  policy         = module.org_policy.policy_id
  perimeter_name = "regular_perimeter_1"
  description    = "Some description"
  resources      = ["1111111111"]

  restricted_services = ["bigquery.googleapis.com", "storage.googleapis.com"]

  shared_resources = {
    all = ["1111111111"]
  }
}
```

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| access\_levels | A list of AccessLevel resource names that allow resources within the ServicePerimeter to be accessed from the internet. AccessLevels listed must be in the same policy as this ServicePerimeter. Referencing a nonexistent AccessLevel is a syntax error. If no AccessLevel names are listed, resources within the perimeter can only be accessed via GCP calls with request origins within the perimeter. Example: 'accessPolicies/MY\_POLICY/accessLevels/MY\_LEVEL'. For Service Perimeter Bridge, must be empty. | `list(string)` | `[]` | no |
| access\_levels\_dry\_run | (Dry-run) A list of AccessLevel resource names that allow resources within the ServicePerimeter to be accessed from the internet. AccessLevels listed must be in the same policy as this ServicePerimeter. Referencing a nonexistent AccessLevel is a syntax error. If no AccessLevel names are listed, resources within the perimeter can only be accessed via GCP calls with request origins within the perimeter. Example: 'accessPolicies/MY\_POLICY/accessLevels/MY\_LEVEL'. For Service Perimeter Bridge, must be empty. If set, a dry-run policy will be set. | `list(string)` | `[]` | no |
| description | Description of the regular perimeter | `string` | n/a | yes |
| perimeter\_name | Name of the perimeter. Should be one unified string. Must only be letters, numbers and underscores | `any` | n/a | yes |
| policy | Name of the parent policy | `string` | n/a | yes |
| resources | A list of GCP resources that are inside of the service perimeter. Currently only projects are allowed. | `list(string)` | `[]` | no |
| resources\_dry\_run | (Dry-run) A list of GCP resources that are inside of the service perimeter. Currently only projects are allowed. If set, a dry-run policy will be set. | `list(string)` | `[]` | no |
| restricted\_services | GCP services that are subject to the Service Perimeter restrictions. Must contain a list of services. For example, if storage.googleapis.com is specified, access to the storage buckets inside the perimeter must meet the perimeter's access restrictions. | `list(string)` | `[]` | no |
| restricted\_services\_dry\_run | (Dry-run) GCP services that are subject to the Service Perimeter restrictions. Must contain a list of services. For example, if storage.googleapis.com is specified, access to the storage buckets inside the perimeter must meet the perimeter's access restrictions.  If set, a dry-run policy will be set. | `list(string)` | `[]` | no |
| shared\_resources | A map of lists of resources to share in a Bridge perimeter module. Each list should contain all or a subset of the perimeters resources | `object({ all = list(string) })` | <pre>{<br>  "all": []<br>}</pre> | no |
| ingress\_policies\_info | A map of objects, it includes ingress_from and ingress_to objects |
map(object({
    ingress_from = object({
      identity_type = string
      identities    = list(string)
      sources = object({
        access_level = string
        resource     = string
      })
    })
    ingress_to = object({
      resources = list(string)
      operations = list(object({
        service_name = string
        method_selectors = object({
        method = string })
        })
      )
    })
    })
  ) | '{}' | no |
| egress\_policies\_info | A map of objects,it includes egress_from and egress_to objects |
 map(object({
    egress_from = object({
      identity_type = string
      identities    = list(string)
    })
    egress_to = object({
      resources = list(string)
      operations = list(object({
        service_name = string
        method_selectors = object({
        method = string })
        })
      )
    })
    })
  ) | '{}' | no |
## Outputs

| Name | Description |
|------|-------------|
| perimeter\_name | The perimeter's name. |
| resources | A list of GCP resources that are inside of the service perimeter. Currently only projects are allowed. |
| shared\_resources | A map of lists of resources to share in a Bridge perimeter module. Each list should contain all or a subset of the perimeters resources |

<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
