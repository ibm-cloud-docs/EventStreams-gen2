---

copyright:
  years: 2024
lastupdated: "2024-09-26"

keywords: terraform, schematics, automation, declarative, Event streams 

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.messagehub}} Terraform reference
{: #eventstreams-terraform}

You can manage your {{site.data.keyword.messagehub_full}} instance using Terraform, which enables you to rapidly and predictably create and change your instance following Infrastructure as Code (IaC) principles. Terraform is an open source tool that codifies APIs into declarative configuration files. These files can be shared among team members and are treated as code, edited, reviewed, and versioned. You write down what your infrastructure should look like and Terraform will create, update, and remove cloud resources as needed. For more information, see [Understand the basics of Terraform](https://www.terraform.io/intro){: external}.

To support a multi-cloud approach, Terraform works with providers. A provider is responsible for understanding API interactions and exposing resources. {{site.data.keyword.cloud}} has its provider for Terraform, enabling users of {{site.data.keyword.cloud}} to manage resources with Terraform. For more information, see the [IBM Terraform documentation](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/). 

## Installing Terraform
{: #eventstreams-installing-terraform}

Follow [these steps](https://learn.hashicorp.com/tutorials/terraform/install-cli?in=terraform/docker-get-started){: external} to install Terraform.

## Terraform references
{: #eventstreams-terraform-references}

The following references are available to support {{site.data.keyword.messagehub}} customers using Terraform:

- [{{site.data.keyword.messagehub}} Terraform module](https://github.com/terraform-ibm-modules/terraform-ibm-event-streams): Module that supports provisioning and configuring {{site.data.keyword.messagehub}}.
- [{{site.data.keyword.messagehub}} Terraform examples](https://github.com/IBM-Cloud/terraform-provider-ibm/blob/master/examples/ibm-event-streams/README.md): Shows several {{site.data.keyword.messagehub}} usage scenarios with Terraform including creating an instance, creating a topic, and creating a schema. 
- [Index of Terraform for {{site.data.keyword.messagehub}}]([/docs/EventStreams?topic=EventStreams-kafka_console_tools](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-resources-datasource-list#ibm-event-streams_rd)): Index of Terraform on {{site.data.keyword.cloud}} resources and data sources. 
