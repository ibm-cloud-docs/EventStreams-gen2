---

copyright:
  years: 2026
lastupdated: "2026-10-05"

keywords: event streams cbr, context-based restrictions, gen2

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

Support for context-based restrictions in Gen2 is currently available in the au-syd region, with further regions being rolled out.
{: .important}

This document outlines the process for using context-based restrictions to protect your {{site.data.keyword.messagehub}} resources. Use this document to prepare your resources for context-based restrictions. 

# Context-based restrictions
{: #cbr}

Context-based restrictions give account owners and administrators the ability to define access rules that limit the network locations that connections are accepted from. For example, network type, IP ranges, VPC, or other services.

These restrictions work with traditional IAM policies, which are based on identity, to provide an extra layer of protection. Unlike IAM policies, context-based restrictions don't assign access. Context-based restrictions check that an access request comes from an allowed context that you configure. Since both IAM access and context-based restrictions enforce access, context-based restrictions offer protection even in the face of compromised or mismanaged credentials. For more information, see [What are context-based restrictions](/docs/account?topic=account-context-restrictions-whatis).

A user must have the Administrator role on the {{site.data.keyword.messagehub}} service to create, update, or delete rules. A user must also have either the Editor or Administrator role on the context-based restrictions service to create, update, or delete network zones. A user with the Viewer role on the context-based restrictions service can add network zones to a rule.
{: note}

Any {{site.data.keyword.cloudaccesstraillong_notm}} or audit log events generated come from the context-based restrictions service, not {{site.data.keyword.messagehub}}.

{{site.data.keyword.messagehub}} does not support Report-only mode. 
{: important}

To start protecting your {{site.data.keyword.messagehub}} resources with context-based restrictions, see the tutorial for [Leveraging context-based restrictions to secure your resources](/docs/account?topic=account-context-restrictions-tutorial).

## How {{site.data.keyword.messagehub}} integrates with context-based restrictions
{: #cbr-overview}

You can create context-based restrictions for the {{site.data.keyword.messagehub}} service, specific resources, and specific APIs.

Context-based restrictions rules are scoped to the Event Streams service and as such apply to both Gen1 and Gen2 instances.
{: note}

### Protecting {{site.data.keyword.messagehub}} resources
{: #cbr-overview-protect-services}

You can create context-based restrictions rules to protect specific **regions**, **resource groups**, and **instances**.

The scope of the rules is that they apply to all connections to the user's instance. Administration functions for the service instance itself (for example the {{site.data.keyword.Bluemix_notm}} CLI `service-instance-create`, `service-instance-delete` or `service-instance-update` commands, or equivalent) are not under the scope of the context-based restrictions rules that are created against an {{site.data.keyword.messagehub}} instance.
{: important}

Region
:   Protects {{site.data.keyword.messagehub}} resources in a specific region. If you include a region in your context-based restrictions rule, resources in the network zones that you associate with the rule can interact with resources only in that region. If you use the CLI, you can specify the `--region` option to protect resources in a specific region. If you use the UI, you can specify *Region* in the resource attributes.

Resource groups
:   Protects a specific resource group. If you include a resource group in your context-based restrictions rule, resources in the network zones that you associate with the rule can interact only with resources in that resource group. Scoping a rule to a specific resource group is available only for rules that protect the cluster API type. If you use the CLI, you can specify the `--resource-group-id` option to protect resources in a specific resource group. If you use the UI, you can specify the *Resource group* in the resource attributes.

Instance
:   Protects a specific instance. If you include an instance in your context-based restrictions rule, resources in the network zones that you associate with the rule can interact only with resources in that instance. Scoping a rule to a specific instance is available only for rules that protect the cluster API type. If you use the CLI, you can specify the `--service-instance` option to protect instances in a specific resource group. If you use the UI, you can specify the *Service instance* in the resource attributes.

### Using the Command Line Interface (CLI)
{: cli}

You can create and manage context-based restrictions with the IBM Cloud CLI by [installing the context-based restrictions CLI plug-in](/docs/cli?topic=cli-cbr-plugin).

## Creating network zones
{: #network-zone}

A network zone represents an allowlist of IP addresses where an access request is created. It defines a set of one or more network locations that are specified by the following attributes:

* IP addresses, which include individual addresses, ranges, or subnets.
* VPCs.

### Creating network zones in the UI
{: #network-zone-ui}
{: ui}

1. Go to **Manage** > **Context-based restrictions** in the {{site.data.keyword.cloud}} console.
1. Select **Network zones**.
1. Click **Create**.
1. Name your network zone and provide a description.
1. Enter your *Allowed IP addresses.* You can enter a single IP address, a range of IP addresses, or a single CIDR.

   The **Denied IP addresses** field is optional and should include only exceptions that are contained within the IP ranges you provide in the allowed IP addresses field.
   {: note}

1. Choose your *Allowed VPCs*, selecting as many as you like.


### Create network zones in the CLI
{: #network-zone-cli}
{: cli}

To create network zones in the CLI, use the `cbr-zone-create` command to add resources to network zones. For more information, see the [context-based restrictions CLI reference](/docs/account?topic=account-cbr-plugin#cbr-zones-cli).

Create a zone by using a command like:

```sh
ibmcloud cbr zone-create --addresses=1.1.1.1,5.5.5.5 --name=<NAME>
```
{: .pre}

### Creating network zones in Terraform
{: #zones-tf}
{: terraform}

To create zones in Terraform, follow the instructions in the [IBM Cloud Terraform provider documentation](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/cbr_zone){: external}.

Example Terraform script to create a CBR zone:

```sh
resource "ibm_cbr_zone" "cbr_zone" {
  account_id = "12ab34cd56ef78ab90cd12ef34ab56cd"
  addresses {
    type = "ipAddress"
    value = "169.23.56.234"
  }
  addresses {
    type = "ipRange"
    value = "169.23.22.0-169.23.22.255"
  }
  excluded {
    type  = "ipAddress"
    value = "169.23.22.10"
  }
  excluded {
    type  = "ipAddress"
    value = "169.23.22.11"
  }
  description = "this is an example of zone"
  excluded {
        type = "ipAddress"
        value = "value"
  }
  name = "an example of zone"
}
```

Alternatively, you can also use [Terraform IBM Modules (TIM) for CBR Zone](https://registry.terraform.io/modules/terraform-ibm-modules/cbr/ibm/latest/submodules/cbr-zone-module){: external} to create a zone for context-based restrictions or update addresses in an existing zone.

An example for creating a CBR zone using [Terraform IBM Modules (TIM)](https://registry.terraform.io/modules/terraform-ibm-modules/cbr/ibm/latest){: external}:

```terraform
module "ibm_cbr" "zone" {
  source           = "terraform-ibm-modules/cbr/ibm//modules/cbr-zone-module"
  version          = "X.X.X" # Replace "X.X.X" with a release version to lock into a specific release
  name             = "zone_for_es_access"
  account_id       = "defc0df06b644a9cabc6e44f55b3880s"
  zone_description = "Zone created from terraform"
  addresses        = [{type  = "vpc",value = "vpc_crn"}]
}
```
{: codeblock}

### Update network zones in the CLI
{: #update-network-zone-cli}
{: cli}

Update a zone by using a command like:

```sh
ibmcloud cbr zone-update <ZONE-ID> --addresses=1.2.3.4 --name=<NAME>
```
{: .pre}

Updating requires the `ZONE-ID`, not the zone name. Use the following command to list your zones and retrieve the relevant `ZONE-ID`:

```sh
ibmcloud cbr zones
```
{: .pre}

The `zone-update` command is an overwrite. Include all of the fields that are required as if you are creating the rule from scratch. If you omit any required fields, the rule overwrites those missing fields as empty, and the rule might fail because some of those fields are required, regardless of whether they are changing the rule. {: .important}

### Delete network zones in the CLI
{: #delete-network-zone-cli}
{: cli}

Delete a zone by using a command like:

```sh
ibmcloud cbr zone-delete <ZONE-ID>
```
{: .pre}

## Creating rules
{: #rules}

Rules restrict access to specific cloud resources based on resource attributes and contexts. A created rule can accept up to 2,000 IP/CIDR values for private endpoints and up to 2,000 IP/CIDR values for public endpoints. This limit is specfic to {{site.data.keyword.messagehub}}. Other {{site.data.keyword.cloud}} service limits may vary.

{{site.data.keyword.messagehub}} does not support IPv6 addresses. If an IPv6 address is included, it will be ignored.

### Creating rules in the UI
{: #rules-ui}
{: ui}

1. Go to **Manage** > **Context-based restrictions** in the {{site.data.keyword.cloud}} console.
2. Select **Rules**.
3. Click **Create**.
4. Under **Service**, select {{site.data.keyword.messagehub}} as the service you want to target with your rule.
5. Under **APIs**, select `All`. 
6. Under **Resources**, scope the rule to **All resources** or **Specific resources**. 
7. Click **Continue**.
8. Define the allowed endpoint types.
   - Keep the toggle set to **No** to allow all endpoint types.
   - Set the toggle to **Yes** to allow only specific endpoint types, then choose from the list.
9. Select a network zone or zones that you have already created, or create a new network zone by clicking **Create**.

   Contexts define from where your resources can be accessed, effectively linking your network zone to your rule.
   {: tip}

10. Click **Add** to add your configuration to the summary.
11. Click **Next**.
12. Name your rule.
13. Select how you want to enforce the rule.

   *Report-only* is not available for {{site.data.keyword.messagehub}}.
   {: important}

### Create rules in the CLI
{: #rules-cli}
{: cli}

To create a rule in the CLI, you need the {{site.data.keyword.messagehub}} `service_name`:

* `messagehub`

All the other parameters that follow are explained in the [CBR plugin reference guide](https://cloud.ibm.com/docs/account?topic=account-cbr-plugin#cbr-rules-cli).

Example command for creating a CBR Rule:

```sh
ibmcloud cbr rule-create --enforcement-mode enabled --context-attributes "networkZoneId=<ZONE-ID>" --resource-group-id <RESOURCE_GROUP_ID> --service-name messagehub --service-instance <SERVICE-INSTANCE> --description <DESCRIPTION>
```
{: .pre}

*Report-only* is not available for {{site.data.keyword.messagehub}}.
{: .important}

### Update rules in the CLI
{: #update-rules-cli}
{: cli}

Example command for updating a CBR rule:

```sh
ibmcloud cbr rule-update <RULE-ID> --enforcement-mode disabled --context-attributes="networkZoneId=<ZONE-ID>" --resource-group-id   <RESOURCE_GROUP_ID> --service-name messagehub --description    <DESCRIPTION>
```
{: .pre}

The `rule-update` command is an overwrite. Include all of the fields that are required as if you are creating the rule from scratch. If you omit any required fields, the rule overwrites those missing fields as empty, and the rule might fail because some of those fields are required, regardless of whether they are changing the rule.
{: .important}

Updating requires the `RULE-ID`, not the rule name. Use the following command to list your rules and retrieve the relevant `RULE-ID`:

```sh
ibmcloud cbr rules
```
{: .pre}

### Delete rules in the CLI
{: #delete-rules-cli}
{: cli}

Delete a rule by using a command like:

```sh
ibmcloud cbr rule-delete <RULE-ID>
```
{: .pre}

Use `ibmcloud cbr <command> — help` for a full list of options and parameters. For example, `ibmcloud cbr rule-create — help` outputs parameters for rule creation.
{: .tip}



### Creating rules in Terraform
{: #rules-tf}
{: terraform}

To create rules in Terraform, follow the instructions in the [IBM Cloud Terraform provider documentation](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/cbr_rule){: external}.

To create a rule, you need the {{site.data.keyword.messagehub}} `service_name`:

* `messagehub`

Create a rule by using a command like:

```sh
resource "ibm_cbr_rule" "cbr_rule" {
  contexts {
        attributes {
            name = "networkZoneId"
            value = "559052eb8f43302824e7ae490c0281eb"
        }
        attributes {
               name = "endpointType"
               value = "private"
    }
  }
  description = "this is an example of a rule with one context one zone"
  enforcement_mode = "enabled"
  resources {
        attributes {
            name = "accountId"
            value = "12ab34cd56ef78ab90cd12ef34ab56cd"
        }
        attributes {
              name = "serviceName"
              value = "messagehub"
        }
        tags {
              name     = "tag_name"
              value    = "tag_value"
        }
  }
}
```
{: pre}

### Verifying your rule
{: #rules-ui-verify}

To verify that your rule is applied, go to the {{site.data.keyword.cloud}} Dashboard and select the relevant instance from your *Resource List*. Within **Recent Tasks**, you see your rule's status.

The task of creating or modifying a rule goes into your instance's task queue. Depending on workload, it might take some time for your rule enforcement to complete.
{: .note}
