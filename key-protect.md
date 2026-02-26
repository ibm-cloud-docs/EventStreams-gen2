---
copyright:
  years: 2026
lastupdated: "2026-02-26"

subcollection: event-streams-gen2

keywords: bring your own key, byok, cryptoshredding, key rotation, key rotation frequency, bring your own key

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.keymanagementserviceshort}} integration
{: #key-protect}

[Gen 2]{: tag-purple}

The data that you store in {{site.data.keyword.messagehub}} is encrypted by default by using randomly generated keys. To control the encryption keys, you can Bring Your Own Key (BYOK) through [{{site.data.keyword.keymanagementservicelong_notm}}](/docs/key-protect?topic=key-protect-integrate-services) and use one of your own keys to encrypt the storage volumes of the brokers.

This document covers the integration of {{site.data.keyword.keymanagementserviceshort}} with Gen 2 {{site.data.keyword.messagehub}}. 
{: .note}

To get started, provision [{{site.data.keyword.keymanagementserviceshort}}](https://cloud.ibm.com/catalog/services/key-protect) on your {{site.data.keyword.cloud_notm}} account.

## Creating or adding a key in {{site.data.keyword.keymanagementserviceshort}}
{: #key-create}

Go to your instance of {{site.data.keyword.keymanagementserviceshort}} and [generate or enter a key](/docs/key-protect?topic=key-protect-getting-started-tutorial).

## Granting service authorization in the UI
{: #granting-service-auth}
{: ui}

Authorize {{site.data.keyword.keymanagementserviceshort}} for use with {{site.data.keyword.messagehub}} deployments:

1. Open your {{site.data.keyword.cloud_notm}} dashboard.
2. From the menu bar, click **Manage > Access (IAM)**.
3. In the side navigation, click **Authorizations**.
4. Click **Create**.
5. In the **Source service** menu, select the service of the deployment: **{{site.data.keyword.messagehub}}**.
6. In the **Source service resources** menu, select **All resources**.
7. In the **Target service** menu, select **{{site.data.keyword.keymanagementserviceshort}}**.
8. Select or retain the default value **Account** as the resource group for the **Target Service**
9. In the Target service **Instance ID** menu, select the service instances to authorize.
10. Enable the **Reader** role.
11. To use "Bring your own key" (BYOK), select the **Enable authorizations to be delegated** box in the **Authorize dependent services** section.  
12. Click **Authorize**.

If the service authorization is not present before provisioning your deployment with a key, the provision fails.

If you want to set up a more restrictive authorization policy, configure your policy for an explicit root key CRN or for a specific {{site.data.keyword.keymanagementserviceshort}} instance. Restrictions applied for {{site.data.keyword.keymanagementserviceshort}} key rings are currently not supported.
{: .important}

## Granting service authorization in the CLI
{: #granting-service-auth-cli}
{: cli}

1. Create an authorization policy to allow the {{site.data.keyword.messagehub}} service to access the {{site.data.keyword.keymanagementserviceshort}} service instance on the CLI. For a full set of arguments, see the [IAM CLI reference](/docs/cli?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_authorization_policy_create).

```bash
bx iam authorization-policy-create messagehub kms "Reader,AuthorizationDelegator"
```
{: codeblock}

If the service authorization is not present before provisioning your deployment with a key, the provision fails.

If you want to set up a more restrictive authorization policy, configure your policy for an explicit root key CRN or for a specific {{site.data.keyword.keymanagementserviceshort}} instance. Restrictions applied for {{site.data.keyword.keymanagementserviceshort}} key rings are currently not supported.
{: .important}

## Granting service authorization via the REST API
{: #granting-service-auth-api}
{: api}

1. Create an authorization policy to allow the {{site.data.keyword.messagehub}} service to access the {{site.data.keyword.keymanagementserviceshort}} service instance via the [IAM REST API](#auth_policy_api).

For a full API reference, see  the [IAM Policy Management API](https://cloud.ibm.com/apidocs/iam-policy-management#create-policy).
{: note}

```bash
curl -X POST 'https://iam.cloud.ibm.com/v1/policies' -H "Authorization: Bearer $TOKEN" -H 'Content-Type: application/json' -d '{
  "type": "authorization",
  "subjects": [
      {
          "attributes": [
              {
                  "name": "accountId",
                  "value": "CUSTOMER_ACCOUNT_ID"
              },
              {
                  "name": "serviceName",
                  "value": "messagehub"
              }
          ]
      }
  ],
  "roles": [
      {
        "role_id": "crn:v1:bluemix:public:iam::::serviceRole:Reader"
      },
      {
        "role_id": "crn:v1:bluemix:public:iam::::role:AuthorizationDelegator"
      }
    ],
    "resources": [
        {
            "attributes": [
                {
                    "name": "accountId",
                    "operator": "stringEquals",
                    "value": "CUSTOMER_ACCOUNT_ID"
                },
                {
                    "name": "serviceName",
                    "operator": "stringEquals",
                    "value": "kms"
                }
            ]
        }
    ]
}'
```
{: codeblock}

If the service authorization is not present before provisioning your deployment with a key, the provision fails.

If you want to set up a more restrictive authorization policy, configure your policy for an explicit root key CRN or for a specific {{site.data.keyword.keymanagementserviceshort}} instance. Restrictions applied for {{site.data.keyword.keymanagementserviceshort}} key rings are currently not supported.
{: .important}

## Using the {{site.data.keyword.keymanagementserviceshort}} key
{: #key-using}

After you grant your {{site.data.keyword.messagehub}} deployments permission to use your keys, you supply the [key name or CRN](/docs/key-protect?topic=key-protect-view-keys) when you provision a deployment. The deployment uses your encryption key to encrypt your data.

## Using the {{site.data.keyword.keymanagementserviceshort}} key in the UI at provision
{: #key-using-ui}
{: ui}

If provisioning from the catalog page, select the {{site.data.keyword.keymanagementserviceshort}} instance and key from the dropdown menus.

## Using the {{site.data.keyword.keymanagementserviceshort}} key in the CLI at provision
{: #key-using-cli}
{: cli}

In the CLI, use the `dataservices.encryption.disk` parameter in the parameters JSON object to assign a root key CRN to your service instance.

```bash
ibmcloud resource service-instance-create EVENT-STREAMS-INSTANCE-NAME messagehub enterprise-gen2 REGION -p '{"dataservices":{"encryption":{"disk":"KMS_KEY_CRN"}}}'
```
{: codeblock}

The {{site.data.keyword.keymanagementserviceshort}} key needs to be identified by its full CRN, not just its ID. A {{site.data.keyword.keymanagementserviceshort}} CRN is in the format `crn:v1:<...>:key:<id>`.
{: .tip}

## Using the {{site.data.keyword.keymanagementserviceshort}} key in the API at provision
{: #key-using-api}
{: api}

In the API, use the `dataservices.encryption.disk` parameter in the body of the request.

```sh
  curl -X POST https://resource-controller.cloud.ibm.com/v2/resource_instances -H "Authorization: Bearer <IAM token>" -H 'Content-Type: application/json' -d '{
    "name": "my-instance",
    "target": "ca-mon",
    "resource_group": "A RESOURCE GROUP GUID",
    "resource_plan_id": "eventstreams-gen2-enterprise",
    "parameters": {
      "dataservices": {
          "encryption": {
            "disk": "KMS_KEY_CRN"
          }
      }
    }
```
{: codeblock}

The {{site.data.keyword.keymanagementserviceshort}} key needs to be identified by its full CRN, not just its ID. A {{site.data.keyword.keymanagementserviceshort}} CRN is in the format `crn:v1:<...>:key:<id>`.
{: .tip}

## Key rotation
{: #keyrotation}

{{site.data.keyword.keymanagementserviceshort}} offers manual and automatic [key rotation](/docs/key-protect?topic=key-protect-rotate-keys) and key rotation is supported by {{site.data.keyword.messagehub}} deployments.

For more information, see [Rotating manually or automatically](/docs/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options).

## Deleting the deployment
{: #key-protect-deleting-deployment}

If you delete a deployment that is protected with a {{site.data.keyword.keymanagementserviceshort}} key, the deployment remains registered against the key during the soft-deletion period (up to 9 days). To delete the key in the soft-deletion period, [force delete](/docs/key-protect?topic=key-protect-delete-keys) the key. After the soft-deletion period, the key can be deleted without the force. To determine when you can delete the key, check the [association between the key and your deployment](/docs/key-protect?topic=key-protect-view-protected-resources).

## Cryptoshredding
{: #crypto-shredding}

Cryptoshredding is a destructive action. When the key is deleted, your data is unrecoverable.
{: .important}

{{site.data.keyword.keymanagementserviceshort}} allows you to [initiate a force delete](/docs/key-protect?topic=key-protect-delete-keys) of a key that is in use by {{site.data.keyword.cloud}} services, including your Gen 2 {{site.data.keyword.messagehub}} deployments. This action is called cryptoshredding. Deleting a key that is in use on your deployment locks the disks that contain your data and disables your deployment. You are still able to access the UI and some metadata such as security settings in the UI, CLI, and API but you are not able to access any of the topics/partitions or data that is contained within them. Key deletion is [sent to the {{site.data.keyword.atracker_short}}](/docs/key-protect?topic=key-protect-at-events) as `kms.secrets.delete`.

### Granting the delegation authorization
{: #grant-auth}

To enable your deployment to use the {{site.data.keyword.keymanagementserviceshort}} key, you need to [Enable authorization to be delegated](/docs/account?topic=account-serviceauth) when granting the service authorizations. If the delegation authorization is not present before provisioning your deployment with a key, the provision fails.

After you enable delegation and provisioned your deployment, two entries appear in your _Authorizations_ in IAM. One is the entry for the deployment that lists its status as delegator. It is "User Created".

| Role | Source | Target | Type |
| -----|-----|-----|----- |
| AuthorizationDelegator, Reader | `event-streams` service | {{site.data.keyword.keymanagementserviceshort}} service | User defined |
{: caption="Example delegator {{site.data.keyword.keymanagementserviceshort}} Authorization" caption-side="bottom"}

And one for the {{site.data.keyword.block_storage_is_full}} volume, where the deployment is the initiator.

| Role | Source | Target | Type |
| -----|-----|-----|----- |
| Reader | {{site.data.keyword.block_storage_is_short}} service | {{site.data.keyword.keymanagementserviceshort}} Service | Created by `event-streams` |
{: caption="Example {{site.data.keyword.keymanagementserviceshort}} authorization for {{site.data.keyword.block_storage_is_short}} and {{site.data.keyword.cos_full}} from {{site.data.keyword.messagehub}}" caption-side="bottom"}

### Removing keys
{: #key-remove}

IAM/{{site.data.keyword.keymanagementserviceshort}} does not stop you from removing the policy between the key and {{site.data.keyword.block_storage_is_short}} (the second example), but doing so can make your topics/partitions unrestorable.

### Common pitfalls
{: #common-pitfalls}

Be careful when removing keys and authorizations. If you have multiple deployments that use the same keys, it is possible to inadvertently destroy data to **all** of those deployments by revoking the delegation authorization. If possible, do not use the same key for multiple deployments.

If you want to shred the data associated with your instance, you can delete the key. {{site.data.keyword.block_storage_is_short}} ensures that the storage is unreadable and unwriteable. However, any other deployments that use that same key will also encounter failures.

If you delete the {{site.data.keyword.block_storage_is_short}} volume authorization (as seen in Table 2), not only the deployment that is shown as the creator is affected, but any deployments that also use the same key are affected as well. Those deployments will encounter topic/partition failures until you open a support ticket and request the policy to be re-created.

Use caution if you reuse keys.
{: .important}
