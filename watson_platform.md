---

copyright:
  years: 2026
lastupdated: "2026-11-11"

keywords: platform bridge, managed bridge, event pipeline, cos bridge

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}


# Connecting {{site.data.keyword.iot_full}} to {{site.data.keyword.cloud_notm}} {{site.data.keyword.messagehub}}
{: #watson_iot}

[Gen 2]{: tag-purple}

Use the {{site.data.keyword.iot_full}} bridge to create a unidirectional link to {{site.data.keyword.cloud}} {{site.data.keyword.messagehub}} and make device events available in real time.
{: shortdesc}

When you connect {{site.data.keyword.messagehub}} to {{site.data.keyword.iot_short_notm}}, you can use {{site.data.keyword.messagehub}} as an event pipeline. The pipeline consumes your device events from {{site.data.keyword.iot_short_notm}} and makes the events available in real time to the rest of the platform.

A common pattern is to use the {{site.data.keyword.iot_short_notm}} bridge, {{site.data.keyword.messagehub}}, and the {{site.data.keyword.cos_full}} bridge to create an end-to-end pipeline for real-time and batch interactions.
