---

copyright:
  years: 2025
lastupdated: "2025-12-16"

keywords: platform bridge, managed bridge, event pipeline, cos bridge

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}


# Watson IoT Platform bridge
{: #watson_iot}

[Gen 2]{: tag-purple}

{{site.data.keyword.messagehub_full}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see the [Beta reference](/docs/EventStreams-gen2?topic=EventStreams-gen2-gen2-beta).
{: beta}

{{site.data.keyword.iot_full}} provides a managed bridge that lets you create a unidirectional link to {{site.data.keyword.messagehub_full}}.
{: shortdesc}

Connecting {{site.data.keyword.messagehub}} to {{site.data.keyword.iot_short_notm}} means that you can use {{site.data.keyword.messagehub}} as an event pipeline to consume your device events from the {{site.data.keyword.iot_short_notm}} and to make the events available in real time to the rest of the platform.

A common pattern is to use the {{site.data.keyword.iot_short_notm}} bridge, {{site.data.keyword.messagehub}}, and the {{site.data.keyword.cos_full}} bridge to create an end-to-end pipeline, which makes real-time and batch interactions easier.
