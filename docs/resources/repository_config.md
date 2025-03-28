---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "xray_repository_config Resource - terraform-provider-xray"
subcategory: ""
---

# xray_repository_config (Resource)

Provides an Xray repository config resource. See [Xray Indexing Resources](https://www.jfrog.com/confluence/display/JFROG/Indexing+Xray+Resources#IndexingXrayResources-SetaRetentionPeriod) and [REST API](https://www.jfrog.com/confluence/display/JFROG/Xray+REST+API#XrayRESTAPI-UpdateRepositoriesConfigurations) for more details.

## Example Usage

```terraform
resource "xray_repository_config" "xray-repo-config-pattern" {
  repo_name = "example-repo-local"

  config {
    vuln_contextual_analysis = true
    retention_in_days        = 90
  }

  paths_config {
    pattern {
      include             = "core/**"
      exclude             = "core/internal/**"
      index_new_artifacts = true
      retention_in_days   = 60
    }

    pattern {
      include             = "core/**"
      exclude             = "core/external/**"
      index_new_artifacts = true
      retention_in_days   = 45
    }

    all_other_artifacts {
      index_new_artifacts = true
      retention_in_days   = 60
    }
  }
}

resource "xray_repository_config" "xray-repo-config" {
  repo_name   = "example-repo-local"
  jas_enabled = true

  config {
    vuln_contextual_analysis = true
    retention_in_days        = 90
  }
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `repo_name` (String) The name of the repository to update configurations for.

### Optional

- `config` (Block Set) Single repository configuration. (see [below for nested schema](#nestedblock--config))
- `jas_enabled` (Boolean) Specified if JFrog Advanced Security is enabled or not. Default to 'false'
- `paths_config` (Block Set) Enables you to set a more granular retention period. It enables you to scan future artifacts within the specific path, and set a retention period for the historical data of artifacts after they are scanned (see [below for nested schema](#nestedblock--paths_config))

<a id="nestedblock--config"></a>
### Nested Schema for `config`

Optional:

- `exposures` (Block Set) Enables Xray to perform scans for multiple categories that cover security issues in your configurations and the usage of open source libraries in your code. Available only to CLOUD (SaaS)/SELF HOSTED for ENTERPRISE X and ENTERPRISE+ with Advanced DevSecOps. Must be set for Docker, Maven, NPM, PyPi, and Terraform Backend package type. (see [below for nested schema](#nestedblock--config--exposures))
- `retention_in_days` (Number) The artifact will be retained for the number of days you set here, after the artifact is scanned. This will apply to all artifacts in the repository. Can be omitted when `paths_config` is set.
- `vuln_contextual_analysis` (Boolean) Enables or disables vulnerability contextual analysis. Only for SaaS instances, will be available after Xray 3.59. Must be set for Docker, OCI, and Maven package types.

<a id="nestedblock--config--exposures"></a>
### Nested Schema for `config.exposures`

Optional:

- `scanners_category` (Block Set) Exposures' scanners categories configurations. (see [below for nested schema](#nestedblock--config--exposures--scanners_category))

<a id="nestedblock--config--exposures--scanners_category"></a>
### Nested Schema for `config.exposures.scanners_category`

Optional:

- `applications` (Boolean) Detect whether common OSS libraries and services are used securely by the application.
- `iac` (Boolean) Scans IaC files stored in Artifactory for early detection of cloud and infrastructure misconfigurations to prevent attacks and data leak. Only supported by Terraform Backend package type.
- `secrets` (Boolean) Detect any secret left exposed in any containers stored in Artifactory to stop any accidental leak of internal tokens or credentials.
- `services` (Boolean) Detect whether common OSS libraries and services are configured securely, so application can be easily hardened by default.




<a id="nestedblock--paths_config"></a>
### Nested Schema for `paths_config`

Optional:

- `all_other_artifacts` (Block Set) If you select by pattern, you must define a retention period for all other artifacts in the repository in the All Other Artifacts setting. (see [below for nested schema](#nestedblock--paths_config--all_other_artifacts))
- `pattern` (Block Set) Pattern, applied to the repositories. (see [below for nested schema](#nestedblock--paths_config--pattern))

<a id="nestedblock--paths_config--all_other_artifacts"></a>
### Nested Schema for `paths_config.all_other_artifacts`

Optional:

- `index_new_artifacts` (Boolean) If checked, Xray will scan newly added artifacts in the path. Note that existing artifacts will not be scanned. If the folder contains existing artifacts that have been scanned, and you do not want to index new artifacts in that folder, you can choose not to index that folder.
- `retention_in_days` (Number) The artifact will be retained for the number of days you set here, after the artifact is scanned. This will apply to all artifacts in the repository.


<a id="nestedblock--paths_config--pattern"></a>
### Nested Schema for `paths_config.pattern`

Required:

- `include` (String) Paths pattern to include in the set specific configuration.

Optional:

- `exclude` (String) Paths pattern to exclude from the set specific configuration.
- `index_new_artifacts` (Boolean) If checked, Xray will scan newly added artifacts in the path. Note that existing artifacts will not be scanned. If the folder contains existing artifacts that have been scanned, and you do not want to index new artifacts in that folder, you can choose not to index that folder.
- `retention_in_days` (Number) The artifact will be retained for the number of days you set here, after the artifact is scanned. This will apply to all artifacts in the repository.

## Import

To import repository configuration, you'll need to specific if your JFrog Platform has Advanced Security enabled as part of the resource ID along with repository name, separated by a colon (`:`).

For instance, using the following config during import:
```terraform
resource "xray_repository_config" "xray-repo-config" {
  repo_name   = "example-repo-local"
  jas_enabled = false

  config {
    retention_in_days = 90
  }
}
```

Then use `terraform import xray_repository_config.xray-repo-config example-repo-local:false` to import the repository configuration `xray-repo-config` with `jas_enabled` set to `false`.
