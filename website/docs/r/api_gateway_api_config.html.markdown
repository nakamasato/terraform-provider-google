---
# ----------------------------------------------------------------------------
#
#     ***     AUTO GENERATED CODE    ***    Type: MMv1     ***
#
# ----------------------------------------------------------------------------
#
#     This file is automatically generated by Magic Modules and manual
#     changes will be clobbered when the file is regenerated.
#
#     Please read more about how to change this file in
#     .github/CONTRIBUTING.md.
#
# ----------------------------------------------------------------------------
subcategory: "API Gateway"
description: |-
  An API Configuration is an association of an API Controller Config and a Gateway Config
---

# google\_api\_gateway\_api\_config

An API Configuration is an association of an API Controller Config and a Gateway Config

~> **Warning:** This resource is in beta, and should be used with the terraform-provider-google-beta provider.
See [Provider Versions](https://terraform.io/docs/providers/google/guides/provider_versions.html) for more details on beta resources.

To get more information about ApiConfig, see:

* [API documentation](https://cloud.google.com/api-gateway/docs/reference/rest/v1beta/projects.locations.apis.configs)
* How-to Guides
    * [Official Documentation](https://cloud.google.com/api-gateway/docs/creating-api-config)

<div class = "oics-button" style="float: right; margin: 0 0 -15px">
  <a href="https://console.cloud.google.com/cloudshell/open?cloudshell_git_repo=https%3A%2F%2Fgithub.com%2Fterraform-google-modules%2Fdocs-examples.git&cloudshell_working_dir=apigateway_api_config_basic&cloudshell_image=gcr.io%2Fgraphite-cloud-shell-images%2Fterraform%3Alatest&open_in_editor=main.tf&cloudshell_print=.%2Fmotd&cloudshell_tutorial=.%2Ftutorial.md" target="_blank">
    <img alt="Open in Cloud Shell" src="//gstatic.com/cloudssh/images/open-btn.svg" style="max-height: 44px; margin: 32px auto; max-width: 100%;">
  </a>
</div>
## Example Usage - Apigateway Api Config Basic


```hcl
resource "google_api_gateway_api" "api_cfg" {
  provider = google-beta
  api_id = "api-cfg"
}

resource "google_api_gateway_api_config" "api_cfg" {
  provider = google-beta
  api = google_api_gateway_api.api_cfg.api_id
  api_config_id = "cfg"

  openapi_documents {
    document {
      path = "spec.yaml"
      contents = filebase64("test-fixtures/apigateway/openapi.yaml")
    }
  }
  lifecycle {
    create_before_destroy = true
  }
}
```
<div class = "oics-button" style="float: right; margin: 0 0 -15px">
  <a href="https://console.cloud.google.com/cloudshell/open?cloudshell_git_repo=https%3A%2F%2Fgithub.com%2Fterraform-google-modules%2Fdocs-examples.git&cloudshell_working_dir=apigateway_api_config_grpc&cloudshell_image=gcr.io%2Fgraphite-cloud-shell-images%2Fterraform%3Alatest&open_in_editor=main.tf&cloudshell_print=.%2Fmotd&cloudshell_tutorial=.%2Ftutorial.md" target="_blank">
    <img alt="Open in Cloud Shell" src="//gstatic.com/cloudssh/images/open-btn.svg" style="max-height: 44px; margin: 32px auto; max-width: 100%;">
  </a>
</div>
## Example Usage - Apigateway Api Config Grpc


```hcl
resource "google_api_gateway_api" "api_cfg" {
  provider = google-beta
  api_id = "api-cfg"
}

resource "google_api_gateway_api_config" "api_cfg" {
  provider = google-beta
  api = google_api_gateway_api.api_cfg.api_id
  api_config_id = "cfg"

  grpc_services {
    file_descriptor_set {
      path = "api_descriptor.pb"
      contents = filebase64("test-fixtures/apigateway/api_descriptor.pb")
    }
  }
  managed_service_configs {
    path = "api_config.yaml"
    contents = base64encode(<<-EOF
      type: google.api.Service
      config_version: 3
      name: ${google_api_gateway_api.api_cfg.managed_service}
      title: gRPC API example
      apis:
        - name: endpoints.examples.bookstore.Bookstore
      usage:
        rules:
        - selector: endpoints.examples.bookstore.Bookstore.ListShelves
          allow_unregistered_calls: true
      backend:
        rules:
          - selector: "*"
            address: grpcs://example.com
            disable_auth: true

    EOF
    )
  }
  lifecycle {
    create_before_destroy = true
  }
}
```

## Argument Reference

The following arguments are supported:


* `api` -
  (Required)
  The API to attach the config to.


- - -


* `display_name` -
  (Optional)
  A user-visible name for the API.

* `labels` -
  (Optional)
  Resource labels to represent user-provided metadata.

* `gateway_config` -
  (Optional)
  Immutable. Gateway specific configuration.
  If not specified, backend authentication will be set to use OIDC authentication using the default compute service account
  Structure is [documented below](#nested_gateway_config).

* `openapi_documents` -
  (Optional)
  OpenAPI specification documents. If specified, grpcServices and managedServiceConfigs must not be included.
  Structure is [documented below](#nested_openapi_documents).

* `grpc_services` -
  (Optional)
  gRPC service definition files. If specified, openapiDocuments must not be included.
  Structure is [documented below](#nested_grpc_services).

* `managed_service_configs` -
  (Optional)
  Optional. Service Configuration files. At least one must be included when using gRPC service definitions. See https://cloud.google.com/endpoints/docs/grpc/grpc-service-config#service_configuration_overview for the expected file contents.
  If multiple files are specified, the files are merged with the following rules: * All singular scalar fields are merged using "last one wins" semantics in the order of the files uploaded. * Repeated fields are concatenated. * Singular embedded messages are merged using these rules for nested fields.
  Structure is [documented below](#nested_managed_service_configs).

* `api_config_id` -
  (Optional)
  Identifier to assign to the API Config. Must be unique within scope of the parent resource(api).

* `project` - (Optional) The ID of the project in which the resource belongs.
    If it is not provided, the provider project is used.

* `api_config_id_prefix` - (Optional) Creates a unique name beginning with the
 specified prefix. If this and api_config_id are unspecified, a random value is chosen for the name.

<a name="nested_gateway_config"></a>The `gateway_config` block supports:

* `backend_config` -
  (Required)
  Backend settings that are applied to all backends of the Gateway.
  Structure is [documented below](#nested_backend_config).


<a name="nested_backend_config"></a>The `backend_config` block supports:

* `google_service_account` -
  (Required)
  Google Cloud IAM service account used to sign OIDC tokens for backends that have authentication configured
  (https://cloud.google.com/service-infrastructure/docs/service-management/reference/rest/v1/services.configs#backend).

<a name="nested_openapi_documents"></a>The `openapi_documents` block supports:

* `document` -
  (Required)
  The OpenAPI Specification document file.
  Structure is [documented below](#nested_document).


<a name="nested_document"></a>The `document` block supports:

* `path` -
  (Required)
  The file path (full or relative path). This is typically the path of the file when it is uploaded.

* `contents` -
  (Required)
  Base64 encoded content of the file.

<a name="nested_grpc_services"></a>The `grpc_services` block supports:

* `file_descriptor_set` -
  (Required)
  Input only. File descriptor set, generated by protoc.
  To generate, use protoc with imports and source info included. For an example test.proto file, the following command would put the value in a new file named out.pb.
  $ protoc --include_imports --include_source_info test.proto -o out.pb
  Structure is [documented below](#nested_file_descriptor_set).

* `source` -
  (Optional)
  Uncompiled proto files associated with the descriptor set, used for display purposes (server-side compilation is not supported). These should match the inputs to 'protoc' command used to generate fileDescriptorSet.
  Structure is [documented below](#nested_source).


<a name="nested_file_descriptor_set"></a>The `file_descriptor_set` block supports:

* `path` -
  (Required)
  The file path (full or relative path). This is typically the path of the file when it is uploaded.

* `contents` -
  (Required)
  Base64 encoded content of the file.

<a name="nested_source"></a>The `source` block supports:

* `path` -
  (Required)
  The file path (full or relative path). This is typically the path of the file when it is uploaded.

* `contents` -
  (Required)
  Base64 encoded content of the file.

<a name="nested_managed_service_configs"></a>The `managed_service_configs` block supports:

* `path` -
  (Required)
  The file path (full or relative path). This is typically the path of the file when it is uploaded.

* `contents` -
  (Required)
  Base64 encoded content of the file.

## Attributes Reference

In addition to the arguments listed above, the following computed attributes are exported:

* `id` - an identifier for the resource with format `projects/{{project}}/locations/global/apis/{{api}}/configs/{{api_config_id}}`

* `name` -
  The resource name of the API Config.

* `service_config_id` -
  The ID of the associated Service Config (https://cloud.google.com/service-infrastructure/docs/glossary#config).


## Timeouts

This resource provides the following
[Timeouts](https://developer.hashicorp.com/terraform/plugin/sdkv2/resources/retries-and-customizable-timeouts) configuration options:

- `create` - Default is 20 minutes.
- `update` - Default is 20 minutes.
- `delete` - Default is 20 minutes.

## Import


ApiConfig can be imported using any of these accepted formats:

```
$ terraform import google_api_gateway_api_config.default projects/{{project}}/locations/global/apis/{{api}}/configs/{{api_config_id}}
$ terraform import google_api_gateway_api_config.default {{project}}/{{api}}/{{api_config_id}}
$ terraform import google_api_gateway_api_config.default {{api}}/{{api_config_id}}
```

## User Project Overrides

This resource supports [User Project Overrides](https://registry.terraform.io/providers/hashicorp/google/latest/docs/guides/provider_reference#user_project_override).
