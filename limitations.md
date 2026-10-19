---

copyright:
  years: 2026
lastupdated: "2026-10-19"

keywords: event streams gen 2 differences, event streams gen 2 limitations, event streams enterprise gen 2, gen 1 vs gen 2 kafka

subcollection: EventStreams-gen2

content-type: reference

---

{{site.data.keyword.attribute-definition-list}}

# Architectural differences and known limitations
{: #eventstreams-gen1-vs-gen2-differences-limitations}

[Gen 2]{: tag-purple}

{{site.data.keyword.messagehub}} Generation 2 (VPC) introduces permanent architectural changes and has temporary feature gaps compared to Generation 1 (Classic). Use this topic to understand the differences before you design, provision, or migrate workloads.
{: shortdesc}

For plan selection guidance, see [Choosing your plan](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose). This topic is organized into two sections:

- [Architectural differences](#gen1-vs-gen2-architectural-differences): Permanent platform decisions and foundational shifts that define how Gen 2 operates.
- [Known limitations](#gen1-vs-gen2-known-limitations): Temporary feature gaps where capabilities available in Gen 1 are planned for Gen 2 in upcoming releases.

## Architectural differences
{: #gen1-vs-gen2-architectural-differences}

Architectural differences represent permanent design changes in Gen 2 to align with the next generation of the service, built on software-defined {{site.data.keyword.cloud}} VPC infrastructure, zero-trust security architecture, and predictable compute performance.

| Feature / Area | Gen 1 (Classic) | Gen 2 (VPC) | Why it changed (Architecture details) |
| --- | --- | --- | --- |
| Network endpoints | Public and private endpoints (Classic Service Endpoints). | Private endpoints only (VPC VPE). | Private endpoints only (VPC VPE).	Gen 2 is private-only. All client connections route through Virtual Private Endpoints (VPE), eliminating support for public internet access and aligning with a zero-trust networking model. |
| Service provisioning and API experience | Custom {{site.data.keyword.messagehub}} provisioning broker. | {{site.data.keyword.cloud_notm}} Resource Controller based. | Standardizes lifecycle management onto the core {{site.data.keyword.cloud_notm}} Resource Controller and Resource Manager APIs, unifying resource tagging, IAM governance, and deployment orchestration. |
| Kafka version | Kafka 3.8. | Kafka 4.1. | Gen 2 standardizes on a recent Kafka release to leverage the latest protocol improvements, consumer group enhancements, and security features. |
| Provisioning model | Enterprise plan provisioning could take up to 3 hours. | Typical provisioning in minutes. | Gen 2 uses VPC compute capacity and a streamlined provisioning pipeline, reducing cluster bring-up time. |
| TLS / SSL certificates | Certificates signed by the internal {{site.data.keyword.IBM}} Certificate Authority. | Certificates signed by a public Certificate Authority (Let's Encrypt). | Standardizes on industry-standard CA signatures, simplifying automated certificate rotation and client trust store management. |
| Available plans (multi-tenancy) | Lite, Standard, and Enterprise plans — including multi-tenant shared clusters for lower-cost workloads. | Enterprise Gen 2 plan only (single-tenant). | Gen 2 is designed to take advantage of the enhanced isolation available on VPC infrastructure, including dedicated compute, networking, and storage resources for each deployment. These isolation levels are not feasible within the existing multi-tenant shared-cluster model. As a result, multi-tenant plans (Lite and Standard) remain Gen 1 (Classic) only plans. |
{: caption="Architectural differences between {{site.data.keyword.messagehub}} Gen 1 and Gen 2" caption-side="bottom"}

## Known limitations
{: #gen1-vs-gen2-known-limitations}

Known limitations represent capabilities available in Gen 1 that are not yet available in Gen 2 as the service progresses toward full feature parity.

| Capability | Gen 2 status | Notes and workarounds |
| --- | --- | --- |
| REST Producer API — HTTP-based message production without a native Kafka client. | Future release | A new implementation based on Confluent REST Proxy is in development, providing REST-based message producing and schema integration beyond current Classic capabilities. Targeted for delivery in early 2027. **Workaround:** Use the native Kafka producer API with a Kafka client library. |
| Admin REST API — HTTP-based topic and resource administration. | Future release | REST-based cluster administration is planned in a future release. **Workaround:** Use the Kafka AdminClient API directly or manage topics through the {{site.data.keyword.cloud_notm}} console and `ibmcloud` CLI. |
| Managed Schema Registry — Centralized Avro/JSON/Protobuf schema management integrated with the service. | Future release | Schema Registry support is planned in a future release. **Workaround:** Deploy a self-managed schema registry within your VPC or use client-side schema validation. |
| Mirroring — Managed topic replication between {{site.data.keyword.messagehub}} instances or regions. | Future release | Cross-instance and cross-region mirroring is planned in a future release. **Workaround:** Deploy and manage MirrorMaker 2 within your VPC to replicate topics between clusters. |
| Capacity scaling (throughput/capacity units) — Self-service scaling of throughput and storage after provisioning. | Future release | Dynamic capacity scaling is planned in a future release. **Workaround:** Shard your workload across multiple instances until you have the desired throughput. |
| Compliance certifications — Full certification portfolio matching Gen 1 Enterprise (HIPAA, IRAP, ENS, HITRUST, ProtectedB, and others). | In progress | Compliance certifications are in progress as Gen 2 matures. See [Choosing your plan](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose) for the current certification status. |
{: caption="Known temporary limitations for {{site.data.keyword.messagehub}} Gen 2" caption-side="bottom"}

## Detailed domain breakdown
{: #gen1-vs-gen2-domain-details}

The following sections provide additional detail for each capability area summarized in the tables.

### Networking and connectivity
{: #networking-connectivity-differences}

- **Private-only networking:** Gen 2 instances do not support public internet routing. All Kafka client traffic must originate from within an authorized {{site.data.keyword.cloud_notm}} VPC or over Direct Link or Transit Gateway connected to the instance's VPE. Applications running outside a VPC must connect through an appropriately configured transit path.
- **No Classic Service Endpoints:** Clients that connected to Gen 1 using Classic Service Endpoints (CSE) must migrate to VPE-based connectivity before moving to Gen 2.

### APIs and client connectivity
{: #api-client-differences}

- **Kafka native API:** The native Kafka producer, consumer, and AdminClient APIs are fully supported on Gen 2 and are the primary integration path.
- **REST Producer API:** The HTTP-based REST Producer API available on Gen 1 Classic and Enterprise plans is not yet available on Gen 2. A new implementation based on Confluent REST Proxy is in development, targeted for delivery in early 2027. Workloads that rely exclusively on the REST Producer API should plan their migration timeline accordingly.
- **Admin REST API:** HTTP-based administration is not yet available. Use the Kafka AdminClient API or {{site.data.keyword.cloud_notm}} console for topic and resource management in the interim.

### Schema Registry
{: #schema-registry-differences}

- **Managed Schema Registry:** Available on Gen 1 Enterprise plan. Not yet available on Gen 2. Teams requiring schema management should deploy a self-managed registry within their VPC until the managed offering is released.

### Mirroring and replication
{: #mirroring-replication-differences}

- **Managed mirroring:** Available on the Gen 1 Enterprise plan for cross-region topic replication. This capability is not currently available on Gen 2. If you require cross-instance or cross-region replication, deploy Apache MirrorMaker 2 as a self-managed service within your VPC as an interim solution until native support becomes available on Gen 2.

### Security and compliance
{: #security-compliance-differences}

- **Customer-managed encryption (Bring Your Own Key):** Supported on Gen 2 via Key Protect. Encryption keys must be configured at initial provisioning; attaching a key to an existing instance without reprovisioning is not supported on either generation.
- **Compliance certifications:** Gen 1 Enterprise holds a broad certification portfolio. Gen 2 certifications are being pursued as the plan matures. Review the current status in [Choosing your plan](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose) before committing regulated workloads to Gen 2.
