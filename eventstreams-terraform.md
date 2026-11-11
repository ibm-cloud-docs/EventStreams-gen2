---

copyright:
  years: 2026
lastupdated: "2026-11-11"

keywords: terraform, schematics, automation, declarative, Event streams

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Manage {{site.data.keyword.messagehub}} with Terraform on {{site.data.keyword.cloud_notm}}
{: #eventstreams-terraform}

[Gen 2]{: tag-purple}

Learn how to use Terraform to manage your {{site.data.keyword.messagehub}} instance on {{site.data.keyword.cloud}}, following infrastructure as code (IaC) principles.
{: shortdesc}

You can manage your {{site.data.keyword.messagehub_full}} instance by using Terraform. Terraform helps you create and change your instance predictably, following Infrastructure as Code (IaC) principles. Terraform is an open source tool that codifies APIs into declarative configuration files. Team members can share these files, and you can edit, review, and version them like code. You describe what your infrastructure should look like, and Terraform creates, updates, and removes cloud resources as needed. For more information, see [Understand the basics of Terraform](https://developer.hashicorp.com/terraform/intro){: external}.

To support a multi-cloud approach, Terraform works with providers. A provider understands API interactions and exposes resources. {{site.data.keyword.cloud_notm}} has a provider for Terraform that you can use to manage {{site.data.keyword.cloud_notm}} resources. For more information, see the [IBM Terraform documentation](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/).

## Installing Terraform
{: #eventstreams-installing-terraform}

To install Terraform, follow the steps in [Install Terraform](https://developer.hashicorp.com/terraform/tutorials/docker-get-started/install-cli?in=terraform%2Fdocker-get-started){: external}.


## Terraform examples for {{site.data.keyword.messagehub}} Gen 2
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
