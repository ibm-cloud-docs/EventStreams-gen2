---

copyright:
  years: 2022, 2025
lastupdated: "2025-09-15" 

keywords: release notes

subcollection: EventStreams

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}


# Release notes for {{site.data.keyword.messagehub}}
{: #event-streams-relnotes}

Use these release notes to learn about the latest updates to {{site.data.keyword.messagehub_full}} that are grouped by month and year. Release notes are available for a minimum of three years.
{: shortdesc}

## March 2025
{: #EventStreams-mar2025}
{: release-note}

Apache Kafka upgrade
:   Upgrade to Apache Kafka version 3.8 complete. All deployments {{site.data.keyword.messagehub}} are now on Kafka version 3.8.

IBM Cloud® Event Streams for IBM Cloud Satellite® is deprecated
:   {{site.data.keyword.messagehub}} for Satellite is deprecated. As of March 10 2025, all documentation relating to {{site.data.keyword.messagehub}} for Satellite has been removed, as well as the ability to select {{site.data.keyword.messagehub}} {{site.data.keyword.satelliteshort}} plan in the Cloud console.

## January 2025
{: #EventStreams-jan2025}
{: release-note}

Apache Kafka upgrade
:   Upgrade to Apache Kafka version 3.8. New deployments of {{site.data.keyword.messagehub}} are now on Kafka version 3.8.

## October 2024
{: #EventStreams-oct2024}
{: release-note}

Terraform Updates
:  More user capabilities are supported via Terraform including obtaining the kafka version. To find out more, check out the [Event Streams Terraform Module](https://github.com/terraform-ibm-modules/terraform-ibm-event-streams) as part of [Event Streams Terraform reference](/docs/EventStreams?topic=EventStreams-eventstreams-terraform).

Mirror Maker Enhancements
: Migrating between two instances of event streams is easier and faster with renaming topics and consumer groups now supported with Event Streams mirror maker. See [select topics and consumer groups](/docs/EventStreams?topic=EventStreams-mirroring_setup#step5_selecttopics).

## September 2024
{: #EventStreams-sep2024}
{: release-note}

Schema Registry commands in CLI plugin v2.6.0
:  Manage schema related tasks using the event streams CLI plugin v2.6.0. To find out more, see [Event Streams CLI reference](/docs/cli?topic=cli-cli_reference).

Reserved disk space percentage per topic metric available
:  You can now monitor the reserved disk space percentage per topic with the {{site.data.keyword.messagehub}} enterprise plan. To find out more, see [Reserved disk space percentage per topic](/docs//EventStreams?topic=EventStreams-metrics#ibm_eventstreams_instance_reserved_disk_space_per_topic_percent). 

## July 2024
{: #EventStreams-jul2024}
{: release-note}

Schema Registry request rate metric available
:  You can now monitor the serialization and deserialization rates on your schema registry with the {{site.data.keyword.messagehub}} enterprise plan. To find out more, see [Schema registry request rate](/docs//EventStreams?topic=EventStreams-metrics#ibm_eventstreams_instance_schema_registry_serdes_requests_per_sec). 


## March 2024
{: #EventStreams-mar2024}
{: release-note}

Apache Kafka upgrade
:   Upgrade to Apache Kafka version 3.6.

Instance utilization metric available
:    You can monitor CPU usage of an {{site.data.keyword.messagehub}} instance to plan capacity or be alerted in a timely manner if your service starts to consume all CPU capacity. To find out more, see [Instance utilization](/docs/EventStreams?topic=EventStreams-metrics#ibm_eventstreams_instance_utilization). 

Health Metric now available via CLI and API
:   A service administrator can quickly assess the health and availability status of their Kafka cluster with a single metric using the CLI and API. To find out more, see [get status](/apidocs/event-streams/adminrest#getstatus). 

## February 2024
{: #EventStreams-feb2024}
{: release-note}

Mirroring for single-zone regions 
:   {{site.data.keyword.messagehub}} now supports mirroring for single-zone regions. A Kafka user can now mirror messages from one cluster in a single-zone region to another. To find out more, see [Enabling mirroring](/docs/EventStreams?topic=EventStreams-mirroring_setup).

## January 2024
{: #EventStreams-jan2024}
{: release-note}

Deprecation of {{site.data.keyword.sqlquery_short}}
: {{site.data.keyword.messagehub}} uses {{site.data.keyword.sqlquery_full}} for streaming. {{site.data.keyword.sqlquery_short}} is deprecated. As of 18 February 2024 you can't create new instances, and access to free instances will be removed. Existing Standard plan instances are supported until 18 January 2025. Any instances that still exist on that date will be deleted.


## November 2023
{: #EventStreams-nov2023}
{: release-note}

Private networking in SZR
:   This capability means that any data that you publish or consume from the {{site.data.keyword.messagehub}} service is over the private network and not public interfaces. Private endpoints are now supported across all regions (MZR and SZR) in the Enterprise plan. 


## October 2023
{: #EventStreams-oct2023}
{: release-note}

{{site.data.keyword.messagehub}} is available in Madrid
:   {{site.data.keyword.messagehub}} is now supported in a new region: Madrid (eu-es).

## September 2023
{: #EventStreams-sep2023}
{: release-note}

Self-service mirroring enablement 
:   You can enable mirroring by running a **service-instance-update** CLI command against your target cluster, instead of raising a support ticket. To find out more, see [Enabling mirroring](/docs/EventStreams?topic=EventStreams-mirroring_setup).

## August 2023
{: #EventStreams-aug2023}
{: release-note}

UI Enhancements  
:   Topics Page Upgrade - Users interacting the UI now have improved topic overview page with core components redesigned including topic details and streamlanding wizard
:   Topic Editing - Users can configure their topics directly from the GUI
:   Topics Pagination - Topic overview page now paginates topics, providing users with a better layout and easier navigation of the UI.

## June 2023
{: #EventStreams-jun2023}
{: release-note}

Additional topic partition metrics
:   The message rate per partition is now available. For more information, see [Message rate per partition metric](/docs/EventStreams?topic=EventStreams-metrics#ibm_eventstreams_instance_message_rate_per_partition). 

Usage quotas per user available
:   Kafka administrators can now use metrics to identify users who are consuming unexpectedly large amounts of bandwidth and then apply a quota. Kafka users with a quota have visibility of how much capacity they have left before throttling starts. For more information, see [IAM ID bytes](/docs/EventStreams?topic=EventStreams-metrics#ibm_eventstreams_iam_id_bytes_in_per_second).
 
## May 2023
{: #EventStreams-may2023}
{: release-note}

Schema registry endpoints
:   An {{site.data.keyword.messagehub}} user can use a larger subset of the Confluent Schema Registry REST API to manage their schema registry. 

Lenses.io integration
: {{site.data.keyword.messagehub}} now integrates with Lenses, a third party DataOps platform for Apache Kafka. To find out how to install Lenses with {{site.data.keyword.messagehub}}, see [Getting started with {{site.data.keyword.messagehub}}](https://docs.lenses.io/5.2/installation/getting-started/managed/ibm/). 

Apache Kafka upgrade
:   Upgrade to Apache Kafka version 3.3.

## April 2023
{: #EventStreams-apr2023}
{: release-note}

{{site.data.keyword.messagehub}} Enterprise plan is certified IRAP Protected
:   The Information Security (InfoSec) Registered Assessors Program (IRAP) outlines a framework for assessing the implementation and effectiveness of {{site.data.keyword.messagehub}} and IBM Cloud's security controls against the Australian government's Information Security Manual (ISM). The {{site.data.keyword.messagehub}} Enterprise plan has been assessed as compliant with IRAP Protected. For reports, documentation and further details on IBM Cloud IRAP Protected Offerings, see [IBM Cloud compliance: IRAP (Australia)](https://www.ibm.com/cloud/compliance/irap-australia?_gl=1*nwr26q*_ga*OTk4NjUzOTE2LjE2OTI4OTA4MDA.*_ga_FYECCCS21D*MTY5Mjg5MDgwMC4xLjEuMTY5Mjg5MjcyNC4wLjAuMA).

## March 2023
{: #EventStreams-mar2023}
{: release-note}

OAUTHBEARER users app to service authentication
:   {{site.data.keyword.messagehub}} users can configure their Kafka client with more secure and ephemeral [SASL OAUTHBEARER](/docs/EventStreams?topic=EventStreams-kafka_using#kafka_api_client), in addition to API keys.

ISMAP and C5 Certification
:   {{site.data.keyword.messagehub}} Standard and Enteprise plans were certified by the [ISMAP (Information System Security Management and Assessment Program)](/docs/EventStreams?topic=EventStreams-compliance#ismap), the Japanese government program for assessing the security of public cloud services. {{site.data.keyword.messagehub}} Standard and Enterprise plans were audited and certified to [C5 (Cloud Computing Compliance Controls Catalogue)](/docs/EventStreams?topic=EventStreams-compliance#c5) and meet the requirements for cloud security and the adoption of public cloud solutions by German government agencies.

Kafka version by using the CLI and console
:   You can now check the version of Kafka by using the command line and console.

## January 2023
{: #EventStreams-jan2023}
{: release-note}

Quota management by using the REST API
:   You can enforce production and consumption rate limits by settings quotas using the Admin REST API. For more information, see [List each entity's quota information](/apidocs/event-streams/adminrest#list-quotas) to prevent network saturation or monopolizing broker resources.

## October 2022
{: #EventStreams-oct2022}
{: release-note}

Context-based restrictions
:   You can use network type or context-based restrictions to restrict the network connectivity on the Enterprise plan. For more information, see [Restricting Network Access](/docs/EventStreams?topic=EventStreams-restrict_access).

Apache Kafka upgrade
:   Upgrade to Apache Kafka version 3.1.

## July 2022
{: #EventStreams-jul2022}
{: release-note}

Enhanced support for matching schemas
:   Enhanced support for matching schemas when you use Confluent SerDes with the schema registry.

## May 2022
{: #EventStreams-may2022}
{: release-note}

Schema registry support on the Satellite plan
:   The new [Satellite plan](/docs/EventStreams?topic=EventStreams-satellite_about) now supports the [Schema registry API](/docs/EventStreams?topic=EventStreams-satellite-provisioning#satellite-enable-schema-registry).

## April 2022
{: #EventStreams-apr2022}
{: release-note}

Satellite plan
:   With the new [Satellite plan](/docs/EventStreams?topic=EventStreams-satellite_about), you can create a hybrid environment that brings the scalability and on-demand flexibility of public cloud services to the applications and data that run in your secure private cloud.

More topic and consumer group metrics
:   More topic and consumer group metrics are available, see [Monitoring Event Streams metrics by using IBM Cloud Monitoring](/docs/EventStreams?topic=EventStreams-metrics).

## December 2021
{: #EventStreams-dec2021}
{: release-note}

Apache Kafka upgrade
:   Upgrade to Apache Kafka version 2.8.

## October 2021
{: #EventStreams-oct2021}
{: release-note}

IAM
:  [IAM IP address access restrictions](/docs/EventStreams?topic=EventStreams-restricting_access_iam).

## August 2021
{: #EventStreams-aug2021}
{: release-note}

New region support
:   Sao Paulo region support.

## June 2021
{: #EventStreams-jun2021}
{: release-note}

Streaming to {{site.data.keyword.cos_full}}  by using {{site.data.keyword.sqlquery_full}} 
:   [Event Streaming general availability](/docs/EventStreams?topic=EventStreams-streaming_cos_sql). From the {{site.data.keyword.messagehub}} UI, select and link topics to {{site.data.keyword.cos_full}}  buckets, and stream data automatically and securely by using the fully managed {{site.data.keyword.sqlquery_short}} service. 

## May 2021
{: #EventStreams-may2021}
{: release-note}

New region support
:   Toronto region support.

## April 2021
{: #EventStreams-apr2021}
{: release-note}

Security and compliance center support
:  [Security and compliance center support](/docs/EventStreams?topic=EventStreams-manage-security-compliance) helps you manage security and compliance for your organization.

## March 2021
{: #EventStreams-mar2021}
{: release-note}

New region support
:   Osaka region support.

## January 2021
{: #EventStreams-jan2021}
{: release-note}

Apache Kafka upgrade
:   Upgrade to Apache Kafka version 2.6 for all plans.

## September 2020
{: #EventStreams-sep2020}
{: release-note}

Scaling support for Enterprise plan
:   [Scale your Enterprise Plan clusters](/docs/EventStreams?topic=EventStreams-plan_choose#plan_enterprise) to allow for greater throughput and higher retention.

## August 2020
{: #EventStreams-aug2020}
{: release-note}

Mirroring support for Enterprise plan
:   [Mirroring](/docs/EventStreams?topic=EventStreams-mirroring) enables messages in one service instance to be copied continually to a second instance, allowing disaster recovery scenarios to be implemented easily.

Terraform support
:   Create your {{site.data.keyword.messagehub}} instances and subsequent configuration by using Terraform.

## June 2020
{: #EventStreams-jun2020}
{: release-note}

Schema registry support for Enterprise plan
:   Add structure to your messages by using the {{site.data.keyword.messagehub}} [Schema registry](/docs/EventStreams?topic=EventStreams-ES_schema_registry).

## March 2020
{: #EventStreams-mar2020}
{: release-note}

Customer metrics support
:   Get visibility of metrics for your Enterprise {{site.data.keyword.messagehub}} instance through a new customer metrics dashboard.

## November 2019
{: #EventStreams-nov2019}
{: release-note}

Apache Kafka upgrade
:   Upgrade to Apache Kafka version 2.3.

Bring Your own key for Enterprise plan
:   Allows users to secure their data at rest with their own encryption key.

IP Allowlisting support
:   Allows users to stipulate source IPs from which users must connect to {{site.data.keyword.messagehub}}.

## September 2019
{: #EventStreams-sep2019}
{: release-note}

New Lite plan
:   Release of the [Lite plan](/docs/EventStreams?topic=EventStreams-plan_choose#plan_lite) in US-South allowing users to provision a minimal {{site.data.keyword.messagehub}} instance for free.

## 1 July 2019
{: #EventStreams-july2019}
{: release-note}

Updates to Enterprise plan
:   Apache Kafka version upgrade.
:   Support for Cloud Service Endpoints.

Kafka version upgrade
:   {{site.data.keyword.messagehub}} Enterprise supports version 2.2 of Apache Kafka to align with our recently released and upgraded Standard plan.
:   This update for {{site.data.keyword.messagehub}} is nondisruptive and was tested with our supported Kafka client list.
:   If your Kafka client is not on this list, then while we expect the upgrade to be nondisruptive, these clients were not tested and we cannot offer any support
statement for these clients. If this is a concern, do any extra testing that you require.
:   Note: The Standard plan is already updated to Apache Kafka version 2.2 and can be used for extra testing.

Support for Cloud service endpoints
:   {{site.data.keyword.messagehub}} supports Cloud service endpoints.
:   This capability means that any data that you publish or consume from the {{site.data.keyword.messagehub}} service is over the private network and not public interfaces.

## 14 May 2019
{: #EventStreams-may2019}
{: release-note}

New Standard plan
:   Support for Apache Kafka version 2.2.
:   Support for fine-grained authorization to topic level.
:   Support for availability zones to further increase the resiliency of the service.
:   A brand new {{site.data.keyword.IBM}} Design Thinking-led developer and user experience.
:   Available in the US-South region.
