---

copyright:
  years: 2015, 2025
lastupdated: "2025-09-15"

keywords: connections, endpoints, cli, vpc, create service key

subcollection: EventStreams

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}


# Connecting to {{site.data.keyword.messagehub}}
{: #connecting}

To connect to your {{site.data.keyword.messagehub}} instance, you need the endpoint URLs for the APIs and the credentials for authentication. Learn how to obtain these details and the connectivity options that you can use.
{: #shortdesc}


## Overview
{: #connect_enterprise}

Services that are provisioned by using the Lite, Standard, or Enterprise plans are grouped in the dashboard under the heading **Services**. 

All plans use [IAM](/docs/account?topic=account-overview){: external} for authentication. You don't need to understand IAM to get started but some knowledge is recommended if you want to secure your {{site.data.keyword.messagehub}} service. For more information, see [Managing access to your {{site.data.keyword.messagehub}} resources](/docs/EventStreams?topic=EventStreams-security). To complete the following steps and be authorized to create topics, your application or Service Key must have a Manager access role. By default, the owner of the account that contains the service instance has this role.

By default, {{site.data.keyword.messagehub}} instances are configured to use the {{site.data.keyword.cloud}} public network, so they are accessible over the public internet. If required, you can restrict this access by selecting an alternative networking type or restricting the location that connections are accepted from. For more information, see [Restricting Network Access](/docs/EventStreams?topic=EventStreams-restrict_access).

## Connection information
{: #connection_information}

To access a service instance, create a service key. A service key contains the information that is needed to access the instance, including the endpoint details for its APIs and a unique API key credential.

To create a service key by using the {{site.data.keyword.cloud_notm}} console:

1. Locate your {{site.data.keyword.messagehub}} service on the dashboard.
2. Click your service tile.
3. Click **Service Credentials**.
4. Click **New Credential**. 
5. Complete the details for your new credential. Choose a name and a role and click **Add**. A new credential appears in the credentials list.
6. Click the new credential by using **View Credentials** to reveal the details in JSON format.

To create a service key by using the {{site.data.keyword.cloud_notm}} CLI, complete the following steps.

1. Locate your service: 
    ```bash
    ibmcloud resource service-instances
    ```
    {: codeblock}

2. Create a service key: 
    ```bash
    ibmcloud resource service-key-create <key_name> <key_role> --instance-name <your_service_name>
    ```
    {: codeblock}

3. Print the service key: 
    ```bash
    ibmcloud resource service-key <key_name>
    ```
    {: codeblock}

    A single set of endpoint details are contained in each service key. For service instances configured to be connected to a single network type, either the {{site.data.keyword.cloud_notm}} Public network (the default) or the {{site.data.keyword.cloud_notm}} Private network, the service key contains the details relevant to that network type. For instances configured to support both the private and public networks, details for the public network are returned. If you want details for the private network, you must add the `--service-endpoint private` parameter the previous CLI command, as in the following example. 
    {: note}

    ```bash
    ibmcloud resource service-key-create <private-key-name> <role> --instance-name <instance-name> --service-endpoint private
    ```
    {: codeblock}

For more information, see [Network types](/docs/EventStreams?topic=EventStreams-restrict_access).

## Establishing a connection
{: #establishing_connection}

To connect a Kafka application, complete the following tasks.

* Use the `<bootstrap_endpoints>` field from the service key as the `bootstrap.servers` property of your Kafka application.
* Set the security.protocol property to SASL_SSL and the sasl.mechanism property to PLAIN.
* Use the `<user>` field from the service key as the username and the `<api_key>` field from the service key as the password. Ensure that your application parses the details. 
* For more information, see [Configuring your Kafka API client](/docs/EventStreams?topic=EventStreams-kafka_using#kafka_api_client). 

To call an HTTP API, complete the following tasks.

* Use the `<kafka_admin_url>` field of the service key as the base URL for HTTP requests. 
* Use the {{site.data.keyword.cloud_notm}} CLI `ibmcloud iam oauth-tokens` command to generate an auth token. 
    Place this token in the `Authorization` header of the HTTP request with the value formatted as `Bearer <token>`. Both API key or JWT tokens are supported.
* Further documentation is provided for each API.
    * [REST Admin API](/docs/EventStreams?topic=EventStreams-admin_api)
    * [REST Producer API](/docs/EventStreams?topic=EventStreams-rest_producer_using)
    * [Schema Registry API](/docs/EventStreams?topic=EventStreams-ES_schema_registry#schema_registry_rest_endpoints)

## Network connectivity
{: #network_connectivity}

By default, Lite, Standard, and Enterprise instances are configured to be accessible over the public internet. If you're using the Enterprise plan, you can restrict connectivity as follows:

Private networking
:   If your workload is running entirely within the {{site.data.keyword.cloud_notm}}, and public access to the service is not required, {{site.data.keyword.messagehub}} instances can instead be configured to be accessible only over the {{site.data.keyword.cloud_notm}} Private network. This configuration offers increased isolation and does not incur the egress bandwidth charges associated with public traffic. Instances can also be configured to be accessible over both the {{site.data.keyword.cloud_notm}} Public and Private networks.

Context-based restrictions
:   You can define access rules that limit the network locations that connections are accepted from according to certain characteristics. For example, network type, IP ranges, VPC, or other services.

For more information, see [Restricting network access](/docs/EventStreams?topic=EventStreams-restrict_access).

### Accessing an Enterprise instance over the private network from Classic infrastructure
{: #private_network_classic}

To access your Enterprise instance over the private network for workloads deployed on {{site.data.keyword.cloud_notm}} classic infrastructure, the Virtual Route Forwarding (VRF) and Service Endpoints features must be enabled in your account. For more information, see [Restricting network access](/docs/EventStreams?topic=EventStreams-restrict_access).

### Accessing an Enterprise instance over the private network from a VPC
{: #private_network_vpc}

For workloads deployed in an {{site.data.keyword.cloud_notm}} VPC to be able to access your Enterprise instance over the private network, a Virtual Private Endpoint (VPE) must be created in the VPC:
1. In the {{site.data.keyword.cloud_notm}} console, click the menu icon and select **VPC infrastructure** > **Network** > **Virtual private endpoint gateways**. 
2. Create a VPE for your {{site.data.keyword.messagehub}} instance by using the guidance in [About virtual private endpoint gateways](/docs/vpc?topic=vpc-about-vpe){: external}. 
3. After you create your VPE, it might take a few minutes for the new VPE and pDNS to complete the process and begin working for your VPC. Completion is confirmed when you see an IP address set in the [details view](/docs/vpc?topic=vpc-vpe-viewing-details-of-an-endpoint-gateway&interface=ui){: external} of the VPE. 

### Accessing an Enterprise instance over the private network from outside the {{site.data.keyword.cloud_notm}}
{: #private_network_outside_cloud}

Solutions such as [Direct Link 2.0](https://cloud.ibm.com/docs/dl){: external} can be utilized to establish a connection between an external network, such as an on-premise data center, and the {{site.data.keyword.cloud_notm}} private network. However, when dealing with workloads operating on an external network, it is essential to consider additional factors to ensure a successful connection to Kafka. Note that these considerations do not apply to HTTP workloads.

The private endpoint details allocated to your instance (as described in the service key) must be resolvable and routable from the network that the workload is running in. It is not possible to specify alternative hostname entries in the workload's `bootstrap.servers` properties as a way to route traffic from the external network.

The reason for this behavior is Kafka's two-step connection process. In the initial step, the hostnames that are provided in the client's `bootstrap.servers` property are used to establish the first bootstrap connection. However, the server then responds to the client with the actual endpoint hostname details it uses. These hostname details are the private endpoint details originally allocated to your instance and cannot be changed. Hence, the details must be resolvable and routable directly from the external network.

For more information, see [Accessing private API endpoints from an on-premises network by using IBM Cloud Direct Link](https://cloud.ibm.com/docs/vpc?topic=vpc-end-to-end-private-connectivity-vpe&interface=cli){: external}.

## What to do next
{: #after_connecting}

Now you have connection and credential information, you can choose a Kafka client. For more information, see [Using the Kafka API](/docs/EventStreams?topic=EventStreams-kafka_using).
