---

copyright:
  years: 2015, 2026
lastupdated: "2026-11-11"

keywords: protocols, encryption, data isolation, data retention, data isolation model

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Securing data in {{site.data.keyword.cloud_notm}} {{site.data.keyword.messagehub}}: Protocols and best practices
{: #data_security}

Learn about data security and privacy in {{site.data.keyword.cloud}} {{site.data.keyword.messagehub}}, including cryptographic protocols, encryption methods, and data isolation models.
{: shortdesc}

{{site.data.keyword.IBM}} uses the following methods to help ensure the security and privacy of your data: cryptographic protocols, encryption, and data isolation. Each plan provides different data isolation models.

## Cryptographic protocols - THIS SECTION NEEDS INPUT FROM TEAM TO BE UPDATED
{: #cryptographic}

* Connections are restricted to the following strong cipher suites:

For TLS v1.2:

      * TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256
      * TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
      * TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256

For TLS v1.3:

      * TLS_AES_128_GCM_SHA256
      * TLS_AES_256_GCM_SHA384
      * TLS_CHACHA20_POLY1305_SHA256

* To be a fully supported configuration, all clients must support the following:

    * TLS v1.2 or v1.3
    * Elliptic curve cryptography
    * TLS server name indication (SNI)

* Additionally, you must use TLS v1.2 or v1.3 in the following cases:

    * To make connections to the Kafka native and REST interfaces.
    * The browser that you use to access the {{site.data.keyword.messagehub}} dashboard must support TLS v1.2 or v1.3.

## Encryption of message payloads, topic names, and consumer groups
{: #encryption_payloads}

TLS encrypts message data in transit between {{site.data.keyword.messagehub}} and clients. {{site.data.keyword.messagehub}} stores message data at rest and message logs on encrypted disks.

TLS encrypts topic names and consumer groups in transit between {{site.data.keyword.messagehub}} and clients. However, {{site.data.keyword.messagehub}} does not encrypt these values at rest. Therefore, do not use confidential information in your topic names.

For information about compliance on each of the {{site.data.keyword.messagehub}} plans, see [What is supported by the Lite, Standard, Enterprise, and Enterprise Gen 2 plans](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose#what_is_supported).

## Data isolation model
{: #data_isolation}

### Enterprise Gen 2 plan
{: #data_isolation_enterprise}

The Enterprise Gen 2 plan provides a tenant-specific service in the {{site.data.keyword.IBM_notm}} service domain. The Enterprise Gen 2 plan creates a single-tenant instance on a shared Kubernetes cluster on shared hardware (VSI isolation). The Enterprise Gen 2 plan provides private endpoints only for network isolation. The Enterprise Gen 2 plan creates single-tenant {{site.data.keyword.blockstorageshort}} for each new instance.

## Data retention and reclamation
{: #data_retention_reclamation}

On all plans, when you delete a service instance, {{site.data.keyword.messagehub}} does not delete the data immediately. Instead, it schedules the data for reclamation and sets the retention period to three days. After this period, the data (both topics and messages that are written to the topics) is irreversibly destroyed. You can also restore a deleted instance that is not yet reclaimed.

You can check the status of a reclamation, and force or cancel a scheduled reclamation by using the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_reclamations).
