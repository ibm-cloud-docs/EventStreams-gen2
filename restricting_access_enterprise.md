---

copyright:
  years: 2025
lastupdated: "2025-12-04"

keywords: network access, service endpoints, VSIs, VPC, CSE, disruptive, context-based restrictions

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Restricting network access
{: #restrict_access}

[Gen 2]{: tag-purple}

{{site.data.keyword.messagehub_full}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see the [Beta reference](/docs/EventStreams-gen2?topic=EventStreams-gen2-gen2-beta).
{: beta}

By default, {{site.data.keyword.messagehub}} instances are configured to use the {{site.data.keyword.Bluemix_short}} public network, so they are accessible over the public internet.
{: #shortdesc} 

If required, you can use network type or context-based restrictions to restrict the network connectivity as follows:

Network type
:   {{site.data.keyword.Bluemix_notm}} offers both private and public networking. If your workload is running entirely within the {{site.data.keyword.Bluemix_notm}}, and public access to the service is not required, {{site.data.keyword.messagehub}} instances can instead be configured to be accessible only over the {{site.data.keyword.Bluemix_notm}} private network. This offers increased isolation and does not incur the egress bandwidth charges associated with public traffic. Instances can also be configured to be accessible over both the {{site.data.keyword.Bluemix_notm}} public and private networks, where your workload can use the most appropriate interface for its location. You can find further information about private networking at [Virtual routing and forwarding on IBM Cloud](/docs/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud){: external}.

Context-based restrictions (CBR)
:   You can define access rules that limit the network locations that connections are accepted from. For example, network type, IP ranges, VPC, or other services. For more information, see [What are context-based restrictions](/docs/account?topic=account-context-restrictions-whatis).{: external}

:   Auditing events for context-based restrictions are published under context-based restrictions {{site.data.keyword.at_full_notm}} events [Context-based restrictions rules events](/docs/activity-tracker?topic=activity-tracker-events_context_based#restriction_rules_events){: external}. Activity Tracker events related to CBR enforcement are unavailable on private endpoints.

## Configuring the network type
{: #configuring_network_type}

You can configure Enterprise service instances to be available on the {{site.data.keyword.Bluemix_notm}} private, public, or private and public networks. The following information describes how to select and update the required networking type. Public networking is selected by default.

### Prerequisites
{: #prereqs_restrict_access}

Ensure that you complete the following tasks:

- Create your service instance by using the Enterprise plan in one of the supported multi-zone locations. For more information, see [Choosing your plan](/docs/EventStreams?topic=EventStreams-plan_choose). 

- Enable [Virtual Route Forwarding (VRF)](/docs/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud){: external} for your {{site.data.keyword.Bluemix_notm}} account.
- Enable service endpoints connectivity by running the following command:

    ```bash
    ibmcloud account update --service-endpoint-enable true
    ```
    {: codeblock}

    To check whether prerequisites are completed, run the following command, and then check whether the following two properties are true:

    ```bash
    ibmcloud account show

    VRF Enabled:                        true
    Service Endpoint Enabled:           true
    ```
    {: codeblock}

### Selecting a network configuration during provisioning
{: #enable_endpoints}

You have a number of options for selecting the network configuration of your Enterprise cluster.

1. Use the {{site.data.keyword.Bluemix_notm}} public network. Endpoints are accessible on the public internet. This is the default.

2. Use the {{site.data.keyword.Bluemix_notm}} private network. Endpoints are not visible on the public internet.

3. Use the {{site.data.keyword.Bluemix_notm}} public and private network. Endpoints are visible on both the public internet and internally within the {{site.data.keyword.Bluemix_notm}}.

You can make this selection at provision time through the {{site.data.keyword.messagehub}} catalog provisioning page. Use the **Service Endpoints** menu drop- down to select either **Public** (default), **Private** or **Public and Private**.

Alternatively, if you want to use the CLI to provision an {{site.data.keyword.messagehub}} service, use the following commands:

* To enable public endpoints (the default):

    ```bash
    ibmcloud resource service-instance-create <instance-name> messagehub ibm.message.hub.enterprise.3nodes.2tb <region> --service-endpoints public
    ```
    {: codeblock}

* To enable private only endpoints:

    ```bash
    ibmcloud resource service-instance-create <instance-name> messagehub ibm.message.hub.enterprise.3nodes.2tb <region> --service-endpoints private
    ```
    {: codeblock}

* To enable both private and public endpoints:

    ```bash
    ibmcloud resource service-instance-create <instance-name> messagehub ibm.message.hub.enterprise.3nodes.2tb <region> --service-endpoints public-and-private
    ```
    {: codeblock}

### Updating the network configuration
{: #update_endpoints}

You can also switch the endpoints that your Enterprise cluster uses after provisioning. To do this, use the following CLI commands:

To migrate directly from public or private to public-and-private endpoints:

```bash
ibmcloud resource service-instance-update <instance-name> --service-endpoints public-and-private
```
{: codeblock}

To migrate from public-and-private to public endpoints:

```bash
ibmcloud resource service-instance-update <instance-name> --service-endpoints public
```
{: codeblock}

To migrate from public-and-private to private endpoints:

```bash
ibmcloud resource service-instance-update <instance-name> --service-endpoints private
```
{: codeblock}

Switching directly from public to private endpoints or from private to public endpoints is **not supported**. The switch disables all public endpoints and your applications loses access to the cluster. To avoid this, first enable both public and private endpoints, then reconfigure applications to use private endpoints, and finally switch to private only endpoints.
{: important}

For example, to migrate from public to private endpoints:

First, enable both public and private endpoints:

```bash
ibmcloud resource service-instance-update <instance-name> --service-endpoints public-and-private
```
{: codeblock}

Next, create a new credential that contains private endpoints and a new API key, as follows:

```bash
ibmcloud resource service-key-create <private-key-name> <role> --instance-name <instance-name> --service-endpoint private
```
{: codeblock}

Then, update any applications that use the instance with the new service key and endpoint details.

Finally, after applications are migrated to the private endpoints, run the following command to turn off the public endpoints:

```bash
ibmcloud resource service-instance-update <instance-name> --service-endpoints private
```
{: codeblock}

### Specifying an IP allowlist (deprecated)
{: #specify_allowlist}

This feature is now deprecated, instead use context-based restrictions [Configuring CBR](#configuring_cbr).
{: deprecated}

When you provision an instance of the Enterprise plan, select private endpoints and want to further restrict access to only known VSIs with specific VPCs, you can add an IP allowlist by using the CLI and appending as follows:

```bash
ibmcloud resource service-instance-create <instance-name> <plan-name> <region> --service-endpoints private -p '{"private_ip_allowlist":["CIDR1","CIDR2"]}' "
```
{: codeblock}

where CIDR1, CIDR2 are IP addresses of the form a.b.c.d/e. The array of CIDRs can have any number of elements.

Alternatively, to update the IP allowlist for an existing service instance, complete the following steps:

1. Obtain the original IP allowlist applied on the instance:

    ```bash
    $ibmcloud es init -i <instance-name>
    API Endpoint:		https://mh-cktmqpdbvkfczhmn.us-south.containers.appdomain.cloud
    Service endpoints:	public-and-private
    Private IP allowlist:	"10.243.0.8/32","10.243.128.8/32","10.243.64.4/32"
    Storage size:		4096 GB
    Throughput:		300 MB/s
    OK
    ```

2. Add CIDRs into or delete CIDRs from the `Private IP allowlist`.
3. Run the following command to update the service instance with a new list:

    ```bash
    ibmcloud resource service-instance-update <instance-name> --service-endpoints private -p '{"private_ip_allowlist":["CIDR1","CIDR2"]}'
    ```
    {: codeblock}

    where CIDR1, CIDR2 are IP addresses of the form a.b.c.d/e

If the private endpoint is enabled by using the CLI, the next time you update the private IP allowlist, you can omit  `--service-endpoints private`.
{: note}

Switching IP allowlists disable any allowed IP address not in the new list. Applications accessing the cluster from those addresses lose access to the cluster.

### Retrieving endpoint information
{: #retrieve_endpoint}

The endpoint information for your service instance is specific to the network type. Service instances with both public and private networking selected have an endpoint for each.

You can retrieve the endpoint information for the required network type by using the `--service-endpoint` option of the `service-key-create` CLI command as follows:

```sh
ibmcloud resource service-key-create <private-key-name> <role> --instance-name <instance-name> --service-endpoint private
```
{: codeblock}

### Accessing the IBM {{site.data.keyword.messagehub}} console
{: #access_console}

The console continues to be accessible if public networking is available for the instance, including both public and public-and-private.

The console will not be accessiable if private only network is selected and context-based restriction is setup due to technical limitation. 
CLI is recommended to perform any administrative operations. eg. `ibmcloud es topic-create [--name] TOPIC_NAME [--partitions PARTITIONS] [--config KEY=VALUE[;KEY=VALUE]* ]*`

## Configuring context-based restrictions
{: #configuring_cbr}

You can define rules that limit the network locations that connections are accepted from. For example, by network type, IP ranges, VPC, or from other services. For more information, see [What are context-based restrictions?](/docs/account?topic=account-context-restrictions-whatis){: external}

Context-based restrictions can be defined for both Standard and Enterprise plan {{site.data.keyword.messagehub}} instances.
When context-based restrictions rules are defined, the following rules apply:

* Admin REST API, REST Producer API, Schema Registry API, and Kafka client calls are under the scope of context-based restrictions rules that are created against an {{site.data.keyword.messagehub}} instance.

* The Administration functions for the service instance itself (for example the {{site.data.keyword.Bluemix_notm}} CLI `service-instance-create`, `service-instance-delete` or `service-instance-update` commands, or equivalent) are not under the scope of the context-based restrictions rules that are created against an {{site.data.keyword.messagehub}} instance.

### Managing context-based restrictions settings
{: #managing_cbr}

Creating context-based restrictions rules is a two-step process:

1. Create a network zone with list of allowed IP addresses, allowed VPCs, or reference a service. For more information, see [Creating network zones](/docs/account?topic=account-context-restrictions-create&interface=ui#network-zones-create){: external}.

2. Create rules that specify one or more network zones against the {{site.data.keyword.messagehub}} resource. For more information about rule creation, see [Creating rules](/docs/account?topic=account-context-restrictions-create&interface=ui#context-restrictions-create-rules){: external}.

Next, note the following considerations:

* You must be the account owner or have an access policy with the administrator role on all account management services to restrict access.
* After creating or updating a zone or a rule, it can take a few minutes (up to 10 minutes) for the change to take effect.
* After context-based restrictions are applied, they are only enforced for clients that authenticate after the rule has been created. Existing clients should reauthenticate for restriction rules to be enforced.
{: important}

### Supporting connections between services (service-to-service) with context-based restrictions
{: #services_cbr}

If an {{site.data.keyword.messagehub}} service instance is configured to use [customer-managed encryption](/docs/EventStreams?topic=EventStreams-managing_encryption), the service must also be granted the ability to connect to the selected IBM Key Management Services.

The administrator of the account can set this up as follows:
1. Add a service reference for the {{site.data.keyword.messagehub}} service to the required network zone.
2. Ensure that access from this zone is permitted by way of the context-based restrictions rules applicable to other Cloud services.

For more information about service reference creation, see [Service references](/docs/account?topic=account-context-restrictions-whatis#service-attribute){: external}.

### Coexistence of context-based restrictions rules and private IP allowlists
{: #cbr_coexistence}

Context-based restrictions rules now supersede the use of private IP allowlists as the recommended approach for implementing allowlists against an {{site.data.keyword.messagehub}} instance. The use of private IP allowlists continues to be supported but is now deprecated.

If both context-based restrictions rules and IP allowlists are defined against the same {{site.data.keyword.messagehub}} instance, the IP allowlist is ignored because the context-based restrictions rules override any previous private IP allowlist.

### Migrating from private IP allowlists to context-based restrictions
{: #cbr_migrating}

The customer is responsible for migration. You can create IP allowlist definitions again as context-based restrictions network zones and apply them to the service instance by creating a context-based restrictions rule. You can then delete the previous private IP allowlist.
