---

copyright:
  years: 2026
lastupdated: "2026-11-04"

keywords: platform bridge, managed bridge, event pipeline, cos bridge

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}


# Connecting Watson IoT Platform to {{site.data.keyword.cloud_notm}} {{site.data.keyword.messagehub}} 
{: #watson_iot}

[Gen 2]{: tag-purple}

Use the {{site.data.keyword.iot_full}} bridge to create a unidirectional link to {{site.data.keyword.messagehub_full}} and make device events available in real time.
{: shortdesc}

Connecting {{site.data.keyword.messagehub}} to {{site.data.keyword.iot_short_notm}} means that you can use {{site.data.keyword.messagehub}} as an event pipeline to consume your device events from the {{site.data.keyword.iot_short_notm}} and to make the events available in real time to the rest of the platform.

A common pattern is to use the {{site.data.keyword.iot_short_notm}} bridge, {{site.data.keyword.messagehub}}, and the {{site.data.keyword.cos_full}} bridge to create an end-to-end pipeline, which makes real-time and batch interactions easier.
