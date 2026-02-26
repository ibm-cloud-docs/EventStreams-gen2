---

copyright:
  years: 2026
lastupdated: "2026-02-16"

keywords: connections, endpoints, cli, vpc, create service key

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Connecting to {{site.data.keyword.messagehub}}
{: #connecting}

To connect to {{site.data.keyword.messagehub}}, a {{site.data.keyword.vpe_full}} must be created to establish a secure private connection. Credentials must then be created to authenticate with IAM. For applications not running within VPC, additional networking setup is required.

The following information gives an overview of the steps required, using the example of a Kafka client application running on a VPC VSI, but could be applied to any of the VPC application platforms. For apps running externally to a VPC, further information is provided for the additional steps required.
{: #shortdesc}

### Creating a VPE
{: #create_VPE}

To enable an application deployed in an {{site.data.keyword.vpc_full}} to access your Enterprise instance over the private network, a virtual private endpoint (VPE) must be created in the VPC.

1. In the {{site.data.keyword.cloud_notm}} console, click the menu icon and select **VPC infrastructure** > **Network** > **Virtual private endpoint gateways**.
2. Create a VPE for your {{site.data.keyword.messagehub}} instance by using the guidance in [About virtual private endpoint gateways](/docs/vpc?topic=vpc-about-vpe){: external}. 
3. After you create your VPE, it might take a few minutes for the new VPE and pDNS to complete the process and begin working for your VPC. Completion is confirmed when you see an IP address set in the [details view](/docs/vpc?topic=vpc-vpe-viewing-details-of-an-endpoint-gateway&interface=ui){: external} of the VPE.
4. Take a note of this VPE IP address.

### Creating a service credential
{: #create_a_service_credential}

All plans use [IAM](/docs/account?topic=account-overview){: external} for authentication. You don't need to understand IAM to get started but some knowledge is recommended if you want to secure your {{site.data.keyword.messagehub}} service. For more information, see [Managing access to your {{site.data.keyword.messagehub}} resources](/docs/EventStreams-gen2?topic=EventStreams-gen2-security). To complete the following steps and be authorized to create topics, your application must have a Manager access role. By default, the owner of the account that contains the service instance has this role.

A service credential enables an API key to be created with the required access role. To create a service credential by using the {{site.data.keyword.cloud_notm}} console, complete the following steps.

1. Locate your {{site.data.keyword.messagehub}} service on the dashboard.
2. Click your service tile.
3. Click **Service credentials**.
4. Click **New credential**.
5. Complete the details for your new credential. Choose a name and a role and click **Add**. A new credential appears in the credentials list.
6. Click the new credential by using **View credentials** to reveal the details in JSON format.

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

## Connect from a VSI
{: #connect_from_VPE}

The following information shows the steps needed to connect to a VPE from an application running on a VSI.

### Create an {{site.data.keyword.vpc_short}}
{: #create_a_vpc}

Set up a Virtual Private Cloud in your region. Make sure to select 'Allow SSH' in the options for the VPC before creating. [Create your VPC](https://cloud.ibm.com/infrastructure/network/vpcs){: external}

Keep resources in the same region to avoid issues.
{: note}

### Create an SSH key
{: #create_a_ssh_key}

1. [Create an SSH key](https://cloud.ibm.com/infrastructure/compute/sshKeys){: external} in the same region as the VPC.

2. Once your key is ready, move it to the .ssh directory on your local machine to follow best practices for secure SSH key management.

3. Update the key’s permissions to make it read-only for the file owner. On Unix-like systems such as macOS, run the following command:

    ```sh
    $ chmod 400 <COPY LOCAL LOCATION OF THE SSH KEY>
    ```
    {: pre}

### Create a Virtual Server Instance (VSI) in the VPC
{: #create_a_vsi}

[Create a VSI](https://cloud.ibm.com/infrastructure/compute/vs){: external} in the same region, make sure to:
- Choose *Ubuntu Linux* for operating system. (You can use the smallest profile.)
- Select the SSH Key created above.
- Select the VPC created above.

### Reserve a floating IP for your VSI
{: #create_a_floatingip}

[Reserve a floating IP address](https://cloud.ibm.com/infrastructure/network/floatingIPs){: external}. Make sure the correct region and zone is selected, and bind it to the VSI created in the previous step.

This enables the VSI to be connected to from the internet. If required, the security group associated with the VSI can be modified to only allow connections from specific IP addresses, for example, the specific IP address of the machine you connect from.
{: important}

### Log in to your VSI
{: #login_to_vsi}

In your terminal, SSH in to your VSI with the following command.

If you saved the private SSH key in a different directory, replace the file path in the command accordingly.
{: note}

```bash
$ ssh -i ~/.ssh/<NAME OF THE SSH KEY> root@<FLOATING IP ADDRESS>
```
{: codeblock}

Your local terminal session should now be connected to your virtual server. Continue using this session for the next steps.

### Install and use the Kafka console tools
{: #install_tools}

The following steps show how to [install and use the console tools](/docs/EventStreams-gen2?topic=EventStreams-gen2-kafka_console_tools) to create a topic and then produce and consume a message.

## Accessing an Enterprise instance from an external network
{: #private_network_outside_cloud}

Solutions such as [Direct Link 2.0](https://cloud.ibm.com/docs/dl){: external} or [Transit Gateway](https://cloud.ibm.com/docs/transit-gateway?topic=transit-gateway-about){: external} can be utilized to establish a connection between an external network, such as an on-premise data center or Classic infrastructure, and the {{site.data.keyword.cloud_notm}} VPE. However, when dealing with workloads operating on an external network, it is essential to consider additional factors to ensure a successful connection to Kafka. Note that these considerations do not apply to HTTP workloads.

The private endpoint details allocated to your instance (as described in the service key) must be resolvable and routable from the network that the workload is running in. It is not possible to specify alternative hostname entries in the workload's `bootstrap.servers` properties as a way to route traffic from the external network.

The reason for this behavior is Kafka's two-step connection process. In the initial step, the hostnames that are provided in the client's `bootstrap.servers` property are used to establish the first bootstrap connection. However, the server then responds to the client with the actual endpoint hostname details it uses. These hostname details are the private endpoint details originally allocated to your instance and cannot be changed. Hence, the details must be resolvable and routable directly from the external network.

For more information, see [Accessing a VPE externally from a VPC](/docs/vpc?topic=vpc-end-to-end-private-connectivity-vpe&interface=cli){: external}.

## What to do next
{: #after_connecting}

Now you have connection and credential information, you can choose a Kafka client. For more information, see [Using the Kafka API](/docs/EventStreams-gen2?topic=EventStreams-gen2-kafka_using).

## Further Information
{: #further_info}

- [Configuring your Kafka API client](/docs/EventStreams-gen2?topic=EventStreams-gen2-kafka_using#kafka_api_client). 
- [Restricting network access](/docs/EventStreams?topic=EventStreams-restrict_access).
