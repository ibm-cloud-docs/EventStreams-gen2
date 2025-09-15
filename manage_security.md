---
copyright:
  years: 2021, 2023
lastupdated: "2023-07-24"

keywords: security, compliance, resource configuration

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Managing security and compliance with {{site.data.keyword.messagehub}}
{: #manage-security-compliance}

{{site.data.keyword.messagehub}} is integrated with the {{site.data.keyword.compliance_short}} to help you manage security and compliance for your organization.
{: shortdesc}

With the {{site.data.keyword.compliance_short}}, you can do the following:

- Monitor for controls and goals that pertain to {{site.data.keyword.messagehub}}.
- Define rules for {{site.data.keyword.messagehub}} that can help to standardize resource configuration.

{{site.data.keyword.compliance_short}} is only applicable to the Enterprise plan.
{: important}

## Monitoring security and compliance posture with {{site.data.keyword.messagehub}}
{: #monitor-eventstreams}

As a security or compliance focal, use the *Event Streams* goals to help ensure that your organization is adhering to the external and internal standards for your industry. By using the {{site.data.keyword.compliance_short}} to validate the resource configurations in your account against a [profile](#x2034950){: term}, identify potential issues as they arise.

All of the goals for {{site.data.keyword.messagehub}} are added to the {{site.data.keyword.cloud_notm}} Control Library but can also be mapped to other profiles.
{: note}

To start monitoring your resources, check out [Getting started with {{site.data.keyword.compliance_short}}](/docs/security-compliance?topic=security-compliance-getting-started).

### Available goals for Event Streams
{: #eventstreams-available-goals}

* Check whether {{site.data.keyword.messagehub}} is accessible through public endpoints.
* Check whether {{site.data.keyword.messagehub}} is accessible only by using private endpoints.
* Check whether {{site.data.keyword.messagehub}} network access is restricted to a specific IP range.
* Check whether {{site.data.keyword.messagehub}} is enabled with customer-managed encryption and Keep Your Own Key (KYOK).

## Governing Event Streams resource configuration
{: #govern-eventstreams}

As a security or compliance focal, use the {{site.data.keyword.compliance_short}} to define configuration rules for the instances of {{site.data.keyword.messagehub}} that you create.

[Config rules](#x3084914){: term} are used to enforce the configuration standards that you want to implement across your accounts. To learn more about the data that you can use to create a rule for {{site.data.keyword.messagehub}}, review the following table.

| Resource type | Property | Operator | Value | Description |
|---------------|----------|---------------|-------|-------------|
| Instance | public_network_enabled | is_true  is_false | - | Indicates whether access to a {{site.data.keyword.messagehub}} instance is allowed through a public network. |
| Instance | private_network_enabled | is_true  is_false | - | Indicates whether access to a {{site.data.keyword.messagehub}} instance is allowed through a private network. |
| Instance | private_access_allowlist | ips_in_range | - | If private networking is enabled, this property indicates whether access to a {{site.data.keyword.messagehub}} instance should be restricted to a specific range of private IP CIDR formatted subnets. |
{: caption="Rule properties for {{site.data.keyword.messagehub}}" caption-side="bottom"}

To learn more about defining config rules, check out [Defining custom rules](/docs/security-compliance?topic=security-compliance-rules-define).
