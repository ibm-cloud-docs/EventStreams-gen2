---

copyright:
  years: 2025
lastupdated: "2025-12-09"

keywords: terraform, schematics, automation, declarative, Event streams

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.messagehub}} Terraform reference
{: #eventstreams-terraform}

[Gen 2]{: tag-purple}

{{site.data.keyword.messagehub_full}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see the [Beta reference](/docs/EventStreams-gen2?topic=EventStreams-gen2-gen2-beta).
{: beta}

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

## Terraform examples for {{site.data.keyword.messagehub}} Gen2
{: #eventstreams-terraform-examples}

- Create an {{site.data.keyword.messagehub}} instance with default provisioning parameters:

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

- Provisioning with specified throughput and storage size:

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
    
      parameters_json = jsonencode(
        { "dataservices":{"kafka":{"throughput_mb_s":"100","storage_gb":4000}}}
      )
    }
    ```

- Provisioning with customer's encryption key:

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
    
      parameters_json = jsonencode(
        {"dataservices": {"encryption":{"disk":"crn:v1:bluemix:public:your-kms-key-crn..."}}}
      )
    }
    ```
