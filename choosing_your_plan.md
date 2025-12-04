---

copyright:
  years: 2025
lastupdated: "2025-12-04"

keywords: plan, Enterprise, Standard, Lite, pricing, throughput, partitions, tenancy, compliance

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Choosing your plan
{: #plan_choose}

[Gen 2]{: tag-purple}

{{site.data.keyword.messagehub_full}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see the  [Beta reference](/docs/EventStreams-gen2?topic=EventStreams-gen2-gen2-beta).
{: beta}

{{site.data.keyword.messagehub}} is available as Lite plan, Standard plan, and Enterprise plan depending on your requirements.
{: shortdesc}

For information about {{site.data.keyword.messagehub}} plan pricing, see the [catalog](https://cloud.ibm.com/catalog){: external}. Search for `{{site.data.keyword.messagehub}}`, then click the {{site.data.keyword.messagehub}} tile to go to the provisioning page.

## Lite plan
{: #plan_lite}

The Lite plan is free for users who want to try out {{site.data.keyword.messagehub}} or build a proof-of-concept. Do not use the Lite plan for production use. It offers shared access to a multi-tenant {{site.data.keyword.messagehub}} cluster.

## Standard plan
{: #plan_standard}

The Standard plan is appropriate if you require event ingest and distribution capabilities but do not require any additional benefits of the Enterprise plan. The Standard plan offers shared access to a multi-tenant {{site.data.keyword.messagehub}} cluster that seamlessly autoscales as you increase the number of partitions you are using for your workload.

The architecture is highly available by default. The service is distributed across three availability zones, which means that the cluster is resilient to the failure of a single zone or any component within that zone.

## Enterprise plan
{: #plan_enterprise}

The Enterprise plan is appropriate if data isolation, performance, and increased retention are important considerations.
The Enterprise plan includes the following features:

- Exclusive access to a single-tenant {{site.data.keyword.messagehub}} service instance deployed in a highly available multi zone region (MZR).
- Option to provision a single-tenant {{site.data.keyword.messagehub}} service instance in a geographically local but single zone location [(SZR)](/docs/EventStreams?topic=EventStreams-sla#sla_szr).
- Scaling options to customize throughput, storage capacity, or both.

The architecture is highly available when you choose to deploy into a multi-zone region. The service is distributed across three availability zones, which means that the cluster is resilient to the failure of a single zone or any component within that zone.

## Enterprise Gen2 plan
{: #plan_enterprise_gen2}

The Enterprise Gen2 plan is appropriate if data isolation, performance, and increased retention are important considerations.

Built on {{site.data.keyword.IBM}}’s latest platform, using highly secure software-defined networking and ideal for cloud-native applications. Currently only available in select regions with support for private endpoints and isolated compute hosting. This environment is ideal for modern applications that demand advanced networking and secure, software-defined isolation.

The isolated compute hosting model provides a Kafka cluster per service instance which runs on single-tenant virtual machines and storage, providing hypervisor-level isolation and dedicated storage bandwidth. This model is ideal for workloads that demand consistent performance, security, and compliance.

## What is supported by the Lite, Standard, Enterprise and Enterprise Gen2  plans
{: #what_is_supported}

The following table summarizes what is supported by the plans:

|   | Lite plan  |  Standard plan |  Enterprise plan  | Enterprise Gen2 plan  |
|---|---|---|---|---|
| **Tenancy**  | Multi-tenant   | Multi-tenant  | Single-tenant | Single-tenant |
| **Availability zones**   |  3  |   3  |3    \n   (1 in single zone locations)   | 3 |
| **Availability**  |  99.99 % [^tabletext1] |  99.99 % | 99.99 %  (99.9 % in single zone locations) [^tabletext2]  | 99.99 % |
| **Kafka version on cluster**  | Kafka 3.8 | Kafka 3.8  | Kafka 3.8 | Kafka 4.1 |
| **Kafka Connect and Kafka Streams supported**  | No |  Yes | Yes  | Yes  |
| **Managed Schema Registry supported**  | No |  No |  Yes |  Future |
| **Customer-managed encryption**  | No  |  No |  Yes [^tabletext4]  | Yes |
| **Fine-grained access control**  | Yes  |  Yes |  Yes  |  Yes  |
| **Activity tracker events**  | No  |  Yes |  Yes |  Yes |
| **Monitoring Event Streams metrics by using IBM Cloud Monitoring**  | Yes  |  Yes |  Yes |  Yes |
| **Private/Public Networking** | Public   | Public |  Both | Private (VPE) |
| **Scale plan capacity** | No   | No  |  Yes | Future  |
| **Maximum number of partitions**  | 1 [^tabletext5]  | 100   |3000 - 9000 scales with throughput [^tabletext6] | 3000 |
| **Maximum retention limits**   | 100 MB for the partition   | 1 GB per partition  | 2 TB - 12 TB of scalable usable storage [^tabletext7] | ??? |
| **Maximum throughput**  | 100 KB per second per partition  |  1 MB per second per partition (20 MB per service instance) | 150 MB/s - 450 MB/s of scalable throughput [^tabletext8] | 50 MB/s Produce \n 50MB/s Consume [^tabletext9]|
| **Maximum message size**  | 1 MB  | 1 MB   | 1 MB | 1 MB |
| **Maximum number of connected clients**  | 5   | 500  | 10 000  | ???? |
| **Location (region) availability** | Dallas (us-south)  |  **Multizone location (MZR)**   \n Dallas (us-south)   \n Sao Paulo (br-sao)   \n Toronto (ca-tor)   \n Washington (us-east)   \n Frankfurt (eu-de)   \n London (eu-gb)   \n Madrid (eu-es)   \n  Osaka (jp-osa)   \n Sydney (au-syd)   \n Tokyo (jp-tok)|   **Multizone location (MZR)**  \n Dallas (us-south)   \n Sao Paulo (br-sao)   \n Toronto (ca-tor)   \n Washington (us-east)   \n Frankfurt (eu-de)   \n London (eu-gb)   \n Madrid (eu-es)   \n  Osaka (jp-osa)   \n Sydney (au-syd)   \n Tokyo (jp-tok)\n    \n  **Single zone location (SZR)**   \n Chennai (che01)  | Montreal |
| **APIs supported** |  Kafka API   \n Admin REST API  \n REST Producer API |  Kafka API   \n Admin REST API   \n REST Producer API    |  Kafka API   \n Admin REST API   \n REST Producer API   \n  Schema Registry API  | Kafka API |
| **Deployment timeframe** | Instantaneous provisioning  | Instantaneous provisioning    | Expect provisioning to take up to 3 hours. As Enterprise has its own dedicated resources for each cluster, it requires more time for provisioning.  | Minutes |
| **Compliance** |GDPR   \n  Privacy Shield  | GDPR   \n  Privacy Shield   \n  ISO 27001, 27017, 27018, 2701    \n  SOC 1 Type     \n  SOC 2 Type 2   \n  SOC 3   \n  PCI DSS \n ISMAP \n C5  |  GDPR   \n Privacy Shield   \n ISO 27001, 27017, 27018, 2701   \n  SOC 1 Type 2   \n  SOC 2 Type 2   \n  SOC 3   \n  HIPAA ready   \n  PCI DSS  \n ISMAP \n C5 \n IRAP \n ENS \n HITRUST \n ProtectedB | TBA |
| **Manage security and compliance**  | No  |  No |  Yes | ??? |
| **IAM address restrictions** | No | Yes | Yes | Future |
| **IAM token authentication only** | No | No | Yes | ?? |
| **Mirroring** | No | No | Yes | Future |
{: caption="Plan comparison table" caption-side="bottom"}

[^tabletext1]: After 30 days of inactivity, your instance is deleted. (Inactivity is defined as a zero bytes_out metric, even though you might create a partition or produced messages.)

[^tabletext2]: For more information about availability, see [single zone location deployments](/docs/EventStreams?topic=EventStreams-sla#sla_szr).

[^tabletext4]: Only supported on clusters that were created after October 2019.

[^tabletext5]: If you migrate from the Lite to the Standard plan, allow a few minutes for the cached limit of one partition to clear. You can then take advantage of the 100 partition limit for the Standard plan.

[^tabletext6]: This value scales relative to the maximum throughput. For example, if you have a throughput of 150 MB/s the maximum partitions would be 3000, for a throughput of 300 MB/s, 6000 and for 450 MB/s, 9000. This limit is a hard limit for partitions on the Enterprise plan. If you reach this limit, you can no longer create topics. If you want to adjust the number of partitions, you can use the self-service option described in [Scaling Enterprise plan capacity](/docs/EventStreams?topic=EventStreams-ES_scaling_capacity#ES_how_to_scale_capacity){: external}. There is a 3000 partition limit per 2 TB storage with a maximum limit of 18000 partitions with a 12 TB option, which cannot be exceeded.

[^tabletext7]: Maximum message retention (storage) can be specified when the service instance is created. Storage can be later scaled independently as demands increase. The minimum usable storage available is dependent upon the number of capacity units that are configured for the service instance. For more information about capacity options, see [Scaling Event Streams capacity](/docs/EventStreams?topic=EventStreams-ES_scaling_capacity).

[^tabletext8]: Maximum throughput can be specified when the service instance is created. Throughput is expressed as the sum of the number of bytes per second that can be both sent and received in a service instance. Throughput can be later scaled as demands increase. Although throughput scaling is independent of storage, a defined minimum storage amount is required for each tier. For more information about capacity options, see [Scaling Event Streams capacity](/docs/EventStreams?topic=EventStreams-ES_scaling_capacity).

[^tabletext9]: Throughput can be specified when the service instance is created and can be later scaled as demands increase. The throughput achieved can vary according to the configuration of the workload, but for capacity planning, the figure represents the throughput which can be expected to be continually available as a minimim for a typical workload. Although throughput scaling is independent of storage, a defined minimum storage amount is required for each tier. For more information about capacity options, see [Scaling Event Streams capacity](/docs/EventStreams?topic=EventStreams-ES_scaling_capacity).

For more information about limits, see [limits and quotas](/docs/EventStreams?topic=EventStreams-gen2-kafka_quotas).
