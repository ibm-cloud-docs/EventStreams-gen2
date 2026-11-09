---

copyright:
  years: 2026
lastupdated: "2026-11-09"

keywords: event streams, cli, resource controller, service instance, provisioning, service keys

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with {{site.data.keyword.messagehub}} CLI on {{site.data.keyword.cloud_notm}}
{: #cli-reference}

[Gen 2]{: tag-purple}

Learn how to use the {{site.data.keyword.cloud_notm}} Resource Controller CLI to work with {{site.data.keyword.messagehub}} Gen 2, including creating instances and managing service credentials.
{: shortdesc}

To interact with {{site.data.keyword.messagehub}} on Gen 2 by using the CLI, use the {{site.data.keyword.cloud_notm}} Resource Controller CLI. For more information, see [General IBM Cloud CLI (ibmcloud) commands](/docs/cli?topic=cli-ibmcloud_cli).

The {{site.data.keyword.messagehub}} plug-in supports only Gen 1 instances. For Gen 2 instances, use the Resource Controller CLI.
{: note}

## Creating an instance
{: #cli-create}

To create an instance, run the following command.

```sh
ibmcloud resource service-instance-create <INSTANCE_NAME> <SERVICE_NAME> <SERVICE_PLAN_NAME> <LOCATION> -g <RESOURCE_GROUP>
```
{: pre}

See the following example command for {{site.data.keyword.messagehub}}.

```sh
ibmcloud resource service-instance-create <INSTANCE_NAME> messagehub enterprise-gen2 ca-mon -g Default -p '{
   "dataservices":{
      "kafka": {
        "throughput_mb_s": 100,
        "storage_gb": 2000
      }
   }
}'
```
{: pre}

You can specify more parameters in the configuration block.

```json
{
  "dataservices": {
    "kafka": {
      "storage_gb": 2000,
      "throughput_mb_s": 100
    },
    "encryption": {
      "disk": "crn:v1:bluemix:public:kms:us-south:a/..."
    },
    "options": {
      "iam_token_only": false,
      "metrics": ["topic", "partition", "consumers"]
    }
  }
}
```
{: codeblock}

## Retrieving instance details
{: #cli-getting-info}

To retrieve instance details in JSON format, run the following command:

```sh
ibmcloud resource service-instance <INSTANCE_NAME> -o JSON
```
{: pre}

## Updating an instance
{: #cli-update-instance}

To update your instance, such as scaling throughput or storage capacity, run the following command.

```sh
ibmcloud resource service-instance-update <INSTANCE_NAME> -p '<{FIELDS_TO_UPDATE}>'
```
{: pre}

See the following example update command for {{site.data.keyword.messagehub}}.

```sh
ibmcloud resource service-instance-update <INSTANCE_NAME> -p '{
   "dataservices":{
      "kafka": {
         "throughput_mb_s": 100,
         "storage_gb": 4000
      }
   }
}'
```
{: pre}

## Managing service credentials
{: #cli-manage-users}

To create a service credential, run the following command.

```sh
ibmcloud resource service-key-create <NAME> [<ROLE_NAME>] ( --instance-id <SERVICE_INSTANCE_ID> | --instance-name <SERVICE_INSTANCE_NAME>) [--service-id <SERVICE_ID>]
```
{: pre}

To delete a service credential, run the following command.

```sh
ibmcloud resource service-key-delete ( <NAME> | <ID> ) [-g <RESOURCE_GROUP>]
```
{: pre}

Updating service credentials is not supported for VPC instances.
{: note}
