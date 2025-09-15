---

copyright:
  years: 2015, 2025
lastupdated: "2025-09-15"

keywords: protocols, encryption, data isolation, data retention, data isolation model

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Data security and privacy in {{site.data.keyword.messagehub}}
{: #data_security}

{{site.data.keyword.IBM}} uses the following methods to help ensure the security and privacy of your data: cryptographic protocols, encryption, and data isolation. Each plan provides different data isolation models.
{: shortdesc}

## Cryptographic protocols
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

Message data is encrypted for transmission between {{site.data.keyword.messagehub}} and clients as a result of TLS. {{site.data.keyword.messagehub}} stores message data at rest and message logs on encrypted disks.

Topic names and consumer groups are encrypted for transmission between {{site.data.keyword.messagehub}} and clients as a result of TLS. However, {{site.data.keyword.messagehub}} does not encrypt these values at rest. Therefore, do not use confidential information in your topic names.

For information about compliance on each of the {{site.data.keyword.messagehub}} plans, see [What's supported by the Lite, Standard, and Enterprise plans](/docs/EventStreams?topic=EventStreams-plan_choose#what_is_supported).

## Data isolation model
{: #data_isolation}

{{site.data.keyword.messagehub}}'s data isolation model varies according to which plan you use.

### Enterprise plan
{: #data_isolation_enterprise}

The Enterprise plan provides a tenant-specific service in the {{site.data.keyword.IBM_notm}} service domain. The Enterprise plan creates a single tenant instance on a dedicated Kubernetes cluster on shared hardware (VSI isolation). By default, the Enterprise plan provides public endpoints, but it also supports Cloud service endpoints to enable private endpoints for further network isolation on request. The Enterprise plan creates single tenant {{site.data.keyword.blockstorageshort}} for each new instance.

### Standard plan
{: #data_isolation_standard}

The Standard plan provides a public service with public endpoints. The Standard plan creates a tenant instance on a shared Kubernetes cluster on shared hardware (VSI isolation). The Standard plan provides public endpoints only.

The Standard plan uses shared {{site.data.keyword.blockstorageshort}} and achieves tenant isolation through separation of files and access controls.

### Lite plan
{: #data_isolation_lite}

The Lite plan provides a public service with public endpoints. The Lite plan creates a tenant instance on a shared Kubernetes cluster on shared hardware (VSI isolation). The Lite plan provides public endpoints only.

The Lite plan uses shared {{site.data.keyword.blockstorageshort}} and achieves tenant isolation through separation of files and access controls.

## Data retention and reclamation
{: #data_retention_reclamation}

On all plans, when a service instance is deleted, the data is not deleted immediately. It is scheduled for reclamation and {{site.data.keyword.messagehub}} sets this retention period to three days, after which the data (both topics and messages that are written to the topics) is irreversibly destroyed. It is also possible to restore a deleted instance that is not yet reclaimed.

You can check the status of a reclamation, and force or cancel a scheduled reclamation by using [the {{site.data.keyword.IBM_notm}} Cloud CLI](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_reclamations).
