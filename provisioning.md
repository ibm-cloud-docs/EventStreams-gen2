---

copyright:
  years: 2025
lastupdated: "2025-12-15"

keywords: provision, create, cli, vpc, service key

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Provisioning
{: #provisioning}

[Gen 2]{: tag-purple}

{{site.data.keyword.databases-for}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see the [Beta reference](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-icd-gen2-beta).
{: beta}

Provision an {{site.data.keyword.messagehub}} deployment through the [catalog](https://cloud.ibm.com/catalog){: external}, the CLI or through [Terraform](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs){: external}.

## Step 1: Select your plan
{: #choose_plan}

{{site.data.keyword.messagehub}} offers different plans according to the generation of the platform, see [Choosing your plan](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose){: external}. For the Gen2 platform, the [Enterprise Gen 2 plan](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose) can be selected.

The [Enterprise Gen 2 plan](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose) offers pay-as-you-go access to an isolated single-tenant {{site.data.keyword.messagehub}} service running on the Gen2 (VPC) platform. In addition to a selection of throughput and storage options, this plan also offers user-managed encryption private endpoints and meets a higher number of regulatory compliance standards. The Enterprise Gen2 plan is the best choice if data isolation, scalability, security, guaranteed performance, and increased retention are important considerations.

## Step 2: Provision using the console
{: #provision_instance_ui}
{: ui}

1. Log in to the {{site.data.keyword.cloud_notm}} console.

2. In the **Catalog**, select the **[{{site.data.keyword.messagehub}} service]**(https://cloud.ibm.com/catalog/event-streams){: external}.

3. Select a **Location** that supports the Gen 2 platform.

4. From the **Select a pricing plan** section, select the **Enterprise Gen 2 plan** .

5. Enter a name for your service. You can use the default value.

6. Click **Create**. The {{site.data.keyword.messagehub}} **Resource list** page opens.

7. When your instance is created, click on the instance name to view more information.

## Step 3: Create a service credential using the console
{: #create_credential_ui}
{: ui}

To allow you to connect to your {{site.data.keyword.messagehub}} instance, create a service key by using the {{site.data.keyword.cloud_notm}} console:

1. In the **Resource list**, select your {{site.data.keyword.messagehub}} service.
2. Click **Service credentials**.
3. Click **New credential**.
4. Complete the details for your new credential, such as a name and role and click **Add**. A new credential appears in the credentials list.
5. Expand the new credential's section to reveal the details in JSON format.

## Step 2: Provisioning using the CLI
{: #provision-with-cli}
{: cli}

Before provisioning, follow the instructions provided in the documentation to install the [{{site.data.keyword.cloud_notm}} CLI tool](/docs/cli?topic=cli-install-ibmcloud-cli){: external}.

1. Log in to {{site.data.keyword.cloud_notm}}. If you use a federated user ID, it's important that you switch to a one-time passcode (`ibmcloud login --sso`), or use an API key (`ibmcloud --apikey key or @key_file`) to authenticate. For more information about how to log in by using the CLI, see [General CLI (ibmcloud) commands](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login){: external} under `ibmcloud login`.

      ```sh
      ibmcloud login
      ```
      {: pre}

2. You can create an instance by using the following command:

    ```sh
    ibmcloud resource service-instance-create <NAME> <SERVICE_NAME> <SERVICE_PLAN_NAME> <LOCATION> -g <RESOURCE_GROUP>
    ```
    {: .pre}

Example command for {{site.data.keyword.messagehub}}:

```sh
ibmcloud resource service-instance-create <NAME> messagehub enterprise-gen2 ca-mon -g Default -p '{
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
{: .pre}

## Step 3: Create a service credential using the CLI
{: #create_credential_cli}
{: cli}

To create a service credential:

```sh
ibmcloud resource service-key-create NAME [ROLE_NAME] --instance-name SERVICE_INSTANCE_NAME
```
{: .pre}

Replace the variables with your own values:

- NAME - replace with the name to use for the service credential.
- ROLE_NAME - replace with the name of the IAM role for the service credential (MANAGER, WRITER, or READER].
- SERVICE_INSTANCE_NAME - replace with the name of the service instance the service credential is to be associated with.

For more information, see [using the CLI](/docs/EventStreams-gen2?topic=EventStreams-gen2-cli-reference).

## Step 2: Provisioning using Terraform
{: #provision-with-terraform}
{: terraform}

To create an {{site.data.keyword.messagehub}} instance with default provisioning parameters:

```tf
resource "ibm_resource_instance" "es_instance_1" {
  name              = "my_es_instance_1"
  service           = "messagehub"
  plan              = "enterprise-gen2"
  location          = "ca-mon"

  timeouts {
    create = "15m"
    update = "15m"
    delete = "15m"
  }
```

## Step 3: Create a service credential using Terraform
{: #create_credential_terraform}
{: terraform}

To create a service credential:

```tf
resource "ibm_resource_key" "serviceKey" {
  name                  = "NAME"
  role                  = "ROLE_NAME"
  resource_instance_id  = "${ibm_resource_instance.es_instance_1.id}"
}
```

Replace the variables with your own values:

- NAME - replace with the name to use for the service credential.
- ROLE_NAME - replace with the name of the IAM role for the service credential (MANAGER, WRITER, or READER].

For more information, see [using Terraform](docs-draft/EventStreams-gen2?topic=EventStreams-gen2-eventstreams-terraform).
