---

copyright:
  years: 2026
lastupdated: "2026-02-26"

keywords: gen 2, generation 2, vpc, classic, migration, differences, enterprise, kafka 4.1, private endpoints

subcollection: EventStreams-gen2

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for {{site.data.keyword.messagehub}} Gen 2
{: #gen2-faq}

[Gen 2]{: tag-purple}

Frequently asked questions about {{site.data.keyword.messagehub}} Gen 2 (VPC) and how it differs from the Classic (Gen 1) version. To find all of the FAQs for {{site.data.keyword.cloud}}, see our [FAQ library](/docs/faqs).
{: shortdesc}

## What is {{site.data.keyword.messagehub}} Gen 2?
{: #faq-what-is-gen2}
{: faq}

{{site.data.keyword.messagehub}} Gen 2 is the next generation of the {{site.data.keyword.messagehub}} service built on {{site.data.keyword.IBM_notm}}'s latest VPC platform. It features highly secure, software-defined networking with private endpoints and isolated compute hosting. Gen 2 is designed for modern, cloud-native applications that require advanced networking capabilities, secure isolation, and consistent performance. Gen 2 runs Apache Kafka 4.1 and provides hypervisor-level isolation with dedicated storage bandwidth.

## What are the main differences between Gen 2 and Classic (Gen 1)?
{: #faq-main-differences}
{: faq}

The key differences include:

- **Platform**: Gen 2 runs on VPC with software-defined networking, while Classic runs on IBM's original platform.
- **Kafka version**: Gen 2 uses Kafka 4.1, Classic uses Kafka 3.8.
- **Networking**: Gen 2 supports private endpoints only (VPE), Classic supports both public and private endpoints.
- **Plans available**: Gen 2 offers Enterprise plan, Classic offers Lite, Standard, and Enterprise plans.
- **Compute model**: Gen 2 provides isolated compute hosting with single-tenant virtual machines and dedicated storage bandwidth.
- **Regions**: Gen 2 is currently available only in selected regions.

## Which plan should I choose - Gen 2 Enterprise or Classic Enterprise?
{: #faq-which-plan}
{: faq}

Choose Gen 2 Enterprise if you need:

- Private-only connectivity via VPE
- Latest Kafka 4.1 features
- Hypervisor-level isolation with dedicated storage bandwidth
- Modern VPC-based networking

Choose Classic Enterprise if you need:

- Public endpoint connectivity
- Managed Schema Registry (not yet available in Gen 2)
- Mirroring capabilities (not yet available in Gen 2)
- Deployment in the broadest set of regions
- Established compliance certifications (Gen 2 certifications are pending)

## What APIs and tools are supported in Gen 2?
{: #faq-apis-tools}
{: faq}

Gen 2 currently supports:

- **Kafka API**: Full support for native Kafka clients
- **Resource Controller API**: For instance management and provisioning
- **CLI**: IBM Cloud Resource Controller CLI (not the {{site.data.keyword.messagehub}} plugin)
- **Terraform**: Full support for infrastructure as code

Gen 2 does NOT currently support:

- Admin REST API
- REST Producer API
- Schema Registry API
- {{site.data.keyword.messagehub}} CLI plugin (use Resource Controller CLI instead)

## Can I use the {{site.data.keyword.messagehub}} CLI plugin with Gen 2?
{: #faq-cli-plugin}
{: faq}

No, the {{site.data.keyword.messagehub}} CLI plugin only supports Gen 1 (Classic) instances. For Gen 2 instances, you must use the IBM Cloud Resource Controller CLI commands. This includes operations like creating instances, updating configurations, and managing service credentials. See the [CLI reference documentation](/docs/EventStreams-gen2?topic=EventStreams-gen2-cli-reference) for Gen 2-specific commands.

## What throughput and storage options are available in Gen 2?
{: #faq-throughput-storage}
{: faq}

Gen 2 Enterprise currently offers:

- **Throughput**: 100 MB/s (50 MB/s producing and 50 MB/s consuming) as continuously available minimum for typical workloads
- **Storage**: 2/4/6 TB of usable storage
- **Partitions**: 3000 partitions
- **Connections**: 1000 connected applications

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

Direct migration is not currently supported. To move from Classic to Gen 2, you would need to:

1. Provision a new Gen 2 instance in Montreal.
2. Set up VPE connectivity for private networking.
3. Reconfigure your applications to use the new Kafka bootstrap servers.
4. Migrate your data using Kafka tools or custom applications.
5. Update any automation to use Resource Controller API/CLI instead of {{site.data.keyword.messagehub}}-specific tools.

Carefully evaluate whether Gen 2 meets your requirements, especially regarding API support, regional availability, and feature parity before migrating.
{: note}

## What networking options are available in Gen 2?
{: #faq-networking}
{: faq}

Gen 2 supports **private endpoints only** via Virtual Private Endpoints (VPE). Public endpoint connectivity is not available. This provides enhanced security through software-defined networking within your VPC. Classic Enterprise supports both public and private endpoints, while Classic Standard and Lite support public endpoints only.

## What compliance certifications does Gen 2 have?
{: #faq-compliance}
{: faq}

Equivalent certifications as Classic are pending, including GDPR, Privacy Shield, ISO 27001/27017/27018/27701, SOC 1/2/3, HIPAA ready, PCI DSS, ISMAP, C5, IRAP, ENS, HITRUST, and ProtectedB. If specific compliance requirements are critical for your use case, you should continue using Classic Enterprise until Gen 2 certifications are completed.

## How is pricing different between Gen 2 and Classic?
{: #faq-pricing}
{: faq}

Gen 2 Enterprise follows a pay-as-you-go model similar to Classic Enterprise. For current pricing information, see the [IBM Cloud catalog](https://cloud.ibm.com/catalog) and search for {{site.data.keyword.messagehub}}.

## What Kafka version does Gen 2 use?
{: #faq-kafka-version}
{: faq}

Gen 2 uses Apache Kafka 4.1, while Classic uses Kafka 3.8. This means Gen 2 includes the latest Kafka features and improvements. Ensure your Kafka clients are compatible with Kafka 4.1 before connecting to Gen 2 instances.

## Can I scale my Gen 2 instance?
{: #faq-scaling}
{: faq}

Capacity scaling for storage is supported for 2/4/6 TB. Note that like Classic, storage capacity cannot be scaled down - you would need to create a new instance at the lower capacity.

Scaling for throughput is planned for a future release. Currently, Gen 2 instances are provisioned with a fixed throughput capacity (100 MB/s throughput). 

## What are the partition limits in Gen 2?
{: #faq-partitions}
{: faq}

A maximum of 3000 partitions can be created.

## Can I use Kafka Connect and Kafka Streams with Gen 2?
{: #faq-kafka-tools}
{: faq}

Yes, both Kafka Connect and Kafka Streams are supported on Gen 2, just as they are on Classic Standard and Enterprise plans. These are not available on the Classic Lite plan.

## What is the maximum message size in Gen 2?
{: #faq-message-size}
{: faq}

The maximum message size is 1 MB, consistent across all {{site.data.keyword.messagehub}} plans (Lite, Standard, Enterprise Classic, and Enterprise Gen 2).

## How many concurrent connections does Gen 2 support?
{: #faq-connections}
{: faq}

Gen 2 Enterprise supports a maximum of 1000 concurrently active Kafka clients.

## Where can I provision Gen 2 instances?
{: #faq-regions}
{: faq}

Gen 2 is currently available only in selected regions. For a list of the currently supported regions, see [Choosing a plan](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose).

## What encryption options are available in Gen 2?
{: #faq-encryption}
{: faq}

Gen 2 supports customer-managed encryption via Key Protect integration (BYOK - Bring Your Own Key), similar to Classic Enterprise. This allows you to manage your own encryption keys for data at rest. The encryption key CRN can be specified during provisioning using the `dataservices.encryption.disk` parameter.
