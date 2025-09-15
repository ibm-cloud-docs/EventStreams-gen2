---

copyright:
  years: 2015, 2024
lastupdated: "2024-02-20"

keywords: client, wildcarding, wildcard, policies

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Managing authentication to your {{site.data.keyword.messagehub}} instances
{: #security}

{{site.data.keyword.messagehub}} supports two [SASL](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer) (Simple Authentication and Security Layer) mechanisms as the authentication methods to {{site.data.keyword.messagehub}} instances by default: PLAIN and OAUTHBEARER.
{: shortdesc}

Kafka client configured with SASL PLAIN uses an IAM API key as a plain text password in the authentication process, {{site.data.keyword.messagehub}} sends the API key to IAM for verification. When authenticated, this client will keep connected and will not require re-authentication until it is disconnected and wants to re-connect.

Kafka client configured with SASL OAUTHBEARER uses IAM access token in the authentication process, {{site.data.keyword.messagehub}} verifies the token via IAM public key. Because an IAM access token has an expiration time (usually at 1 hour), Kafka client is required to re-generate a new token and go through the authentication process again when previous token is approaching expiration time. This approach provides better security comparing to SASL PLAIN in two ways: 

1. The API key always stays at client side to generate the access token and is no longer sent to Kafka brokers over the network, which removes the risk of API key exposure. 
2. The authentication process happens on a regular basis when the access token is expiring and this minimizes the risk of token exposure.

For more secure authentication, SASL OAUTHBEARER is the only recommended authentication method for Kafka clients. See [Configuring your Kafka API client](/docs/EventStreams?topic=EventStreams-kafka_using#kafka_api_client) how to configure SASL OAUTHBEARER in Kafka clients.

Enterprise users have the option to disable SASL PLAIN in their Enterprise instances. Use the following command:

```sh
ibmcloud resource service-instance-update <instance-name> -p '{"iam_token_only":true}'
```

## Connecting to {{site.data.keyword.messagehub}}
{: #connect_message_enterprise }

For more information about how to get a security key credential for an external application, see [Connecting to {{site.data.keyword.messagehub}}](/docs/EventStreams?topic=EventStreams-connecting).

## Managing authorization to your {{site.data.keyword.messagehub}} resources
{: #security_resources}

You can secure your {{site.data.keyword.messagehub}} resources in a fine-grained manner to manage the access that you want to grant each user to each resource.

When you change IAM policies and permissions, they can sometimes take several minutes to be reflected in the underlying service.
{: important}

## What can I secure?
{: #what_secure}

Within {{site.data.keyword.messagehub}}, you have secure access to the following resources:

- Cluster (cluster): You can control which applications and users can connect to the service.
- Topics (topic): You can control the ability of users and applications to create, delete, read, and write to a topic.
- Consumer groups (group): You can control an application's ability to join a consumer group.
- Producer transactions (txnid): You can control the ability to use the transactional producer capability in Kafka (that is, single, atomic writes across multiple partitions).

The levels of access (also known as a role) that you can assign to a user to each resource are as follows.

| Access role | Description of actions | Example actions |
|:-----------------|:-----------------|:-----------------|
|  Reader | Perform read-only actions within {{site.data.keyword.messagehub}} such as viewing resources. | Allow an app to connect to a cluster by assigning read access to cluster resource type. |
| Writer | Writers have permissions beyond the reader role, including editing {{site.data.keyword.messagehub}} resources. | Allow an app to produce to topics by assigning write access to topic resource and topic name types. |
| Manager | Managers have permissions beyond the writer role to complete privileged actions. In addition, you can create and edit {{site.data.keyword.messagehub}} resources. | Allow full access to all resources by assigning manage access to the {{site.data.keyword.messagehub}} instance.
{: caption="Example {{site.data.keyword.messagehub}} user roles and actions" caption-side="bottom"}

## How do I assign access?
{: #assign_access }

Cloud Identity and Access Management (IAM) policies are attached to the resources to be controlled. Each policy defines the level of access that a particular user must have and to which resource or set of resources. A policy consists of the following information:

- The type of service the policy applies to. For example, {{site.data.keyword.messagehub}}. You can scope a policy to include all service types.
- The instance of the service to be secured. You can scope a policy to include all instances of a service type.
- The type of resource to be secured. The valid values are `cluster`, `topic`, `group`, `schema`, or `txnid`. Specifying a type is optional. If you do not specify a type, the policy then applies to all resources in the service instance. If you want to specify more than one type of resource, you must create one policy per resource.
- The resource to be secured. Specify for resources of type `topic`, `group`, `schema`, and `txnid`. If you do not specify the resource, the policy then applies to all resources of the type specified in the service instance.
- The role that is assigned to the user. For example, Reader, Writer, or Manager.

For more information about IAM, see [IBM Cloud Identity and Access Management](/docs/account?topic=account-iamoverview).

For an example of how to set policies, see [IBM Cloud IAM Service IDs and API Keys](https://www.ibm.com/blog/introducing-ibm-cloud-iam-service-ids-api-keys/){: external}.

### Wildcarding
{: #wildcarding }

You can take advantage of the IAM wildcarding facility to set policies for groups of resources on {{site.data.keyword.messagehub}}. For example, if you give all your topics names like `Dept1_Topic1` and `Dept1_Topic2`, you can set policies for topics that are called `Dept1_*` and these policies are applied to all topics with that prefix. For more information, see [Assigning access by using wildcard policies](/docs/account?topic=account-wildcard){: external}.

## What are the default security settings?
{: #default_settings }

By default, when {{site.data.keyword.messagehub}} is provisioned, the user who provisioned it is granted the manager role to all the instance's resources. Additionally, any user who has a manager role for either 'All' services or 'All' {{site.data.keyword.messagehub}} service instances in the same account also has full access.

You can then apply more policies to extend access to other users. You can either scope a policy to apply to {{site.data.keyword.messagehub}} as a whole or to individual resources within {{site.data.keyword.messagehub}}. For more information, see [Common actions](#common_actions).

Only users with an administration role for an account can assign policies to users. Assign policies either by using IBM Cloud dashboard or by using the **ibmcloud** commands.

## Common actions
{: #common_actions}

The following tables summarize some common {{site.data.keyword.messagehub}} actions and the access that you need to assign.

### Cluster requirements
{: #cluster_actions}

By controlling access to the cluster resource, you can determine which applications and users can connect to the service. In addition to the policies required for the resource types below, access to `ResourceType: Cluster` and a `Role: Reader, Writer, Manager` is required. 

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

### Schema Registry actions
{: #schema_registry_actions} 

With Schema Registry actions, you can alter the schema version, such as create, update, and delete artifact or artifact versions (Enterprise plan only). *Artifact* is the term that {{site.data.keyword.messagehub}} uses to describe related schemas, often associated with and used by a particular Kafka topic. The term *subject* is often used to describe the same concept. For more information, see [Using Event Streams Schema Registry](/docs/EventStreams?topic=EventStreams-ES_schema_registry). In addition to the policies required for this resource type, access to `ResourceType: Cluster` and a `Role: Reader, Writer, Manager` is required.

| Schema Registry actions | Schema  |
| --- | --- |
| Get latest artifact. | Reader |
| List versions. | Reader |
| Get version. | Reader |
| Get metadata by content. | Reader  |
| Get metadata. | Reader |
| Get version metadata. | Reader |
| Get the schema string identified by the input ID.  | Reader |
| Retrieve only the schema identified by the input ID. | Reader |
| Get the subject-version pairs identified by the input ID. | Reader |
| Get a list of versions registered under the specified subject. | Reader |
| Get artifact compatibility rule. | Reader |
| Get a specific version of the schema registered under this subject. | Reader |
| Get the schema for the specified version of this subject. | Reader |
| Register a new schema under the specified subject (if version already exists). | Reader |
| Check if a schema has already been registered under the specified subject. | Reader|
| Get a list of IDs of schemas that reference the schema with the given subject and version.  | Reader |
| Test input schema against a particular version of a subject’s schema for compatibility. | Reader |
| Perform a compatibility check on the schema against one or more versions in the subject. | Reader |
| Get compatibility level for a subject. | Reader |
| Register a new schema under the specified subject (if version is to be created). | Writer |
| Create artifact. | Writer  |
| Update artifact. | Writer  |
| Disable artifact. |  Writer |
| Create version. | Writer  |
| Delete version. | Manager  |
| Update artifact state. | Manager  |
| Update version state. | Manager  |
| Delete artifact. | Manager  |
| Create artifact compatibility rule. | Manager  |
| Update artifact compatibility rule. | Manager  |
| Update compatibility level for the specified subject. | Manager  |
| Delete artifact compatibility rule. | Manager  |
| Deletes the specified subject and its associated compatibility level if registered. | Manager  |
| Delete a specific version of the schema registered under this subject. | Manager |
| Delete the specified subject-level compatibility level config and reverts to the global default. | Manager  |
| Update the global compatibility rule. [^tabletext3] |  |
| Update the global compatibility level. [^tabletext4] |  |
{: caption="Schema Registry actions" caption-side="bottom"}

[^tabletext3]: You do not need access to the schema resource, instead Manager access on the cluster resource is required.
[^tabletext4]: You do not need access to the schema resource, instead Manager access on the cluster resource is required.

### Schema Registry compatibility actions
{: #schema_registry_compatibility_actions}

For interoperation with existing applications, the {{site.data.keyword.messagehub}} Schema Registry supports a subset of the [Confluent Schema Registry API v7.2](https://docs.confluent.io/platform/current/schema-registry/develop/api.html). To perform these actions, you need the following resource level access.

| Schema Registry compatibility actions | Schema  |
| --- | --- |
| Get the schema string identified by the input ID. | Reader |
| Retrieves only the schema identified by the input ID. | Reader |
| Get the schema types that are registered with Schema Registry. |  |
| Get the subject-version pairs identified by the input ID. | Reader |
| Get a list of registered subjects. |  |
| Get a list of versions registered under the specified subject. | Reader |
| Deletes the specified subject and its associated compatibility level if registered. | Manager |
| Get a specific version of the schema registered under this subject. | Reader |
| Get the schema for the specified version of this subject. | Reader |
| Register a new schema under the specified subject. | Reader/Writer [^tabletext5] |
| Check if a schema has already been registered under the specified subject. | Reader |
| Deletes a specific version of the schema registered under this subject. | Manager |
| Get a list of IDs of schemas that reference the schema with the given subject and version. | Reader |
| Test input schema against a particular version of a subject’s schema for compatibility. | Reader |
| Perform a compatibility check on the schema against one or more versions in the subject. | Reader |
| Update global compatibility level. [^tabletext6] |  |
| Get global compatibility level. |  |
| Update compatibility level for the specified subject. | Manager |
| Get compatibility level for a subject. | Reader |
| Deletes the specified subject-level compatibility level config and reverts to the global default. | Manager |
{: caption="Compatibility actions table" caption-side="bottom"}

[^tabletext5]: Reader if the version already exists, Writer if the version is to be created by the API call.
[^tabletext6]: You do not need access to the schema resource, instead Manager access on the cluster resource is required.

## Managing access to the Schema Registry
{: #managing_access_schemas}

The authorization model for the Schema Registry uses the same style of policies that are described in the [Managing authorization to your {{site.data.keyword.messagehub}} resources](#security_resources) section of this document.

### IAM resources
{: #iam_resources}

With the new `schema` IAM resource type, it is possible to create policies that control access by using varying degrees of granularity, as in the following examples.

- A specific schema.
- A set of schemas selected by a wildcard expression.
- All of the schemas stored by an instance of IBM {{site.data.keyword.messagehub}}.
- All of the schemas stored by all of the instances of IBM {{site.data.keyword.messagehub}} in an account.

{{site.data.keyword.messagehub}} already has the concept of a cluster resource type. It is used to control all access to the service instance, with the minimum role of Reader being required to access any Kafka or HTTPS endpoint. This use of the cluster resource type is also applied to the Schema Registry whereby a minimum role of Reader is required to access the registry.

### Example authorization scenarios
{: #example_authorization_scenarios}

The following table describes some examples of scenarios for interacting with the {{site.data.keyword.messagehub}} Schema Registry, together with the roles that are required by the actors involved. The process of managing schemas is handled separately to deploying applications. So policies are required for both the service ID that manages schemas in the registry and the application that connects to the registry.

Scenario | Person or process role | Person or process resource| Application role | Application resource
--- | --- | --- | --- | ---
 New schema versions are placed into the registry by a person or process that is separate from the applications that use the schemas.| `Reader`  \n `Writer`| `cluster`  \n `schema` | `Reader`  \n `Reader` | `cluster`   \n `schema`
Adding a schema to the registry needs to specify a nondefault rule that controls how versions of the schema are allowed to evolve. |`Reader`  \n `Manager` | `cluster`  \n `schema` | Not applicable |  Not applicable
Schemas are managed alongside the application code that uses the schema. New schema versions are created at the point that an application tries to use the new schema version. | Not applicable | Not applicable | `Reader`  \n `Writer`| `cluster`  \n `schema`
The global default rule that controls schema evolution is changed. | `Manager` | `cluster` | Not applicable | Not applicable
{: caption="Examples of authorization scenarios" caption-side="bottom"}
