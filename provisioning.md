---

copyright:
  years: 2026
lastupdated: "2026-11-10"

keywords: provision, create, cli, vpc, service key

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Provisioning {{site.data.keyword.cloud_notm}} {{site.data.keyword.messagehub}}
{: #provisioning}

[Gen 2]{: tag-purple}

Learn how to provision {{site.data.keyword.cloud_notm}} {{site.data.keyword.messagehub}} by using the console, CLI, or Terraform with the Enterprise Gen 2 plan for isolated and secure workloads.
{: shortdesc}

## Step 1: Select your plan
{: #choose_plan}

{{site.data.keyword.messagehub}} offers plans based on platform generation. For more information, see [Choosing your plan](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose).

The [Enterprise Gen 2 plan](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose) provides dedicated single-tenant {{site.data.keyword.messagehub}} service instances on the VPC platform. In addition to scalable throughput and storage options, this plan offers customer-managed encryption and private endpoints. The Enterprise Gen 2 plan is designed for workloads where data isolation, security, performance, and extended retention are essential.

## Step 2: Provisioning by using the console
{: #provision_instance_ui}
{: ui}

1. Log in to the {{site.data.keyword.cloud_notm}} console.
1. In the [**Catalog**](https://cloud.ibm.com/catalog){: external}, search for `{{site.data.keyword.messagehub}}`, then click the {{site.data.keyword.messagehub}} tile to open the provisioning page.
1. Select a location that supports the Gen 2 platform.
1. In the **Select a pricing plan** section, select the **Enterprise Gen 2 plan**.
1. Enter a name for your service. You can use the default value.
1. Click **Create**. The {{site.data.keyword.messagehub}} **Resource list** page opens.
1. When your instance is created, click the instance name to view more information.

## Step 3: Creating a service credential by using the console
{: #create_credential_ui}
{: ui}

To connect to your {{site.data.keyword.messagehub}} instance, create a service key by using the {{site.data.keyword.cloud_notm}} console:

1. In the **Resource list**, select your {{site.data.keyword.messagehub}} service.
1. Click **Service credentials**.
1. Click **New credential**.
1. Complete the details for your new credential, such as a name and role, and click **Add**. A new credential appears in the credentials list.
1. Expand the new credential section to view the details in JSON format.

## Step 2: Provisioning by using the CLI
{: #provision-with-cli}
{: cli}

Before you provision an instance, install the [{{site.data.keyword.cloud_notm}} CLI tool](/docs/cli?topic=cli-install-ibmcloud-cli).

1. Log in to {{site.data.keyword.cloud_notm}}. If you use a federated ID, log in with a one-time passcode (`ibmcloud login --sso`) or an API key (`ibmcloud login --apikey <key>`). For more information, see [General CLI commands](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login).

   ```sh
   ibmcloud login
   ```
   {: pre}

1. Run the following command to create an instance:

   ```sh
   ibmcloud resource service-instance-create NAME SERVICE_NAME SERVICE_PLAN_NAME LOCATION -g RESOURCE_GROUP
   ```
   {: pre}

The following example command creates an {{site.data.keyword.messagehub}} instance:

```sh
ibmcloud resource service-instance-create NAME messagehub enterprise-gen2 ca-mon -g Default -p '{
   "dataservices":{
      "kafka": {
        "throughput_mb_s": 100,
        "storage_gb": 2000
      },
      "encryption": {
         "disk": "crn:v1..."
      },
      "$schema": {
         "version": "1.0.0"
      }
   }
}'
```
{: pre}

## Step 3: Creating a service credential by using the CLI
{: #create_credential_cli}
{: cli}

To create a service credential, run the following command:

```sh
ibmcloud resource service-key-create NAME [ROLE_NAME] --instance-name SERVICE_INSTANCE_NAME
```
{: pre}

Replace the following variables with your values:

`NAME`
:   The name of the service credential.

`ROLE_NAME`
:   The IAM role for the credential (`Manager`, `Writer`, or `Reader`).

`SERVICE_INSTANCE_NAME`
:   The name of the service instance.

For more information, see [Getting started with the CLI](/docs/EventStreams-gen2?topic=EventStreams-gen2-cli-reference).

## Step 2: Provisioning by using Terraform
{: #provision-with-terraform}
{: terraform}

To create an {{site.data.keyword.messagehub}} instance by using Terraform, use the following resource configuration:

```hcl
resource "ibm_resource_instance" "es_instance_1" {
  name     = "my_es_instance_1"
  service  = "messagehub"
  plan     = "enterprise-gen2"
  location = "ca-mon"

  timeouts {
    create = "15m"
    update = "15m"
    delete = "15m"
  }
}
```
{: codeblock}

## Step 3: Creating a service credential by using Terraform
{: #create_credential_terraform}
{: terraform}

To create a service credential by using Terraform, use the following resource configuration:

```hcl
resource "ibm_resource_key" "serviceKey" {
  name                 = "my-service-key"
  role                 = "Writer"
  resource_instance_id = ibm_resource_instance.es_instance_1.id
}
```
{: codeblock}

Replace the configuration values with your desired credential name and role (`Manager`, `Writer`, or `Reader`).

For more information, see [Using Terraform](/docs/EventStreams-gen2?topic=EventStreams-gen2-eventstreams-terraform).
