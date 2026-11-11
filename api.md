---

copyright:
  years: 2026
lastupdated: "2026-11-11"

subcollection: EventStreams-gen2

keywords: api, resource controller, rest api, event streams api

---

{{site.data.keyword.attribute-definition-list}}

# Using the {{site.data.keyword.messagehub}} API on {{site.data.keyword.cloud_notm}}
{: #api}

[Gen 2]{: tag-purple}

Learn about the {{site.data.keyword.messagehub}} API on {{site.data.keyword.cloud}}, including authentication, error handling, and available methods for managing your data services.
{: shortdesc}

The {{site.data.keyword.messagehub}} API uses the [Resource Controller API](/docs/apis/resource-controller/resource-controller#intro) for lifecycle management operations. Use this reference to manage your service instances programmatically.
{: note}

## Authentication
{: #api-authentication}

Access to the API requires token authentication by using the `Authorization: Bearer <token>` header. The token must be IAM-issued. You can provide an IAM API key directly as the token or [generate an IAM bearer token](/docs/iam?topic=iam-iamtoken_from_apikey) from your API key.

To call API methods, you must be assigned a role with the required IAM actions. For more information about IAM actions and how they map to roles, see [Managing access to {{site.data.keyword.messagehub}}](/docs/EventStreams-gen2?topic=EventStreams-gen2-security).

## Error handling
{: #api-error-handling}

The API uses standard HTTP response codes to indicate request outcomes:

- `200 OK` or `201 Created`: The request succeeded.
- `4xx`: A client error occurred, such as invalid parameters or insufficient permissions.
- `500`: An internal system error occurred.

Responses can include a JSON response body with detailed error messages.

## Event tracking
{: #api-event-tracking}

You can monitor API activity in your account by using the [{{site.data.keyword.cloudaccesstraillong}}](/docs/EventStreams-gen2?topic=EventStreams-gen2-at_events) service. When you invoke an API method, the service generates an event that you can track and audit in Activity Tracker.

## Deployment IDs and CRNs
{: #api-deployment}

Deployment IDs are Cloud Resource Names (CRNs). When you use a CRN in API request paths, URL-encode the CRN value because it contains forward slash (`/`) characters (`%2F`).

Example CRN:

```text
crn:v1:bluemix:public:messagehub:us-south:a/274074dce64e9c423ffc238516c755e1:29caf0e7-120f-4da8-9551-3abf57ebcfc7::
```
{: codeblock}

URL-encoded CRN:

```text
crn%3Av1%3Abluemix%3Apublic%3Amessagehub%3Aus-south%3Aa%2F274074dce64e9c423ffc238516c755e1%3A29caf0e7-120f-4da8-9551-3abf57ebcfc7::
```
{: codeblock}

## Pagination
{: #api-pagination}

No endpoint currently returns paginated data.

## Rate limiting
{: #api-rate-limiting}

No endpoint currently implements rate limiting.

## Methods
{: #api-methods}

### List all deployed products
{: #api-methods-list-deployed}

Returns a list of all deployed cloud services.

```sh
GET /v2/resource_instances
```
{: pre}

Example request:

```sh
curl -X GET https://resource-controller.cloud.ibm.com/v2/resource_instances -H "Authorization: Bearer <IAM token>"
```
{: pre}

Example response:

```sh
Name:                test-eventstreams-01
Location:            us-south
Family:              resource_controller
Resource Type:       resource-instance
Resource Group ID:   eb922ba0717f47589ca26d202c5cc915
CRN:                 crn:v1:bluemix:public:messagehub:us-south:a/40ddc34a953a8c02f10987b59085b60e:09a8b8a3-29cf-4c1b-85ad-35e25611a414::
Tags:
Service Tags:
Access Tags:

Name:                test-eventstreams-02
Location:            us-south
Family:              resource_controller
Resource Type:       resource-instance
Resource Group ID:   eb922ba0717f47589ca26d202c5cc915
CRN:                 crn:v1:bluemix:public:messagehub:us-south:a/40ddc34a953a8c02f10987b59085b60e:3a81147c-ae5a-48c4-a0e5-cbf3e88cf215::
Tags:
Service Tags:
Access Tags:
```
{: pre}

### Get deployment information
{: #api-methods-get-deployment}

Retrieve the complete data associated with a deployment, including the ID, name, plan, connection details, and configuration.

```sh
GET /v2/resource_instances/{id}
```
{: pre}

Example request:

```sh
curl -X GET https://resource-controller.cloud.ibm.com/v2/resource_instances/8d7af921-b136-4078-9666-081bd8470d94 -H "Authorization: Bearer <IAM token>"
```
{: pre}

Example response:

```sh
[
    {
        "guid": "5aa314a5-76d9-4440-8e84-faa343d40127",
        "id": "crn:v1:staging:public:messagehub:us-east:a/cf8d4161fa0243b9a2a5494cd7ff66b7:5aa314a5-76d9-4440-8e84-faa343d40127::",
        "url": "/v2/resource_instances/5aa314a5-76d9-4440-8e84-faa343d40127",
        "created_at": "2025-08-28T16:35:07.650372748Z",
        "updated_at": "2025-08-28T16:41:53.419092609Z",
        "deleted_at": null,
        "name": "eventstreams-test",
        "region_id": "us-east",
        "account_id": "cf8d4161fa0243b9a2a5494cd7ff66b7",
        "resource_plan_id": "messagehub-standard",
        "resource_group_id": "97bc590fddc845ee903645bce8771bab",
        "crn": "crn:v1:staging:public:messagehub-cdp-dev:us-east:a/cf8d4161fa0243b9a2a5494cd7ff66b7:5aa314a5-76d9-4440-8e84-faa343d40127::",
        "extensions": {
          "dataservices": {
            "resources": {
              "kafka": {
                "storage_gb": 2000,
                "throughput_mb_s": 100,
                "version": "4.1"
              }
            },
            "connection": {
                "bootstrap_servers": "kafka-0.03ab707f-514a-49ea-959f-ff65d84a8818.private.uhp.ca-mon.eventstreams.dataservices.appdomain.cloud:9093,kafka-1.03ab707f-514a-49ea-959f-ff65d84a8818.private.uhp.ca-mon.eventstreams.dataservices.appdomain.cloud:9093,kafka-2.03ab707f-514a-49ea-959f-ff65d84a8818.private.uhp.ca-mon.eventstreams.dataservices.appdomain.cloud:9093",
                "rest_url": "https://api.ca-mon.eventstreams.dataservices.cloud.ibm.com/v1/instance/03ab707f-514a-49ea-959f-ff65d84a8818"
            },
            "encryption": {
              "disk": "crn:v1..."
            },
            "endpoints": "private",
            "version": "18",
            "$schema": {
              "version": "1.0.0"
            },
            "virtual_private_endpoints": {
                    "dns_domain": "private.axd.us-east.eventstreams.dataservices.dev.appdomain.cloud",
                    "dns_hosts": [
                        "5aa314a5-76d9-4440-8e84-faa343d40127",
                        "*.5aa314a5-76d9-4440-8e84-faa343d40127"
                    ],
                    "endpoints": [
                        {
                            "ip_address": "10.16.117.27",
                            "zone": "us-east-1"
                        },
                        {
                            "ip_address": "10.16.119.1",
                            "zone": "us-east-2"
                        },
                        {
                            "ip_address": "10.16.123.0",
                            "zone": "us-east-3"
                        }
                    ],
                    "origin_type": "vpc",
                    "ports": [
                        {
                            "port_max": 5432,
                            "port_min": 5432
                        }
                    ]
                }
            },
          }
        }
        "create_time": 0,
        "created_by": "IBMid-5500013U80",
        "state": "active",
        "type": "service_instance",
        "resource_id": "0de4305b-b472-449d-b369-07d14bba2d1d",
        "dashboard_url": null,
        "allow_cleanup": false,
        "locked": false,
        "onetime_credentials": true,
        "last_operation": {
            "type": "create",
            "state": "succeeded",
            "description": "Provision completed successfully",
            "updated_at": null,
            "cancelable": true
        },
        "account_url": "",
        "resource_plan_url": "",
        "resource_bindings_url": "/v2/resource_instances/5aa314a5-76d9-4440-8e84-faa343d40127/resource_bindings",
        "resource_aliases_url": "/v2/resource_instances/5aa314a5-76d9-4440-8e84-faa343d40127/resource_aliases",
        "siblings_url": "",
        "target_crn": "crn:v1:staging:public:globalcatalog::::deployment:messagehub-standard-deployment-us-east-72d307a5"
    }
]
```
{: pre}

### Provision and configure a service instance
{: #api-manage-config}

Create and configure a new {{site.data.keyword.messagehub}} service instance.

```sh
POST /v2/resource_instances
```
{: pre}

Example request:

```sh
curl -X POST https://resource-controller.cloud.ibm.com/v2/resource_instances \
  -H "Authorization: Bearer <IAM token>" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "my-instance",
    "target": "ca-mon",
    "resource_group": "5c49eabc-f5e8-5881-a37e-2d100a33b3df",
    "resource_plan_id": "messagehub-standard",
    "dataservices": {
      "kafka": {
        "storage_gb": 2000,
        "throughput_mb_s": 100
      },
      "encryption": {
        "disk": "crn:v1..."
      },
      "version": "18",
      "$schema": {
        "version": "1.0.0"
      }
    }
  }'
```
{: pre}

## Input parameters
{: #api-input-parameters}

The request body for configuring a service instance supports the following properties:

`dataservices` (object)
:   Service-specific configuration object.

`dataservices.kafka` (object)
:   Kafka configuration parameters.

`dataservices.kafka.storage_gb` (integer)
:   Configured storage capacity in gigabytes.

`dataservices.kafka.throughput_mb_s` (integer)
:   Configured throughput allocation in megabytes per second.

`dataservices.encryption.disk` (string)
:   CRN of the customer-managed key used for disk encryption.

`dataservices.version` (string)
:   Data services platform version.

## Response properties
{: #api-response-properties}

The API returns JSON objects with the following key fields:

`last_operation` (object)
:   Indicates the status of the most recent lifecycle task, such as provisioning or scaling.

`connection` (object)
:   Contains connection parameters, including bootstrap servers and REST URLs.

`dataservices` (object)
:   Contains service-specific information, such as Kafka capacity and encryption settings.

`virtual_private_endpoints` (object)
:   Lists private DNS domains, host mappings, and allocated IP endpoints across availability zones.
