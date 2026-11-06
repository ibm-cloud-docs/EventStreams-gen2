---

copyright:
  years: 2026
lastupdated: "2026-11-06"

keywords: data portability, data management, operations, compliance, disaster recovery, data ownership, responsibilities

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Exporting data from {{site.data.keyword.messagehub}}: Understanding data portability
{: #data-portability}

[Gen 2]{: tag-purple}

Learn about data portability for {{site.data.keyword.messagehub}} on {{site.data.keyword.cloud_notm}}, including tools and procedures to export digital artifacts and configuration for use on other cloud providers or on-premises software.
{: shortdesc}

[Data portability](#x2113280){: term} includes the tools and procedures that enable you to export digital artifacts to implement similar workloads on other cloud providers or on-premises software. It includes procedures for copying and storing your content and configuration in your own environment.

## Responsibilities
{: #data-portability-responsibilities}

{{site.data.keyword.cloud_notm}} services provide interfaces and instructions to guide you in copying and storing your content and related configuration in your chosen location.

You are responsible for using the exported data and configuration for data portability to other infrastructures. This responsibility includes:

- Planning and executing the setup of alternate infrastructure on other cloud providers or on-premises software that provides similar capabilities to IBM services.
- Porting application code to the alternate infrastructure, including adapting code and deployment automation.
- Converting exported data and configuration to the formats required by the alternate infrastructure and applications.

For more information about shared responsibilities between {{site.data.keyword.IBM_notm}} and customers, see [Shared responsibilities for {{site.data.keyword.cloud_notm}} products](/docs/overview?topic=overview-shared-responsibilities).

For more information about responsibilities when you use {{site.data.keyword.messagehub}}, see [Shared responsibilities for {{site.data.keyword.messagehub}}](/docs/EventStreams-gen2?topic=EventStreams-gen2-event_streams_responsibilities).

## Data export procedures
{: #data-portability-procedures}

{{site.data.keyword.messagehub}} provides mechanisms to export content that is stored and processed by using the service:

- You can access and export message data by using standard Apache Kafka APIs as described in [Using the Kafka API](/docs/EventStreams-gen2?topic=EventStreams-gen2-using_kafka_api). Data that exceeds the configured retention period is deleted and is no longer available. For more information, see [Limits and quotas](/docs/EventStreams-gen2?topic=EventStreams-gen2-kafka_quotas).
- To export topic configuration, use the [Administration REST API](/docs/EventStreams?topic=EventStreams-admin_api). This API is not currently available on the Enterprise Gen 2 plan.
- To export schema definitions, use the [Schema Registry REST API](/docs/EventStreams?topic=EventStreams-ES_schema_registry). This API is not currently available on the Enterprise Gen 2 plan.

## Exported data formats
{: #data-portability-data-formats}

Data exported through the Apache Kafka APIs retains the binary format in which it was produced. Topic and schema definitions follow the JSON structures defined by their respective REST APIs.

Schema Registry is not currently available on the Enterprise Gen 2 plan.
{: note}

## Data ownership
{: #data-ownership}

All exported data is classified as customer content. You retain full customer ownership and licensing rights as stated in the [IBM Cloud Service Agreement](https://www.ibm.com/support/customer/csol/terms/?id=Z126-6304_WS){: external}.
