---
copyright:
  years: 2025
lastupdated: "2025-12-03"

subcollection: event-streams-gen2

keywords: bring your own key, byok, cryptoshredding, key rotation, key rotation frequency, bring your own key

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.keymanagementserviceshort}} integration
{: #key-protect}

[Gen 2]{: tag-purple}

{{site.data.keyword.messagehub}} Gen 2 is currently in Beta and does not support {{site.data.keyword.keymanagementservicelong}} integration. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see [Beta reference](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-icd-gen2-beta).
{: beta}

The data that you store in {{site.data.keyword.messagehub}} is encrypted by default by using randomly generated keys. To control the encryption keys, you can Bring Your Own Key (BYOK) through [{{site.data.keyword.keymanagementservicelong_notm}}](/docs/key-protect?topic=key-protect-integrate-services) and use one of your own keys to encrypt your databases and backups.

This document covers the integration of {{site.data.keyword.keymanagementserviceshort}} with Gen 2 {{site.data.keyword.messagehub}}, which includes {{site.data.keyword.databases-for-postgresql}} and {{site.data.keyword.databases-for-mongodb}}. 
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
2. From the menu bar, click **Manage** -> **Access (IAM)**.
3. In the side navigation, click **Authorizations**.
4. Click **Create**.
5. In the **Source service** menu, select the service of the deployment. For example, **Databases for PostgreSQL** or **Databases for MongoDB**.
6. In the **Source service resources** menu, select **All resources**.
7. In the **Target service** menu, select **{{site.data.keyword.keymanagementserviceshort}}**.
8. Select or retain the default value **Account** as the resource group for the **Target Service**
9. In the Target service **Instance ID** menu, select the service instances to authorize.
10. Enable the **Reader** role.
11. To use "Bring your own key" (BYOK) for backups, Select the **Enable authorizations to be delegated** box in the **Authorize dependent services** section.  
12. Click **Authorize**.

If the service authorization is not present before provisioning your deployment with a key, the provision fails.

## Using the {{site.data.keyword.keymanagementserviceshort}} key
{: #key-using}

After you grant your {{site.data.keyword.messagehub}} deployments permission to use your keys, you supply the [key name or CRN](/docs/key-protect?topic=key-protect-view-keys) when you provision a deployment. The deployment uses your encryption key to encrypt your data.

## Using the {{site.data.keyword.keymanagementserviceshort}} key in the UI
{: #key-using-ui}
{: ui}

If provisioning from the catalog page, select the {{site.data.keyword.keymanagementserviceshort}} instance and key from the dropdown menus.

## Using the {{site.data.keyword.keymanagementserviceshort}} key in the CLI
{: #key-using-cli}
{: cli}

In the CLI, use the `disk_encryption_key_crn` parameter in the parameters JSON object.
```sh
ibmcloud resource service-instance-create <INSTANCE_NAME> <SERVICE-NAME> standard us-south \
-p \ '{
  "disk_encryption_key_crn": "crn:v1:<...>:key:<id>"
}'
```

The {{site.data.keyword.keymanagementserviceshort}} key needs to be identified by its full CRN, not just its ID. A {{site.data.keyword.keymanagementserviceshort}} CRN is in the format `crn:v1:<...>:key:<id>`.
{: .tip}

## Using the {{site.data.keyword.keymanagementserviceshort}} key in the API
{: #key-using-api}
{: api}

In the API, use the `disk_encryption_key` parameter in the body of the request.
```sh
curl -X POST \
  https://resource-controller.cloud.ibm.com/v2/resource_instances \
  -H 'Authorization: Bearer <>' \
  -H 'Content-Type: application/json' \
    -d '{
    "name": "my-instance",
    "target": "blue-ca-mon",
    "resource_group": "5g9f447903254bb58972a2f3f5a4c711",
    "resource_plan_id": "databases-for-x-gen2-standard",
    "disk_encryption_key_crn": "crn:v1:<...>:key:<id>"
  }'
```

The {{site.data.keyword.keymanagementserviceshort}} key needs to be identified by its full CRN, not just its ID. A {{site.data.keyword.keymanagementserviceshort}} CRN is in the format `crn:v1:<...>:key:<id>`.
{: .tip}

## Key rotation
{: #keyrotation}

{{site.data.keyword.keymanagementserviceshort}} offers manual and automatic [key rotation](/docs/key-protect?topic=key-protect-rotate-keys) and key rotation is supported by {{site.data.keyword.messagehub}} deployments. When you rotate a key, the process initiates a _syncing KMS state_ task, and your deployment is reencrypted with the new key. The task is displayed on the _Tasks_ page on your deployment's _Overview_ and the associated {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.messagehub}} events are sent to Activity Tracker.

For more information, see [Rotating manually or automatically](/docs/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options).

## Deleting the deployment
{: #key-protect-deleting-deployment}

If you delete a deployment that is protected with a {{site.data.keyword.keymanagementserviceshort}} key, the deployment remains registered against the key during the soft-deletion period (up to 9 days). To delete the key in the soft-deletion period, [force delete](/docs/key-protect?topic=key-protect-delete-keys) the key. After the soft-deletion period, the key can be deleted without the force. To determine when you can delete the key, check the [association between the key and your deployment](/docs/key-protect?topic=key-protect-view-protected-resources).

## Cryptoshredding
{: #crypto-shredding}

Cryptoshredding is a destructive action. When the key is deleted, your data is unrecoverable.
{: .important}

{{site.data.keyword.keymanagementserviceshort}} allows you to [initiate a force delete](/docs/key-protect?topic=key-protect-delete-keys) of a key that is in use by {{site.data.keyword.cloud}} services, including your Gen 2 {{site.data.keyword.messagehub}} deployments. This action is called cryptoshredding. Deleting a key that is in use on your deployment locks the disks that contain your data and disables your deployment. You are still able to access the UI and some metadata such as security settings in the UI, CLI, and API but you are not able to access any of the databases or data that is contained within them. Key deletion is [sent to the {{site.data.keyword.atracker_short}}](/docs/key-protect?topic=key-protect-at-events) as `kms.secrets.delete`.

## Bring your own key for backups
{: #key-byok}

If you use {{site.data.keyword.keymanagementserviceshort}}, when you provision a database you can also designate a key to encrypt the {{site.data.keyword.block_storage_is_full}} disk that holds your deployment's backups. 

BYOK for backups is available only in select regions. 
{: .note}

Only keys in the select regions are durable to region failures. To ensure that your backups are available even if a region failure occurs, you must use a key from these select regions, regardless of your deployment's location.
{: .important}

### Granting the delegation authorization
{: #grant-auth}

To enable your deployment to use the {{site.data.keyword.keymanagementserviceshort}} key, you need to [Enable authorization to be delegated](/docs/account?topic=account-serviceauth) when granting the service authorizations. If the delegation authorization is not present before provisioning your deployment with a key, the provision fails.

### Using the key at provision in the CLI
{: #key-provision-cli}
{: cli}

After the appropriate authorization and delegation is granted, you supply the [key name or CRN](/docs/key-protect?topic=key-protect-view-keys) when you provision a deployment.

In the CLI, use the `backup_encryption_key_crn` parameter in the parameters JSON object.
```sh
ibmcloud resource service-instance-create <INSTANCE_NAME> <SERVICE-NAME> standard us-south \
-p \ '{
  "backup_encryption_key_crn": "crn:v1:<...>:key:<id>"
}'
```

### Using the key at provision in the API
{: #key-provision-api}
{: api}

In the API, use the `backup_encryption_key_crn` parameter in the body of the request.
```sh
curl -X POST \
  https://resource-controller.cloud.ibm.com/v2/resource_instances \
  -H 'Authorization: Bearer <>' \
  -H 'Content-Type: application/json' \
    -d '{
    "name": "my-instance",
    "target": "blue-ca-mon",
    "resource_group": "5g9f447903254bb58972a2f3f5a4c711",
    "resource_plan_id": "databases-for-x-gen2-standard",
    "backup_encryption_key_crn": "crn:v1:<...>:key:<id>"
  }'
```

After you enable delegation and provisioned your deployment, two entries appear in your _Authorizations_ in IAM. One is the entry for the deployment that lists its status as delegator. It is "User Created".

| Role | Source | Target | Type |
| -----|-----|-----|----- |
| AuthorizationDelegator, Reader | `<cloud-databases>` Service | {{site.data.keyword.keymanagementserviceshort}} Service | User defined |
{: caption="Example delegator {{site.data.keyword.keymanagementserviceshort}} Authorization " caption-side="bottom"}

And one for the {{site.data.keyword.block_storage_is_short}} volume for its backups, where the deployment is the initiator.

| Role | Source | Target | Type |
| -----|-----|-----|----- |
| Reader | {{site.data.keyword.block_storage_is_short}} service | {{site.data.keyword.keymanagementserviceshort}} Service | Created by `<cloud-databases-crn>` |
{: caption="Example {{site.data.keyword.keymanagementserviceshort}} authorization for Cloud Object Storage from {{site.data.keyword.messagehub}} " caption-side="bottom"}

### Removing keys
{: #key-remove}

IAM/{{site.data.keyword.keymanagementserviceshort}} does not stop you from removing the policy between the key and {{site.data.keyword.block_storage_is_short}} (the second example), but doing so can make your backups unrestorable. To prevent this, if you delete the {{site.data.keyword.block_storage_is_short}} policy that governs the ability of {{site.data.keyword.messagehub}} to use the key for {{site.data.keyword.block_storage_is_short}}, the policy is re-created to continue backing up your deployment.

Be careful when removing keys and authorizations. If you have multiple deployments that use the same keys, it is possible to inadvertently destroy backups to **all** of those deployments by revoking the delegation authorization. If possible, do not use the same key for multiple deployment's backups.

If you want to shred the backups, you can delete the key. {{site.data.keyword.block_storage_is_short}} ensures that the storage is unreadable and unwriteable. However, any other deployments that use that same key for backups encounter subsequent backup failures.

If you do require that the same key to be used for multiple deployment's backups, removing keys and authorizations can have the following side effects.
- If you delete just the {{site.data.keyword.block_storage_is_short}} volume authorization (as seen in Table 2), then not only is the deployment that is shown as the creator affected, but any deployments that also use the same key are also affected. Those deployments can encounter temporary backup failures until the policy is automatically re-created. There should be no lasting effects, except for missing backups.
- If you delete just {{site.data.keyword.messagehub}} delegator authorization, which is created by you (as seen in Table 1), nothing immediately breaks because the second authorization is still in place. However, if the {{site.data.keyword.block_storage_is_short}} authorization is ever removed, it cannot be re-created, and can lead to multiple deployments that use the same key losing the ability to back up.
- If you delete both the {{site.data.keyword.block_storage_is_short}} authorization **AND** the {{site.data.keyword.messagehub}} delegator authorization, all deployments that use the same key will immediately not have the ability to back up and the correct authorizations will not be able to be re-created, effectively destroying the backups for all deployments that use that key.

Use caution if you reuse keys.
{: .important}
