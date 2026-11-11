---

copyright:
  years: 2026
lastupdated: "2026-11-11"

keywords: connections, endpoints, cli, vpc, create service key

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Connecting to {{site.data.keyword.messagehub}} on {{site.data.keyword.cloud_notm}}: A step-by-step guide
{: #connecting}

Learn how to connect to {{site.data.keyword.messagehub}} on {{site.data.keyword.cloud}} by creating a virtual private endpoint and service credentials for secure authentication.
{: shortdesc}

To connect to {{site.data.keyword.messagehub}}, create a {{site.data.keyword.vpe_full}} gateway to establish a secure private connection. Then create credentials to authenticate with IAM. For applications that do not run within a VPC, additional networking setup is required.

The following information gives an overview of the required steps. The example uses a Kafka client application that runs on a VPC VSI, but you can apply it to any of the VPC application platforms. For apps that run externally to a VPC, further information describes the additional steps.

## Creating a VPE gateway
{: #create_VPE}

To enable an application deployed in an {{site.data.keyword.vpc_full}} to access your Enterprise instance over the private network, you must create a virtual private endpoint (VPE) gateway in the VPC.

1. In the {{site.data.keyword.cloud_notm}} console, click the menu icon and select **VPC infrastructure** > **Network** > **Virtual private endpoint gateways**.
2. Create a VPE gateway for your {{site.data.keyword.messagehub}} instance by using the guidance in [About virtual private endpoint gateways](/docs/vpc?topic=vpc-about-vpe){: external}. 
3. After you create your VPE gateway, it might take a few minutes for the new VPE gateway and pDNS to complete the process and begin working for your VPC. The process is complete when you see an IP address set in the [details view](/docs/vpc?topic=vpc-vpe-viewing-details-of-an-endpoint-gateway&interface=ui){: external} of the VPE gateway.
4. Note this VPE gateway IP address.

## Creating a service credential
{: #create_a_service_credential}

All plans use [IAM](/docs/account?topic=account-overview){: external} for authentication. You don't need to understand IAM to get started, but some knowledge is recommended if you want to secure your {{site.data.keyword.messagehub}} service. For more information, see [Managing access to your {{site.data.keyword.messagehub}} resources](/docs/EventStreams-gen2?topic=EventStreams-gen2-security). To complete the following steps and be authorized to create topics, your application must have a Manager access role. By default, the owner of the account that contains the service instance has this role.

A service credential creates an API key with the required access role. To create a service credential by using the {{site.data.keyword.cloud_notm}} console, complete the following steps.

1. Locate your {{site.data.keyword.messagehub}} service on the dashboard.
2. Click your service tile.
3. Click **Service credentials**.
4. Click **New credential**.
5. Complete the details for your new credential. Choose a name and a role and click **Add**. A new credential appears in the credentials list.
6. Click **View credentials** for the new credential to reveal the details in JSON format.

To create a service key by using the {{site.data.keyword.cloud_notm}} CLI, complete the following steps.

1. Locate your service:

    ```bash
    ibmcloud resource service-instances
    ```
    {: codeblock}

2. Create a service key:

    ```bash
    ibmcloud resource service-key-create KEY_NAME KEY_ROLE --instance-name YOUR_SERVICE_NAME
    ```
    {: codeblock}

3. Print the service key:

    ```bash
    ibmcloud resource service-key KEY_NAME
    ```
    {: codeblock}

## Connect from a VSI
{: #connect_from_VPE}

The following information shows the steps to connect to a VPE gateway from an application that runs on a VSI.

### Create an {{site.data.keyword.vpc_short}}
{: #create_a_vpc}

Set up a Virtual Private Cloud in your region. Make sure to select **Allow SSH** in the options for the VPC before creating. [Create your VPC](https://cloud.ibm.com/infrastructure/network/vpcs){: external}

Keep resources in the same region to avoid issues.
{: note}

### Create an SSH key
{: #create_a_ssh_key}

1. [Create an SSH key](https://cloud.ibm.com/infrastructure/compute/sshKeys){: external} in the same region as the VPC.

2. When your key is ready, move it to the .ssh directory on your local machine to follow best practices for secure SSH key management.

3. Update the key’s permissions to make it read-only for the file owner. On Unix-like systems such as macOS, run the following command:

    ```sh
    chmod 400 <COPY LOCAL LOCATION OF THE SSH KEY>
    ```
    {: pre}

### Create a Virtual Server Instance (VSI) in the VPC
{: #create_a_vsi}

[Create a VSI](https://cloud.ibm.com/infrastructure/compute/vs){: external} in the same region, make sure to:
- Choose **Ubuntu Linux** for operating system. (You can use the smallest profile.)
- Select the SSH Key created above.
- Select the VPC created above.

### Reserve a floating IP for your VSI
{: #create_a_floatingip}

[Reserve a floating IP address](https://cloud.ibm.com/infrastructure/network/floatingIPs){: external}. Make sure that you select the correct region and zone, and bind the address to the VSI that you created in the previous step.

This setting allows connections to the VSI from the internet. If required, you can modify the security group that is associated with the VSI to allow connections from only specific IP addresses, for example, the IP address of the machine that you connect from.
{: important}

### Log in to your VSI
{: #login_to_vsi}

In your terminal, SSH in to your VSI with the following command.

If you saved the private SSH key in a different directory, replace the file path in the command accordingly.
{: note}

```bash
ssh -i ~/.ssh/<NAME OF THE SSH KEY> root@<FLOATING IP ADDRESS>
```
{: codeblock}

Your local terminal session is now connected to your virtual server. Continue using this session for the next steps.

### Install and use the Kafka console tools
{: #install_tools}

The following steps show how to [install and use the console tools](/docs/EventStreams-gen2?topic=EventStreams-gen2-kafka_console_tools) to create a topic and then produce and consume a message.

## Accessing an Enterprise instance from an external network
{: #private_network_outside_cloud}

Solutions such as [Direct Link 2.0](/docs/dl) or [Transit Gateway](/docs/transit-gateway?topic=transit-gateway-about) can establish a connection between an external network, such as an on-premise data center or Classic infrastructure, and the {{site.data.keyword.cloud_notm}} VPE gateway. However, for workloads on an external network, consider additional factors to ensure a successful connection to Kafka. These considerations do not apply to HTTP workloads.

The private endpoint details allocated to your instance (as described in the service key) must be resolvable and routable from the network that the workload runs in. You can't specify alternative hostname entries in the workload's `bootstrap.servers` properties to route traffic from the external network.

This behavior occurs because of the Kafka two-step connection process. In the initial step, the client uses the hostnames in its `bootstrap.servers` property to establish the first bootstrap connection. The server then responds to the client with the actual endpoint hostname details. These hostname details are the private endpoint details originally allocated to your instance, and you can't change them. Therefore, the details must be resolvable and routable directly from the external network.

For more information, see [Accessing a VPE externally from a VPC](/docs/vpc?topic=vpc-end-to-end-private-connectivity-vpe&interface=cli){: external}.

## What to do next
{: #after_connecting}

After you have connection and credential information, you can choose a Kafka client. For more information, see [Using the Kafka API](/docs/EventStreams-gen2?topic=EventStreams-gen2-using_kafka_api).

## Further information
{: #further_info}

- [Configuring your Kafka API client](/docs/EventStreams-gen2?topic=EventStreams-gen2-using_kafka_api#kafka_api_client). 
- [Securing {{site.data.keyword.messagehub}} with context-based restrictions on {{site.data.keyword.cloud_notm}}](/docs/EventStreams-gen2?topic=EventStreams-gen2-cbr).
