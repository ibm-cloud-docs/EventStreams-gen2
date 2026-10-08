---
copyright:
  years: 2025, 2026
lastupdated: "2026-10-08"

keywords: gen 2

subcollection: event-streams-gen2

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}

# Release notes for {{site.data.keyword.messagehub}} Gen 2
{: #event-streams-relnotes-gen2}

[Gen 2]{: tag-purple}

Use these release notes to learn about the latest updates to {{site.data.keyword.messagehub}} Gen 2 that are grouped by month and year. Release notes are available for a minimum of three years.
{: shortdesc}

## 7 October 2026
{: #EventStreams-gen2-017oct2026}
{: release-note}

Enhanced Bring Your Own Key (BYOK) experience in the provisioning UI
 : The provisioning experience now includes an updated encryption configuration component for customer-managed encryption keys through {{site.data.keyword.keymanagementservicefull}}. This update provides a more consistent key management experience during deployment creation. Learn more about [{{site.data.keyword.keymanagementserviceshort}} integration](/docs/EventStreams-gen2?topic=EventStreams-gen2-key-protect&interface=ui) or provision a new [{{site.data.keyword.messagehub}} Gen 2 deployment](https://cloud.ibm.com/eventstreams-provisioning/6a7f4e38-f218-48ef-9dd2-df408747568e/create) with customer-managed encryption enabled.


## 06 October 2026
{: #EventStreams-gen2-06Oct2026}
{: release-note}

Context-based restrictions (CBR) support now available
:  You can now use context-based restrictions (CBR) to restrict access to {{site.data.keyword.messagehub}} resources based on network context, such as IP addresses, VPCs, and {{site.data.keyword.cloud}} services. For more information, see [Managing access with context-based restrictions](/docs/EventStreams-gen2?topic=EventStreams-gen2-cbr&interface=ui).

## 30 September 2026
{: #EventStreams-gen2-30sep2026}
{: release-note}

{{site.data.keyword.messagehub}} Gen 2 is now available in all VPC multizone regions
: You can now deploy {{site.data.keyword.messagehub}} Gen 2 in all supported {{site.data.keyword.cloud}} VPC multizone regions (MZRs). This release adds support for Toronto (ca-tor), Tokyo (jp-tok), Osaka (jp-osa), and Sao Paulo (br-sao). For more information, see [Location availability](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose#what_is_supported).

## 17 September 2026
{: #EventStreams-gen2-17sep2026}
{: release-note}

{{site.data.keyword.messagehub}} Gen 2 is now available in Dallas and London
: {{site.data.keyword.messagehub}} Gen 2 is now also available in Dallas (us-south) and London (eu-gb). For more information, see [Location availability](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose#what_is_supported).

## 10 September 2026
{: #EventStreams-gen2-10sep2026}
{: release-note}

{{site.data.keyword.messagehub}} Gen 2 is now available in Sydney and Madrid
: {{site.data.keyword.messagehub}} Gen 2 is now also available in Sydney (au-syd) and Madrid (eu-es). For more information, see [Location availability](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose#what_is_supported).

## 20 July 2026
{: #EventStreams-gen2-20jul2026}
{: release-note}

{{site.data.keyword.messagehub}} Gen 2 is now available in Washington DC
: {{site.data.keyword.messagehub}} Gen 2 is now also available in Washington DC (us-east), in addition to Chennai - Airtel (in-che), Montreal (ca-mon), Mumbai (in-mum), and Frankfurt (eu-de). For more information, see [Location availability](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose#what_is_supported).

## 6 July 2026
{: #EventStreams-gen2-06jul2026}
{: release-note}

{{site.data.keyword.messagehub}} Gen 2 is now available in Frankfurt
: {{site.data.keyword.messagehub}} Gen 2 is now also available in Frankfurt (eu-de), in addition to Chennai - Airtel (in-che), Montreal (ca-mon), and Mumbai (in-mum). For more information, see [Location availability](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose#what_is_supported).

## 01 June 2026
{: #EventStreams-gen2-01jun2026}
{: release-note}

{{site.data.keyword.messagehub}} Gen 2 now available in Mumbai
: {{site.data.keyword.messagehub}} Gen 2 is now also available in Mumbai (in-mum), in addition to Chennai - Airtel (in-che) and Montreal (ca-mon). For more information, see [Location availability](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose#what_is_supported).

## 27 March 2026
{: #EventStreams-gen2-27mar2026}
{: release-note}

Deprecation of {{site.data.keyword.hscrypto}}
: {{site.data.keyword.cloud}} is transitioning its dedicated key management offering from {{site.data.keyword.hscrypto}} to {{site.data.keyword.keymanagementservicelong}} Dedicated (Single Tenant). As part of this transition, {{site.data.keyword.hscrypto}} will reach **End of Life (EOL) on March 20, 2027**. After this date, the service will no longer be supported, and any remaining instances will be terminated.
To ensure continued service availability and support, you must migrate all existing HPCS root keys to {{site.data.keyword.keymanagementservicelong_notm}} Dedicated (Single Tenant) before the EOL date. For more information on how to migrate your encryption keys, see [Migrating from {{site.data.keyword.hscrypto}} (HPCS) to {{site.data.keyword.keymanagementserviceshort}} Dedicated (KP-ST)](/docs/EventStreams-gen2?topic=EventStreams-gen2-managing_encryption#migrating_hpcs_to_kp).

## 02 March 2026
{: #EventStreams-gen2-02mar2026}
{: release-note}

{{site.data.keyword.messagehub}} Gen 2 now available in Chennai
: {{site.data.keyword.messagehub}} Gen 2 is now also available in Chennai - Airtel (in-che), in addition to Montreal (ca-mon). For more information, see [Location availability](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose#what_is_supported).

## 26 February 2026
{: #EventStreams-gen2-26feb2026}
{: release-note}

{{site.data.keyword.messagehub}} Gen 2
: {{site.data.keyword.messagehub}} Gen 2 is now available, offering the same fully managed {{site.data.keyword.messagehub}} engine on newer VPC‑based infrastructure with improved security and networking. [Try {{site.data.keyword.messagehub}} Gen 2 now](/docs/EventStreams-gen2?topic=EventStreams-gen2-provisioning).

## December 2025
{: #EventStreams-gen2-dec2025}
{: release-note}

Beta introduction of Enterprise Gen2 plan
: Support for Apache Kafka version 4.1.
: Built on VPC and software defined networking.
:   Available in the Montreal region.
