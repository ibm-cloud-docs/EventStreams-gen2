---

copyright:
  years: 2025
lastupdated: "2025-12-16"

keywords: event streams, migrating, disk size, memory size, CPU size, resources, cli, postgresql administrator

subcollection: event-streams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.messagehub}} CLI
{: #cli-reference}

[Gen 2]{: tag-purple}

{{site.data.keyword.messagehub_full}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see the [Beta reference](/docs/EventStreams-gen2?topic=EventStreams-gen2-gen2-beta).
{: beta}

To interact with {{site.data.keyword.messagehub}} on Gen 2 via the CLI you must utilize the IBM Cloud Resource Controller's CLI. For more information, see the [General {{site.data.keyword.messagehub}} CLI (ibmcloud) commands](/docs/cli?topic=cli-ibmcloud_cli).

The {{site.data.keyword.messagehub}} plugin supports only Gen 1 instances. For Gen 2 instances, use the Resource Controller CLI.
{: note}

## Getting started - Create an instance
{: #cli-create}

You can create an instance by using the following command:

```sh
ibmcloud resource service-instance-create <INSTANCE_NAME> <SERVICE_NAME> <SERVICE_PLAN_NAME> <LOCATION> -g <RESOURCE_GROUP>
```
{: .pre}

Example command for {{site.data.keyword.messagehub}}:

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
{: .pre}

A number of additional options can also be specified in the parameter block.

```
"dataservices": {
    "kafka": {
        "storage_gb": 2000,
        "throughput_mb_s": 100,
    },
    "encryption": {
        "disk": "crn:v1..."
    },
    "options": {
        "iam_token_only": false,
        "metrics": ["topic", "partition", "consumers"]
    }
},
```
{: .pre}

## Getting information about your instance
{: #cli-getting-info}

You can get instance information using the following command:

```sh
ibmcloud resource service-instance <INSTANCE_NAME> -o JSON
```
{: .pre}

## Update your instance
{: #cli-update-instance}

To update your instance (this includes operations, such as scaling and modifying other parts of your service), use the following command:

```sh
ibmcloud resource service-instance-update <INSTANCE_NAME> -p '<{FIELDS_TO_UPDATE}>'
```
{: .pre}

Example of full updates for {{site.data.keyword.messagehub}}:

```sh
ibmcloud resource service-instance-update <INSTANCE_NAME> -p'{
   "dataservices":{
      "kafka": {
         "throughput_mb_s": 100, <------ Change to the value you desire
         "storage_gb": 4000 <------ Change to the value you desire
      },
   }
}'
```
{: .pre}

## Manage users
{: #cli-manage-users}

To create a user:

```sh
ibmcloud resource service-key-create NAME [ROLE_NAME] ( --instance-id SERVICE_INSTANCE_ID | --instance-name SERVICE_INSTANCE_NAME) [--service-id SERVICE_ID]
```
{: .pre}

To delete a user:

```sh
ibmcloud resource service-key-delete ( NAME | ID ) [-g RESOURCE_GROUP]
```
{: .pre}

Updating a user is not possible for VPC instances.
{: note}
