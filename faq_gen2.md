---

copyright:
  years: 2026
lastupdated: "2026-11-11"

keywords: gen 2, generation 2, vpc, classic, migration, differences, enterprise, kafka 4.1, private endpoints

subcollection: EventStreams-gen2

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.messagehub}} Gen 2 FAQs: Comparing VPC and Classic versions
{: #gen2-faq}

[Gen 2]{: tag-purple}

Get answers to frequently asked questions about {{site.data.keyword.messagehub}} Gen 2 on {{site.data.keyword.cloud}} VPC, including differences from the Classic version and key features.
{: shortdesc}

## What is {{site.data.keyword.messagehub}} Gen 2?
{: #faq-what-is-gen2}
{: faq}

{{site.data.keyword.messagehub}} Gen 2 is the next generation of the {{site.data.keyword.messagehub}} service built on {{site.data.keyword.IBM_notm}}'s latest VPC platform. It features secure, software-defined networking with private endpoints and isolated compute hosting. Gen 2 is designed for modern, cloud-native applications that require advanced networking capabilities, secure isolation, and consistent performance. Gen 2 runs Apache Kafka 4.1 and provides hypervisor-level isolation with dedicated storage bandwidth.

## What are the main differences between Gen 2 and Classic (Gen 1)?
{: #faq-main-differences}
{: faq}

The key differences include:

Platform
:   Gen 2 runs on VPC with software-defined networking, whereas Classic runs on the original IBM platform.

Kafka version
:   Gen 2 uses Kafka 4.1, and Classic uses Kafka 3.8.

Networking
:   Gen 2 supports private endpoints only (VPE), whereas Classic supports both public and private endpoints.

Plans available
:   Gen 2 offers the Enterprise plan, whereas Classic offers Lite, Standard, and Enterprise plans.

Compute model
:   Gen 2 provides isolated compute hosting with single-tenant virtual machines and dedicated storage bandwidth.

Regions
:   Gen 2 is available only in selected regions.

## Which plan should I choose: Gen 2 Enterprise or Classic Enterprise?
{: #faq-which-plan}
{: faq}

Choose Gen 2 Enterprise for the following requirements:

- Private-only connectivity by using virtual private endpoints (VPE)
- Latest Apache Kafka 4.1 features
- Hypervisor-level isolation with dedicated storage bandwidth
- VPC-based networking

Choose Classic Enterprise for the following requirements:

- Public endpoint connectivity
- Managed Schema Registry (not available in Gen 2)
- Mirroring capabilities (not available in Gen 2)
- Deployment across all global regions
- Established compliance certifications (Gen 2 certifications are pending)

## What APIs and tools are supported in Gen 2?
{: #faq-apis-tools}
{: faq}

Gen 2 supports the following tools and interfaces:

Kafka API
:   Full support for native Kafka clients.

Resource Controller API
:   Instance lifecycle management and provisioning.

CLI
:   The {{site.data.keyword.cloud_notm}} Resource Controller CLI.

Terraform
:   Support for infrastructure as code.

Gen 2 does not currently support the following tools and interfaces:

- Admin REST API
- REST Producer API
- Schema Registry API
- {{site.data.keyword.messagehub}} CLI plug-in (use the Resource Controller CLI instead)

## Can I use the {{site.data.keyword.messagehub}} CLI plug-in with Gen 2?
{: #faq-cli-plugin}
{: faq}

No. The {{site.data.keyword.messagehub}} CLI plug-in supports only Gen 1 (Classic) instances. For Gen 2 instances, use the {{site.data.keyword.cloud_notm}} Resource Controller CLI commands for operations such as creating instances, updating configurations, and managing service credentials. For more information, see [Getting started with the CLI](/docs/EventStreams-gen2?topic=EventStreams-gen2-cli-reference).

## What throughput and storage options are available in Gen 2?
{: #faq-throughput-storage}
{: faq}

Gen 2 Enterprise currently offers the following options:

Throughput
:   100 MB/s (50 MB/s producing and 50 MB/s consuming) as a continuously available minimum for typical workloads.

Storage
:   2 TB, 4 TB, or 6 TB of usable storage.

Partitions
:   Up to 3000 partitions.

Connections
:   Up to 1000 connected applications.

Throughput scaling is planned for a future release.
{: note}

## What features from Classic are not yet available in Gen 2?
{: #faq-missing-features}
{: faq}

The following features are planned for future releases:

- Managed Schema Registry
- Mirroring capabilities
- Capacity scaling (throughput as well as storage)
- Context-based restrictions (CBR)
- Multiple region availability
- Admin REST API and REST Producer API

## How do I migrate from Classic to Gen 2?
{: #faq-migration}
{: faq}

Direct automated migration is not supported. To migrate from Classic to Gen 2, complete the following steps:

1. Provision a new Gen 2 instance in a supported region (such as Dallas, Frankfurt, London, Madrid, Montreal, Mumbai, Osaka, São Paulo, Sydney, Tokyo, Toronto, or Washington, D.C.).
2. Configure VPE connectivity for private networking.
3. Reconfigure your applications to use the new Kafka bootstrap servers.
4. Migrate message data by using Kafka tools or custom consumer-producer applications.
5. Update automation scripts to use the Resource Controller API or CLI instead of {{site.data.keyword.messagehub}}-specific tools.

Before you migrate, evaluate whether Gen 2 meets your requirements for API support, regional availability, and feature parity.
{: note}

## What networking options are available in Gen 2?
{: #faq-networking}
{: faq}

Gen 2 supports **private endpoints only** by using virtual private endpoints (VPE). Public endpoint connectivity is not available. This configuration provides isolation and security through software-defined networking in your VPC. Classic Enterprise supports both public and private endpoints, and Classic Standard and Lite support public endpoints only.

## What compliance certifications does Gen 2 have?
{: #faq-compliance}
{: faq}

Certifications equivalent to Classic are pending, including GDPR, ISO 27001, ISO 27017, ISO 27018, SOC 1, SOC 2, SOC 3, PCI DSS, and HIPAA readiness. If specific compliance certifications are required for your workload, continue using Classic Enterprise until Gen 2 certifications are complete.

## How is pricing different between Gen 2 and Classic?
{: #faq-pricing}
{: faq}

Gen 2 Enterprise follows an hourly and monthly consumption model similar to Classic Enterprise. For current pricing details, see the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} and search for `{{site.data.keyword.messagehub}}`.

## What Kafka version does Gen 2 use?
{: #faq-kafka-version}
{: faq}

Gen 2 uses Apache Kafka 4.1, whereas Classic uses Kafka 3.8. Therefore, Gen 2 includes the latest Kafka features and improvements. Ensure that your Kafka clients are compatible with Kafka 4.1 before connecting to Gen 2 instances.

## Can I scale my Gen 2 instance?
{: #faq-scaling}
{: faq}

You can scale storage capacity (2 TB, 4 TB, or 6 TB). As with Classic, you can't scale down storage capacity. To reduce storage, you must provision a new instance with lower capacity and migrate your data.

Throughput scaling is planned for a future release. Gen 2 instances are currently provisioned with 100 MB/s throughput allocation.

## What are the partition limits in Gen 2?
{: #faq-partitions}
{: faq}

You can create a maximum of 3000 partitions on a Gen 2 instance.

## Can I use Kafka Connect and Kafka Streams with Gen 2?
{: #faq-kafka-tools}
{: faq}

Yes. Both Kafka Connect and Kafka Streams are supported on Gen 2, consistent with Classic Standard and Enterprise plans.

## What is the maximum message size in Gen 2?
{: #faq-message-size}
{: faq}

The maximum message size is 1 MB across all {{site.data.keyword.messagehub}} plans (Lite, Standard, Enterprise Classic, and Enterprise Gen 2).

## How many concurrent connections does Gen 2 support?
{: #faq-connections}
{: faq}

Gen 2 Enterprise supports up to 1000 concurrently active Kafka client connections.

## Where can I provision Gen 2 instances?
{: #faq-regions}
{: faq}

Gen 2 is available in selected {{site.data.keyword.cloud_notm}} regions. For the current list of supported multizone regions, see [Choosing a plan](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose).

## What encryption options are available in Gen 2?
{: #faq-encryption}
{: faq}

Gen 2 supports customer-managed encryption (bring your own key, or BYOK) by integrating with {{site.data.keyword.keymanagementserviceshort}}. With this capability, you manage encryption keys for data at rest. You can specify the root key CRN during provisioning by using the `dataservices.encryption.disk` parameter.
