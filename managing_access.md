---

copyright:
  years: 2026
lastupdated: "2026-11-11"

keywords: client, wildcarding, wildcard, policies

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Managing authentication to {{site.data.keyword.messagehub}} instances on {{site.data.keyword.cloud_notm}}
{: #security}

[Gen 2]{: tag-purple}

Learn about authentication methods for {{site.data.keyword.messagehub}} instances, including SASL PLAIN and OAUTHBEARER mechanisms and IAM integration for secure access.
{: shortdesc}

{{site.data.keyword.messagehub}} supports two [SASL](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer) (Simple Authentication and Security Layer) mechanisms as the authentication methods to {{site.data.keyword.messagehub}} instances by default: PLAIN and OAUTHBEARER.

A Kafka client that is configured with SASL PLAIN uses an IAM API key as a plain text password in the authentication process. {{site.data.keyword.messagehub}} sends the API key to IAM for verification. When authenticated, the client stays connected and does not require re-authentication until it disconnects and reconnects.

A Kafka client that is configured with SASL OAUTHBEARER uses an IAM access token in the authentication process. {{site.data.keyword.messagehub}} verifies the token by using the IAM public key. Because an IAM access token has an expiration time (usually 1 hour), the Kafka client must regenerate a new token and go through the authentication process again when the previous token approaches its expiration time. This approach provides better security than SASL PLAIN in two ways:

1. The API key always stays on the client side to generate the access token and is no longer sent to Kafka brokers over the network, which removes the risk of API key exposure.
2. The authentication process happens on a regular basis when the access token is expiring, which minimizes the risk of token exposure.

For more secure authentication, SASL OAUTHBEARER is the only recommended authentication method for Kafka clients. For more information about how to configure SASL OAUTHBEARER in Kafka clients, see [Configuring your Kafka API client](/docs/EventStreams-gen2?topic=EventStreams-gen2-using_kafka_api#kafka_api_client).

You can disable SASL PLAIN in your Enterprise instances by running the following command:

```sh
ibmcloud resource service-instance-update INSTANCE_NAME -p '{"dataservices":{"options":{"iam_token_only":true}}}'
 
```

## Connecting to {{site.data.keyword.messagehub}}
{: #connect_message_enterprise}

For more information about how to get a security key credential for an external application, see [Connecting to {{site.data.keyword.messagehub}}](/docs/EventStreams-gen2?topic=EventStreams-gen2-connecting).

## Managing authorization to your {{site.data.keyword.messagehub}} resources
{: #security_resources}

You can secure your {{site.data.keyword.messagehub}} resources in a fine-grained manner to manage the access that you want to grant each user to each resource.

When you change IAM policies and permissions, the changes can take several minutes to take effect in the underlying service.
{: important}

## What can I secure?
{: #what_secure}

Within {{site.data.keyword.messagehub}}, you can secure access to the following resources:

- Cluster (cluster): You can control which applications and users can connect to the service.
- Topics (topic): You can control the ability of users and applications to create, delete, read, and write to a topic.
- Consumer groups (group): You can control an application's ability to join a consumer group.
- Producer transactions (txnid): You can control the ability to use the transactional producer capability in Kafka (that is, single, atomic writes across multiple partitions).

The following table lists the levels of access (also known as roles) that you can assign to a user for each resource.

| Access role | Description of actions | Example actions |
|:-----------------|:-----------------|:-----------------|
|  Reader | Perform read-only actions within {{site.data.keyword.messagehub}} such as viewing resources. | Allow an app to connect to a cluster by assigning read access to cluster resource type. |
| Writer | Writers have permissions beyond the reader role, including editing {{site.data.keyword.messagehub}} resources. | Allow an app to produce to topics by assigning write access to topic resource and topic name types. |
| Manager | Managers have permissions beyond the writer role to complete privileged actions. In addition, you can create and edit {{site.data.keyword.messagehub}} resources. | Allow full access to all resources by assigning manage access to the {{site.data.keyword.messagehub}} instance. |
{: caption="Example {{site.data.keyword.messagehub}} user roles and actions" caption-side="bottom"}

## How do I assign access?
{: #assign_access}

You attach Cloud Identity and Access Management (IAM) policies to the resources that you want to control. Each policy defines the level of access that a particular user must have and to which resource or set of resources. A policy consists of the following information:

- The type of service the policy applies to. For example, {{site.data.keyword.messagehub}}. You can scope a policy to include all service types.
- The instance of the service to be secured. You can scope a policy to include all instances of a service type.
- The type of resource to be secured. The valid values are `cluster`, `topic`, `group`, `schema`, or `txnid`. Specifying a type is optional. If you do not specify a type, the policy applies to all resources in the service instance. If you want to specify more than one type of resource, you must create one policy per resource.
- The resource to be secured. Specify for resources of type `topic`, `group`, `schema`, and `txnid`. If you do not specify the resource, the policy applies to all resources of the type specified in the service instance.
- The role that is assigned to the user. For example, Reader, Writer, or Manager.

For more information about IAM, see [{{site.data.keyword.iamlong}}](/docs/iam?topic=iam-iamoverview).

### Wildcarding
{: #wildcarding}

You can use the IAM wildcarding facility to set policies for groups of resources on {{site.data.keyword.messagehub}}. For example, if you give all your topics names like `Dept1_Topic1` and `Dept1_Topic2`, you can set policies for topics that are called `Dept1_*` and the policies apply to all topics with that prefix. For more information, see [Assigning access by using wildcard policies](/docs/iam?topic=iam-wildcard).

## What are the default security settings?
{: #default_settings}

By default, when {{site.data.keyword.messagehub}} is provisioned, the user who provisioned it is granted the manager role to all the instance's resources. Additionally, any user who has a manager role for either **All** services or **All** {{site.data.keyword.messagehub}} service instances in the same account also has full access.

You can then apply more policies to extend access to other users. You can either scope a policy to apply to {{site.data.keyword.messagehub}} as a whole or to individual resources within {{site.data.keyword.messagehub}}. For more information, see [Common actions](#common_actions).

Only users with an administration role for an account can assign policies to users. Assign policies either by using the {{site.data.keyword.cloud}} dashboard or by using the **ibmcloud** commands.

## Common actions
{: #common_actions}

The following tables summarize some common {{site.data.keyword.messagehub}} actions and the access that you need to assign.

### Cluster requirements
{: #cluster_actions}

By controlling access to the cluster resource, you can determine which applications and users can connect to the service. In addition to the policies required for the resource types in the following sections, access to `ResourceType: Cluster` and a `Role: Reader, Writer, Manager` is required.

### Producer actions
{: #producing_actions}

The following table describes the role and resource requirements that are needed by a user or an application that produces messages to {{site.data.keyword.messagehub}}. In addition to the policies required for this resource type, access to `ResourceType: Cluster` and a `Role: Reader, Writer, Manager` is required.

| Producer actions | Topic | Group | Txnid |
| --- | --- | --- | --- |
| Send a message to a topic. | Writer |  | Writer [^tabletext1] |
| Allow an app to produce to a topic transactionally. | Writer | Reader | Writer |
| Initialize a transaction. |  |  | Writer |
| Commit a transaction. | Writer |  | Writer |
| Abort a transaction. |  |  | Writer |
| Send offsets to a transaction. |  | Reader | Writer |
{: caption="Producer actions" caption-side="bottom"}

[^tabletext1]: Writer on txnid is only required for transactional produce.

### Consumer actions
{: #consumer_actions}

The following table describes the role and resource requirements that are needed by a user or application that consumes messages from {{site.data.keyword.messagehub}}. In addition to the policies required for this resource type, access to `ResourceType: Cluster` and a `Role: Reader, Writer, Manager` is required.

| Consumer actions | Topic | Group | Txnid |
| --- | --- | --- | --- |
| Allow an app to consume a topic (consumer group). | Reader | Reader [^tabletext2] |   |
| Allow an app to connect and consume from a specific topic (no consumer group). | Reader |  |  |
| Allow an app to connect and consume from any topic (no consumer group). | Reader |  |  |
| Use Kafka Streams. | Manager | Reader |  |
| Delete consumer group. |  | Manager |  |
| Assign. |  | Reader |  |
| Commit async. | Reader | Reader |  |
| Commit sync. | Reader | Reader |  |
| Enforce rebalance. |  | Reader |  |
| Poll. |  | Reader |  |
| Subscribe. |  | Reader |  |
| Unsubscribe. |  | Reader | Writer |
{: caption="Consumer actions" caption-side="bottom"}

[^tabletext2]: Reader on group is only required if the assign causes the consumer to leave its current group.

### Administration actions
{: #administration_actions}

In addition to the policies required for this resource type, access to `ResourceType: Cluster` and a `Role: Reader, Writer, Manager` is required.

| Administration actions | Topic  | Group  | Txnid |
| --- | --- | --- | --- |
| Alter topic configurations. | Manager |  |  |
| Alter consumer group offsets. | Reader | Reader |  |
| Create partitions. | Manager |  |  |
| Create topics. | Manager |  |  |
| Delete consumer group offsets. | Reader | Manager |  |
| Delete consumer groups. |  | Manager |  |
| Delete records. |  | Manager |  |
| Delete topics. |  | Manager |  |
| Describe producers. | Reader |  |  |
| Fence producers. |  |  | Writer |
| Incrementally alter topic configurations. | Manager |  |  |
| Remove members from consumer group. |  | Reader |  |
{: caption="Administration actions" caption-side="bottom"}
