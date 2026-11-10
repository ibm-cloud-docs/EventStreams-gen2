---

copyright:
  years: 2025, 2026
lastupdated: "2026-11-10"

keywords: plan, Enterprise, Standard, Lite, pricing, throughput, partitions, tenancy, compliance

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Selecting an {{site.data.keyword.messagehub}} plan for your needs
{: #plan_choose}

[Gen 2]{: tag-purple}

Learn about the {{site.data.keyword.messagehub}} Lite, Standard, and Enterprise plans available on {{site.data.keyword.cloud_notm}}, and choose the plan that suits your requirements.
{: shortdesc}

The following page covers all {{site.data.keyword.messagehub_full}} plans including Classic (Gen 1) and VPC (Gen 2).
{: note}

For information about {{site.data.keyword.messagehub}} plan pricing, see the [catalog](https://cloud.ibm.com/catalog){: external}. Search for `{{site.data.keyword.messagehub}}`, then click the {{site.data.keyword.messagehub}} tile to go to the provisioning page.

## Lite plan (Gen 1)
{: #plan_lite}

The Lite plan is free for users who want to try out {{site.data.keyword.messagehub}} or build a proof-of-concept. Do not use the Lite plan for production use. It offers shared access to a multi-tenant {{site.data.keyword.messagehub}} cluster. The Lite plan is only available on Gen 1 (Classic).

## Standard plan (Gen 1)
{: #plan_standard}

The Standard plan is appropriate if you require event ingest and distribution capabilities but do not require any additional benefits of the Enterprise plan. The Standard plan offers shared access to a multi-tenant {{site.data.keyword.messagehub}} cluster that seamlessly autoscales as you increase the number of partitions you are using for your workload.

The architecture is highly available by default. The service is distributed across three availability zones, which means that the cluster is resilient to the failure of a single zone or any component within that zone. The Standard plan is only available on Gen 1 {{site.data.keyword.messagehub}} (Classic).

## Enterprise plan (Gen 1)
{: #plan_enterprise}

The Enterprise plan is appropriate if data isolation, performance, and increased retention are important considerations.
The Enterprise plan includes the following features:

- Exclusive access to a single-tenant {{site.data.keyword.messagehub}} service instance deployed in a highly available multizone region (MZR).
- An option to provision a single-tenant {{site.data.keyword.messagehub}} service instance in a geographically local but single-zone location (SZR).
- Scaling options to customize throughput, storage capacity, or both.
- A broad set of compliance certifications, regions, and features, including public endpoint connectivity, Managed Schema Registry, and mirroring capabilities.

The architecture is highly available when you choose to deploy into a multi-zone region. The service is distributed across three availability zones, which means that the cluster is resilient to the failure of a single zone or any component within that zone.

## Enterprise plan (Gen 2)
{: #plan_enterprise_gen2}

The Enterprise Gen 2 plan is appropriate if data isolation, performance, and increased retention are important considerations.

The Enterprise Gen 2 plan is built on the latest {{site.data.keyword.IBM_notm}} platform with secure software-defined networking, designed for modern cloud-native applications. The Enterprise Gen 2 plan includes the following features:

- VPC-based software-defined networking with private connectivity through virtual private endpoints (VPE)
- Hypervisor-level isolation with dedicated storage bandwidth
- Apache Kafka 4.1 features

The plan is available in select regions with support for private endpoints and Isolated Compute hosting. The Isolated Compute hosting model provides a dedicated Kafka cluster per service instance that runs on single-tenant virtual machines and storage. This model provides hypervisor-level isolation and dedicated storage bandwidth for workloads that require consistent performance and security.

## What is supported by the Lite, Standard, Enterprise, and Enterprise Gen 2 plans
{: #what_is_supported}

As {{site.data.keyword.messagehub}} Gen 2 evolves, additional capabilities are introduced over time. Some differences from Gen 1 are architectural by design. For more information, see [Architectural differences and known limitations](/docs/EventStreams-gen2?topic=EventStreams-gen2-eventstreams-gen1-vs-gen2-differences-limitations).
{: note}

The following table summarizes what is supported by the plans:

|   | Lite plan  |  Standard plan |  Enterprise plan  | Enterprise Gen 2 plan  |
|---|---|---|---|---|
| **Tenancy**  | Multi-tenant   | Multi-tenant  | Single-tenant | Single-tenant |
| **Availability zones**   |  3  |   3  | 3 (1 in single-zone   \n locations)   | 3 |
| **Availability**  |  99.99% [^tabletext1] |  99.99% | 99.99% (99.9% in   \n  single-zone locations) [^tabletext2]  | 99.99% |
| **Kafka version on cluster**  | Kafka 3.8 | Kafka 3.8  | Kafka 3.8 | Kafka 4.1 |
| **Kafka Connect and Kafka Streams supported**  | No |  Yes | Yes  | Yes  |
| **Managed Schema Registry supported**  | No |  No |  Yes |  Future release |
| **Customer-managed encryption**  | No  |  No |  Yes [^tabletext4]  | Yes |
| **Fine-grained access control**  | Yes  |  Yes |  Yes  |  Yes  |
| **Activity tracker events**  | No  |  Yes |  Yes |  Yes |
| **Monitoring Event Streams metrics by using IBM Cloud Monitoring**  | Yes  |  Yes |  Yes |  Yes |
| **Private/Public Networking** | Public   | Public |  Both | Private (VPE) |
| **Scale plan capacity** | No   | No  |  Yes | Future release |
| **Maximum number of partitions**  | 1 [^tabletext5]  | 100   | 3000 - 9000 scales with throughput [^tabletext6] | 3000 |
| **Maximum retention limits**   | 100 MB for the partition   | 1 GB per partition  | 2 TB - 12 TB of scalable   \n usable storage [^tabletext7] | 2 TB of usable storage |
| **Maximum throughput**  | 100 KB per second per partition  |  1 MB per second per partition (20 MB per service instance) | 150 MB/s - 450 MB/s   \n of scalable throughput [^tabletext8] | 100 MB/s (50 MB/s Produce \n 50 MB/s Consume) [^tabletext9]|
| **Maximum message size**  | 1 MB  | 1 MB   | 1 MB | 1 MB |
| **Maximum number of connected clients**  | 5   | 500  | 10,000  | 1000 |
| **Location (region) availability** | Dallas (us-south)  | Multizone locations (MZR): \n Dallas (us-south) \n Sao Paulo (br-sao) \n Toronto (ca-tor) \n Washington (us-east) \n Frankfurt (eu-de) \n London (eu-gb) \n Madrid (eu-es) \n Osaka (jp-osa) \n Sydney (au-syd) \n Tokyo (jp-tok) | Multizone locations (MZR): \n Dallas (us-south) \n Sao Paulo (br-sao) \n Toronto (ca-tor) \n Washington (us-east) \n Frankfurt (eu-de) \n London (eu-gb) \n Madrid (eu-es) \n Osaka (jp-osa) \n Sydney (au-syd) \n Tokyo (jp-tok) \n \n Single-zone locations (SZR): \n Chennai (che01) | Multizone locations (MZR): \n Dallas (us-south) \n Frankfurt (eu-de) \n London (eu-gb) \n Madrid (eu-es) \n Osaka (jp-osa) \n São Paulo (br-sao) \n Sydney (au-syd) \n Tokyo (jp-tok) \n Toronto (ca-tor) \n Washington (us-east) \n \n Single-campus MZR (SC-MZR): \n Chennai - Airtel (in-che) \n Montreal (ca-mon) \n Mumbai (in-mum) |
| **APIs supported** |  Kafka API   \n Admin REST API  \n REST Producer API |  Kafka API   \n Admin REST API   \n REST Producer API    |  Kafka API   \n Admin REST API   \n REST Producer API   \n  Schema Registry API  | Kafka API |
| **Deployment timeframe** | Instantaneous provisioning  | Instantaneous provisioning    | Expect provisioning to  \n  take up to 3 hours.   \n Because Enterprise has   \n dedicated resources for   \n each cluster, it requires   \n more time for   \n provisioning.  | Typical: Minutes |
| **Compliance** | GDPR   \n  Privacy Shield  | GDPR   \n  Privacy Shield   \n  ISO 27001, 27017,  \n 27018   \n  SOC 1 Type   \n  SOC 2 Type 2   \n  SOC 3   \n  PCI DSS \n ISMAP \n C5  |  GDPR   \n Privacy Shield   \n ISO 27001, 27017,  \n 27018   \n  SOC 1 Type 2   \n  SOC 2 Type 2   \n  SOC 3   \n  HIPAA ready   \n  PCI DSS  \n ISMAP \n C5 \n IRAP \n ENS \n HITRUST \n ProtectedB | New plan – pending certification |
| **Manage security and compliance**  | No  |  No |  Yes | Yes |
| **Context-based restrictions (CBR)** | No | Yes | Yes | Yes |
| **IAM token authentication only** | No | No | Yes | Yes |
| **Mirroring** | No | No | Yes | Future release |
{: caption="Plan comparison table" caption-side="bottom"}

[^tabletext1]: After 30 days of inactivity, your instance is deleted. Inactivity is defined as a zero `bytes_out` metric, even if you create a partition or produce messages.

[^tabletext2]: For more information about availability, see [single-zone location deployments](/docs/EventStreams-gen2?topic=EventStreams-gen2-sla#sla_szr).

[^tabletext4]: Supported only on clusters created after October 2019.

[^tabletext5]: If you migrate from the Lite plan to the Standard plan, allow a few minutes for the cached limit of one partition to clear. You can then use the 100-partition limit for the Standard plan.

[^tabletext6]: This value scales relative to maximum throughput. For example, 150 MB/s throughput allows up to 3000 partitions, 300 MB/s allows 6000 partitions, and 450 MB/s allows 9000 partitions. This is a hard limit for partitions on the Enterprise plan. When you reach this limit, you can no longer create topics. To adjust partitions, use the self-service option described in [Scaling Enterprise plan capacity](/docs/EventStreams-gen2?topic=EventStreams-gen2-ES_scaling_capacity#ES_how_to_scale_capacity). There is a 3000 partition limit per 2 TB storage with a maximum limit of 18,000 partitions with a 12 TB option.

[^tabletext7]: Specify maximum message retention (storage) when creating the service instance. You can scale storage independently as demands increase. The minimum usable storage depends on the configured capacity units. For more information, see [Scaling Event Streams capacity](/docs/EventStreams-gen2?topic=EventStreams-gen2-ES_scaling_capacity).

[^tabletext8]: Specify maximum throughput when creating the service instance. Throughput is the sum of bytes per second that can be sent and received in a service instance. You can scale throughput as demands increase. Each tier requires a defined minimum storage amount. For more information, see [Scaling Event Streams capacity](/docs/EventStreams-gen2?topic=EventStreams-gen2-ES_scaling_capacity).

[^tabletext9]: Specify throughput when creating the service instance. Throughput varies by workload configuration. For capacity planning, this figure represents the minimum throughput continuously available for a typical workload. Each tier requires a defined minimum storage amount. For more information, see [Scaling Event Streams capacity](/docs/EventStreams-gen2?topic=EventStreams-gen2-ES_scaling_capacity).

For more information about limits, see [limits and quotas](/docs/EventStreams-gen2?topic=EventStreams-gen2-kafka_quotas).
