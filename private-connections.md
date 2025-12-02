---

copyright:
  years: 2025
lastupdated: "2025-12-02"

keywords: connections, endpoints, cli, vpc, create service key

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}


# Virtual Private Endpoints for Gen 2 {{site.data.keyword.messagehub}}
{: #private-endpoints-gen2}

[Gen 2]{: tag-purple}

{{site.data.keyword.messagehub}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see [Beta reference](/docs/EventStreams-gen2?topic=EventStreams-gen2-gen2-beta).
{: beta}

[Gen 2 {{site.data.keyword.messagehub}}](https://github.ibm.com/cloud-docs-solutions/cloud-databases-gen2/wiki) enable a secure by default approach using private connections enabled by Virtual Private Endpoints (VPE).

{{site.data.keyword.cloud}} recommends private endpoints that restrict access to your internal network, protecting your data and applications from unwanted access and security vulnerabilities.

Private endpoints also provide greater control over network traffic to your databases for better visibility and enable you to demonstrate adherence to compliance and regulatory requirements. Gen 2 {{site.data.keyword.messagehub}} support context-based restrictions that allow account owners and administrators to define and enforce access restrictions for {{site.data.keyword.cloud_notm}} resources based on the context of access requests.

Setting up Virtual Private Endpoints will incur costs and network transfer charges apply to {{site.data.keyword.cloud_notm}} VPC services using private endpoints. Charges are based on the volume of data transferred and follow a tiered pricing model. Use the [Cost estimator](https://cloud.ibm.com/estimator) to calculate an estimate based on your projected usage.
{: .note}

## Options to connect to your VPC instance via private endpoints
{: #methods-privateendpoints}

You can access your instance from your local device or non-VPC client via private endpoints. Connect to VPE for VPC to provide a secure connectivity to services and instances originating from your VPC network. The instructions to connect to your Gen 2 (VPC) instance depend on where you're connecting from and where your application is running.

1. VPE via VSI (Client to Site) - This method uses a Virtual Server Instance (VSI) that resides in your Virtual Private Cloud (VPC) and leverages the Virtual Private Endpoint (VPE) to provide secure, private connectivity between resources within the VPC and external {{site.data.keyword.cloud_notm}} services. For more information, see [Client to Site](/docs/vpc?topic=vpc-vpn-client-to-site-overview).
1. Use VPN connection established through a VPE. The VPN lets you connect and manage from a local laptop or client via the VPC/VPE.
1. Use a VPC/VPN gateway for secure and private on-premises access to cloud resources. For more information, see [Site to Site](/docs/solution-tutorials?topic=solution-tutorials-vpc-site2site-vpn).

## Establishing Virtual Private Endpoints through a VSI
{: #howto-privateendpoints}

### Step 1: Create a VPC
{: #howto-privateendpoints-vsi1}

Use {{site.data.keyword.cloud_notm}} instructions to [create a Virtual Private Cloud (VPC)](/infrastructure/network/vpcs). Ensure that the VPC is in the same region as your database deployment.

### Step 2: Create a VSI and an SSH key
{: #howto-privateendpoints-vsi2}

1. Provision a VSI within the VPC using [instructions for creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui) in the [Virtual server instances for VPC UI](/infrastructure/compute/vs). Assign a public IP to the VSI to allow external SSH access (optional for testing).
2. Generate and attach an SSH key for secure login. For detailed instructions on how to create an SSH key, see [Getting started with VPC](/docs/vpc?topic=vpc-ssh-keys&interface=ui).
3. Once the key is ready, move it into the .ssh directory on your local machine to follow best practises for secure SSH key management.
4. Log in to your VSI and change the permissions of the key. This makes the key read-only to the file owner. On Unix-like systems such as macOS, run the following command:

    `$ chmod 400 <COPY_LOCAL_LOCATION_OF_THE_SSH_KEY>`
   
5. SSH into your VSI using the following command:

    `$ ssh -i ~/.ss/<NAME_OF_THE_SSH_KEY> root@<FLOATING_IP_ADDRESS>`

### Step 3: Create a VPE
{: #howto-privateendpoints-vsi3}

In the {{site.data.keyword.cloud_notm}} console, click the menu icon and select **VPC Infrastructure > Network > Virtual private endpoint gateways**. Create a VPE for your {{site.data.keyword.messagehub}} instances with [these instructions](/docs/vpc?topic=vpc-about-vpe).

Bind the VPE to your VPC and subnet.

### Step 4: Update VPC security groups
{: #howto-privateendpoints-vsi4}

Modify the security group to allow outbound traffic from your VSI to the database instance. Ensure ports required by your database (for example, Postgresql or MongoDB) are open.

### Step 5: Bind database deployment to VSI
{: #howto-privateendpoints-vsi5}

Use {{site.data.keyword.cloud_notm}} CLI or UI to bind your database deployment to your VSI. This stores the connection strings in a secret.

### Step 6: Configure application
{: #howto-privateendpoints-vsi6}

1. Retrieve the connection string from the secret.
1. Update your application configuration to use the private endpoint connection string. This stores the connection strings in a secret.

### Step 7: Install Client tools on VSI
{: #howto-privateendpoints-vsi7}

1. Use {{site.data.keyword.cloud}} CLI or UI to bind your database deployment to your VSI. This stores the connection strings in a secret.
1. SSH into your VSI.

### Step 8: Connect to your database with private endpoints
{: #howto-privateendpoints-vsi8}

Install and verify the required client. Connect to the VSI from a local environment by sending a root certificate from a local machine to a VSI. This command will vary depending on the database service and the client. The instructions required per database arelisted in the table below.

| Service   | Client tool | Sample command                                                                 |
|-----------------------------|------------------|-------------------------------------------------------------------------------------|
| {{site.data.keyword.databases-for-postgresql}}    | `psql`           | You can connect to the VSI from a local environment by sending the root certificate from a local machine to a VSI.`$  PGPASSWORD=$PASSWORD psql “<PASSWORD>` You can verify your connection with the following command (optional):`$ /list`       |
| {{site.data.keyword.databases-for-mongodb}}       | `mongosh`           | You can verify your connection with the following command:`--eval "db.stats()"`     |
{: caption="Client tools and example connection commands for private access" }

## Establishing Virtual Private Endpoints using a VPN connection
{: #howto-privateendpoints-vpn}

Use a VPN connection established through a VPE. The VPN lets you connect and manage your instance from a local laptop or client via the virtual private endpoint in your VPE.

### Step 1: Create a VPC
{: #howto-privateendpoints-vpn1}

Use {{site.data.keyword.cloud_notm}} instructions to [create a Virtual Private Cloud (VPC)](/infrastructure/network/vpcs). Ensure the VPC is in the same region as your database deployment.

### Step 2: Create a VPE
{: #howto-privateendpoints-vpn2}

1. In the {{site.data.keyword.cloud_notm}} console, click the menu icon and select **VPC Infrastructure > Network > Virtual private endpoint gateways**. Create a VPE for your {{site.data.keyword.messagehub}} instances with [these instructions](/docs/vpc?topic=vpc-about-vpe).
1. Bind the VPE to your VPC and subnet.

### Step 3: Update VPC security groups
{: #howto-privateendpoints-vpn3}

Modify the security group to allow outbound traffic from your VSI to the database instance. Ensure ports required by your database (for example, Postgresql or MongoDB) are open. Check `allow SSH` in default security groups.

### Step 4: Build your connection by creating a VPN
{: #howto-privateendpoints-vpn4}

1. Create an {{site.data.keyword.cloud_notm}} [secrets manager instance](/docs/secrets-manager?topic=secrets-manager-create-instance&interface=ui).
1. Create certificates for VPN server (using a third party certificate authority).
1. Create IAM service to service authorisation for the VPN to secrets manager to enable the VPN server to securely retrieve credentials and configuration data.
1. Create VPN server and configure the VPN server to use the credentials from secrets manager.

You have now set up a secure connection path to allow the VPN to establish a secure tunnel.

### Step 5: Connect to the VPN and login to the network
{: #howto-privateendpoints-vpn5}

Download the client file to connect to the VPN and log into the network.

### Step 7: Connect to your database with private endpoints
{: #howto-privateendpoints-vpn7}

| Service   | Client tool | Sample command                                                                 |
|-----------------------------|------------------|-------------------------------------------------------------------------------------|
| {{site.data.keyword.messagehub}} | `psql`           | You can verify your connection with the following command:`$ /list`       |
{: caption="Client tools and example connection commands for private access via VPN" }

## Establishing VPC/VPN gateway from on-premise
{: #howto-privateendpoints-site2site}

Use a VPC/VPN gateway for secure and private on-premises access to cloud resources.

### Step 1: Create a VPC
{: #howto-privateendpoints-vpn1}

Use {{site.data.keyword.cloud_notm}} instructions to [create a Virtual Private Cloud (VPC)](/infrastructure/network/vpcs).
Ensure the VPC is in the same region as your database deployment.

### Step 2: Create a VPE
{: #howto-privateendpoints-vpn2}

1. In the {{site.data.keyword.cloud_notm}} console, click the menu icon and select -> VPC Infrastructure -> Network -> Virtual private endpoint gateways. Create a VPE for your {{site.data.keyword.messagehub}} instances with [these instructions](/docs/vpc?topic=vpc-about-vpe).
1. Bind the VPE to your VPC and subnet.

### Step 3: Update VPC security groups
{: #howto-privateendpoints-vpn3}

Update security groups with rules to allow traffic between the VPN and Gateway and VPE.

### Step 4: Build your connection by creating a VPN
{: #howto-privateendpoints-vpn4}

1. Create a VPN Gateway in the VPC.
2. Use [strongSwan](https://www.strongswan.org/), a popular VPN Gatewayto simulate an on-premise VPN Gateway.
3. Configure DNS resolution to route queries through {{site.data.keyword.cloud_notm}} DNS resolver.

### Step 5: Log in to the network
{: #howto-privateendpoints-site2site5}

Log in to the network by connecting through the VPN. When the service-to-service VPN is active, the secure connection path works automatically, allowing access to the on-premise VSI and cloud bastion host.

### Step 6: Connect to your database with private endpoints
{: #howto-privateendpoints-vpn7}

Connect to your database from an on-premise location by installing and verifying the required client. The clients for each database and sample verification commands are shown in the following table:

| Service   | Client tool | Sample command                                                                 |
|-----------------------------|------------------|-------------------------------------------------------------------------------------|
| {{{site.data.keyword.messagehub}} | `psql`           | You can verify your connection with the following command (optional):`$ /list`       |
{: caption="Client tools and example connection commands for private access via VPN" }
