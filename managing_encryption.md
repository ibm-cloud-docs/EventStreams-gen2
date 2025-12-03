---

copyright:
  years: 2015, 2025
lastupdated: "2025-12-03" 

keywords: BYOK, encryption, customer-managed encryption, customer-managed key, access to data, rotating key, rotate key

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}
{:ui: .ph data-hd-interface="ui"}
{:cli: .ph data-hd-interface="cli"}
{:api: .ph data-hd-interface="api"}

# Managing encryption in {{site.data.keyword.messagehub}}
{: #managing_encryption}

By default, message payload data in {{site.data.keyword.messagehub_full}} is encrypted at rest by using a randomly generated key. Although this default encryption model provides at-rest security, you might need a higher level of control. For these use cases, {{site.data.keyword.messagehub}} supports customer-managed encryption with the following {{site.data.keyword.cloud}} key management services:

- {{site.data.keyword.keymanagementservicefull}} (Bring Your Own Key - BYOK) helps you provision encrypted keys for apps across {{site.data.keyword.cloud_notm}} services. As you manage the lifecycle of your keys, you benefit from knowing that your keys are secured by FIPS 140-2 Level three certified cloud-based hardware security modules (HSMs) that protect against the theft of information. For more information about using {{site.data.keyword.keymanagementserviceshort}}, see the [Getting Started tutorial](/docs/key-protect?topic=key-protect-getting-started-tutorial){: external}. {{site.data.keyword.keymanagementservicefull}} is supported by both the Enterprise and Enterprise Gen2 plans.

// TODO ANDREA: Help me how to suggest to the customer that if they want KYOK, they have to do Enterprise, they can't do it on Gen2
// TODO ANDREA: the below section may need the removal of hscrypto for the gen2 specific docs only as we don't support hs-crypto

- {{site.data.keyword.hscrypto}} (Keep Your Own Key - KYOK) is a single-tenant, dedicated HSM that is controlled by you. The service is built on FIPS 140-2 Level 4-certified hardware, the highest offered by any cloud provider in the industry. For more information about using {{site.data.keyword.hscrypto}}, see the [Getting Started tutorial](/docs/hs-crypto?topic=hs-crypto-get-started){: external}. {{site.data.keyword.hscrypto}} is only supported by the Enterprise plan.

These services allow the use of a customer-provided key to control encryption. By disabling or deleting this key, you can prevent any further access to the data that is stored by the service, because it is no longer possible to decrypt it.
{: shortdesc}

Consider to use customer-managed keys, if you require the following features:
{: #considerations_keys}
{: notoc}

- Encryption of data at rest controlled by your own key.
- Explicit control of the lifecycle of data that is stored at rest.

Customer-managed keys are available on the Enterprise and Enterprise Gen2 plans only.
{: note}

Deletion of the customer-managed key is unrecoverable and results in the loss of any data that is stored in your {{site.data.keyword.messagehub}} instance.
{: important}

## Message-level encryption for compliance
{: #message_level_encryption_}

Some regulatory standards, such as [PCI DSS](/docs/EventStreams?topic=EventStreams-compliance), require that sensitive messsage data (such as cardholder information) is encrypted throughout its lifecycle - at rest, in motion, and in use. 

The file systems used by {{site.data.keyword.messagehub}} to store message data are encrypted, protecting against unauthorized access. However, as messages pass through the transport layer, {{site.data.keyword.messagehub}} must decrypt these file systems to process the messages. While this means that message payloads are not encrypted end-to-end, encryption in transit and at rest ensures strong protection throughout the message lifecycle, in line with industry best practices.

To ensure that sensitive data remains unreadable to anyone other than the intended recipients, clients must encrypt the message itself at the application level before sending it to {{site.data.keyword.messagehub}} - a practice known as message-level encryption.

## What is not covered by customer-managed encryption
{: #encryption_what}

If customer-managed encryption feature is selected, be aware that **only** message payload data is covered by this encryption. {{site.data.keyword.messagehub}} encrypts at rest other data that is related to the use of the service. However, although encrypted, non-message payload data **is not** encrypted with the customer-managed encryption. Examples are client metadata such as topic names, topic configuration data, schemas stored in the schema registry and metadata that is stored in relation to the configuration of the Enterprise or Enterprise Gen2 instance.

Therefore, do not use confidential information in such client metadata.
{: important}

## How customer-managed encryption works
{: #encryption_how}

// TODO ANDREA: The technology we're using for encryption is slightly different in between Enterprise and Gen2. The end result is roughly the same. In Enterprise, effectively WE managed the encryption key unwrapping/disk decryption. In Gen2 however, we only manage CONFIGURATION but it is IBM Cloud Block Storage who actually manages the wrap/unwrap (decrypt) operation. So we could have an option here to link out to Block Storage's encryption docs but I chose NOT to modify this section as this information feels like _internals_ to me. On this basis, do you agree that the below section is fine 'as is'?

{{site.data.keyword.messagehub}} uses a concept that is called envelope encryption to implement customer-managed keys.

Envelope encryption is the practice of encrypting one encryption key with another encryption key. The key used to encrypt the actual data is known as a data encryption key (DEK). The DEK itself is never stored, but instead is wrapped by a second key that is known as the key encryption key (KEK) to create a wrapped DEK. 

To decrypt data, the wrapped DEK must first be unwrapped to get the DEK. This process is possible only by accessing the KEK, which in this case is your root key that is stored in either [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-about){: external} or [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-overview){: external}. 

// TODO ANDREA: the below section may need the removal of hscrypto for the gen2 specific docs only as we don't support hs-crypto

You own the KEK, which you create as a root key in the {{site.data.keyword.hscrypto}} or {{site.data.keyword.keymanagementserviceshort}} service. The {{site.data.keyword.messagehub}} service never sees the root (KEK) key. Its storage, management, and use to wrap and unwrap the DEK is performed entirely within the key management service. If you disable or delete the key, the data can no longer be decrypted.

## Enabling a customer-managed key for {{site.data.keyword.messagehub}} Enterprise Gen2 Plan Only
{: #enabling_encryption}

// TODO ANDREA I do not know if splitting up the steps into expanded sections is a sensible idea or not. Each step is very short and may not require an expanded explanation. Do you think we should do these 'short and sweet' and so not to link out to say #provision_kms_ui ; etc? What do you think?

Complete the following steps to provision your BYOK encrypted {{site.data.keyword.messagehub}} instance to use a customer-managed key on the ui: {: ui}

* [Step 1: Provision an {{site.data.keyword.keymanagementservicefull}} instance using the console](#provision_kms_ui)
* [Step 2: Create or import a root key in your {{site.data.keyword.keymanagementservicefull}} service instance.](#create_crk_ui)
* [Step 3: Create an authorization policy to allow the {{site.data.keyword.messagehub}} service to access the {{site.data.keyword.keymanagementservicefull}} service instance.](#auth_policy_ui)
* [Step 4: Provision a {{site.data.keyword.messagehub}} Enterprise Gen2 instance using the console while selecting the Encryption configuration](#provision_instance_ui)
{: ui}

Complete the following steps to provision your BYOK encrypted {{site.data.keyword.messagehub}} instance to use a customer-managed key on the cli: {: cli}

* [Step 1: Provision an {{site.data.keyword.keymanagementservicefull}} instance using the cli](#provision_kms_cli)
* [Step 2: Create or import a root key in your {{site.data.keyword.keymanagementservicefull}} service instance using the cli.](#create_crk_cli)
* [Step 3: Create an authorization policy to allow the {{site.data.keyword.messagehub}} service to access the {{site.data.keyword.keymanagementservicefull}} service instance on the cli](#auth_policy_cli)
* [Step 4: Provision a {{site.data.keyword.messagehub}} Enterprise Gen2 instance using the cli while specifying encryption provision parameters](#provision_instance_cli)
{: cli}

Complete the following steps to provision your BYOK encrypted {{site.data.keyword.messagehub}} instance to use a customer-managed key via REST APIs: {: api}

* [Step 1: Provision an {{site.data.keyword.keymanagementservicefull}} instance using the Resource Controller REST API](#provision_kms_api)
* [Step 2: Create or import a root key in your {{site.data.keyword.keymanagementservicefull}} service instance using the {{site.data.keyword.keymanagementservicefull}} REST API.](#create_crk_api)
* [Step 3: Create an authorization policy to allow the {{site.data.keyword.messagehub}} service to access the {{site.data.keyword.keymanagementservicefull}} service instance via the IAM REST API](#auth_policy_api)
* [Step 4: Provision a {{site.data.keyword.messagehub}} Enterprise Gen2 instance using the Resource Controller REST API, while providing the encryption key CRN](#provision_instance_api)
{: api}


An existing cluster without a customer-managed key cannot be updated because this is a destructive operation that will result in the loss of all message and topic definitions. You must delete and recreate your instance with an encryption key.
{: important}

// TODO ANDREA: The steps here are for EVS specifically BUT they are 'the same' for the other CDP gen2 services too. Customer needs to manage Keyprotect --> this is the same. Customer needs to then configure IAM policies. Lastly, customer provisions CDP service with 'encryption' enabled. Is this enough info for you to update/refactor PG and Mongo docs or do you not have enough bandwidth to do this? I'm asking as "someone" has to own this. This may be me, may be you, I don't know but for now, I focused on EVS only.

// TODO ANDREA: Wherever possible, instead of documenting the explicit steps myself, I linked out to other teams' documentation from my sections. I think this is more future proof as if their documentation is updated, the user automatically gets the most up-to-date instructions. We don't want to duplicate archaic steps for example for 'installing the Keyprotect CLI plugin'. Am I right here? Is this useful?

// TODO ANDREA FYI: UI section follows
## Step 1: Provision an {{site.data.keyword.keymanagementservicefull}} instance using the console](#provision_kms_ui)
{: #provision_kms_ui}
{: ui}

1. Provision a {{site.data.keyword.keymanagementservicefull}} instance by following [their documentation](https://cloud.ibm.com/docs/key-protect?topic=key-protect-provision#provision-gui)

## Step 2: Create or import a root key in your {{site.data.keyword.keymanagementservicefull}} service instance.](#create_crk_ui)
{: #create_crk_ui}
{: ui}

1. [Create]((https://cloud.ibm.com/docs/key-protect?topic=key-protect-create-root-keys&interface=ui)) or [import](https://cloud.ibm.com/docs/key-protect?topic=key-protect-import-root-keys&interface=ui) your desired root key

## Step 3: Create an authorization policy to allow the {{site.data.keyword.messagehub}} service to access the {{site.data.keyword.keymanagementservicefull}} service instance.](#auth_policy_ui)
{: #auth_policy_ui}
{: ui}

1. Create an authorization policy to allow the {{site.data.keyword.messagehub}} service to access the {{site.data.keyword.keymanagementservicefull}} service instance as a Reader. Ensure that you select the "Enable authorization to be delegated by source and dependent services" check-box. For more information, see [Using authorizations to grant access between services](/docs/account?topic=account-serviceauth){: external}

If you do not see the "Enable authorization to be delegated by source and dependent services" check box then your user may lack the required access privileges to configure delegated authorization policies.
{: note}
{: ui}

## Step 4: Provision a {{site.data.keyword.messagehub}} Enterprise Gen2 instance using the console while selecting the Encryption configuration](#provision_instance_ui)
{: #provision_instance_ui}
{: ui}

1. Select a location. The Enterprise Gen2 plan may not be available in all locations.
2. Configure the resource  by providing a Service name, Resource group and optionally, Tag(s).
3. Specify Service configuration. Select your desired {{site.data.keyword.keymanagementservicefull}} instance and select a root key that you desire to be used for disk encryption purposes.
4. Configure remaining Service configuration items according to your needs.
5. Press "Create".
6. The service instance is now being created for you asynchonously.


// TODO ANDREA FYI: CLI section follows

## Pre-req: Set up the CLI
{: cli}

1. Install the Key Protect CLI plug-in by [following their documentation](https://cloud.ibm.com/docs/key-protect?topic=key-protect-set-up-cli#install-cli).

## Step 1: Provision an {{site.data.keyword.keymanagementservicefull}} instance using the cli](#provision_kms_cli)
{: #provision_kms_cli}
{: cli}

1. Provision a {{site.data.keyword.keymanagementservicefull}} instance by following [their documentation](https://cloud.ibm.com/docs/key-protect?topic=key-protect-provision#provision-cli)

## Step 2: Create or import a root key in your {{site.data.keyword.keymanagementservicefull}} service instance using the cli.](#create_crk_cli)
{: #provision_kms_cli}
{: cli}

1. [Create a new root key](https://cloud.ibm.com/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-key-create-example-2)

Make sure you provide the --output json argument to the key create command as per the example. This enables you to see the CRN of the root key you just created.
{: important}
{: cli}

2. Note down the CRN of the root key created

## Step 3: Create an authorization policy to allow the {{site.data.keyword.messagehub}} service to access the {{site.data.keyword.keymanagementservicefull}} service instance on the cli](#auth_policy_cli)
{: #provision_kms_cli}
{: cli}

1. Create an authorization policy to allow the {{site.data.keyword.messagehub}} service to access the {{site.data.keyword.keymanagementservicefull}} service instance on the cli. [Please refer to the IAM CLI reference for a full set of arguments](https://cloud.ibm.com/docs/cli?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_authorization_policy_create).

```bash
bx iam authorization-policy-create messagehub kms "Reader,AuthorizationDelegator"
```
{: codeblock}



## Step 4: Provision a {{site.data.keyword.messagehub}} Enterprise Gen2 instance using the cli while specifying encryption provision parameters](#provision_instance_cli)
{: #provision_kms_cli}
{: cli}

// TODO ANDREA almost all IBM docs forget this nugget of info, the formatting almost certainly is broken. Shall this be step 1 and the next one step 2?
Don't forget to target a resource group:
```bash
ibmcloud target -g YOUR_RESOURCE_GROUP
```
{: codeblock}
{: note}
{: cli}

```bash
ibmcloud resource service-instance-create EVENT-STREAMS-INSTANCE-NAME messagehub enterprise-gen2 REGION -p '{"dataservices":{"encryption":{"disk":"KMS_KEY_CRN"}}}'
```
{: codeblock}


// TODO ANDREA FYI: API section follows

### Using APIs
{: api}

You can use multiple APIs to work with {{site.data.keyword.messagehub}}. This tutorial uses the following APIs:

* IAM Identify Services API to [obtain a bearer token](https://cloud.ibm.com/apidocs/iam-identity-token-api#gettoken-apikey)
* IAM Policy Management API to [create an access policy](https://cloud.ibm.com/apidocs/iam-policy-management#create-policy)
* The resource controller API to [provision service instances](https://cloud.ibm.com/apidocs/resource-controller/resource-controller#create-resource-instance)
* The KeyProtect API to [create](https://cloud.ibm.com/apidocs/key-protect#createkey) or [import](https://cloud.ibm.com/apidocs/key-protect#postimporttoken) a root key


// TODO Andrea is this note helpful? No IBM doc ever mentions it but can we assume customers 'know this'?

For SDKs provided for supported languages, you will find links to the programming language of your choice by finding relevant examples for your language in the API documentations above.
{: note}

### Steps to follow
{: api}

## Pre-req: Obtain an IBM Cloud Identity and Access Management (IAM) access token.

1. Refer to https://cloud.ibm.com/apidocs/iam-identity-token-api#gettoken-apikey

## Step 1: Provision an {{site.data.keyword.keymanagementservicefull}} instance using the Resource Controller REST API]{: #provision_kms_api}
{: api}

1. Refer to https://cloud.ibm.com/apidocs/resource-controller/resource-controller#create-resource-instance



## Step 2: Create or import a root key in your {{site.data.keyword.keymanagementservicefull}} service instance using the {{site.data.keyword.keymanagementservicefull}} REST API.](#create_crk_api)
#create_crk_api}
{: api}

// TODO ZB: I never imported a key, I have not got a cryptographic key generated by a HSM, I'm not an enterprise customer
// TODO ANDREA: Given above, shall we ignore key imports alltogether?

1. To create a key, refer to the [create a key](https://cloud.ibm.com/apidocs/key-protect#createkey) page of the {{site.data.keyword.keymanagementservicefull}} API Documentation. To import a key, refer to [import tokens](https://cloud.ibm.com/apidocs/key-protect#postimporttoken)

Make sure you note down the CRN of the root key that has just been created by capturing the response from the {{site.data.keyword.keymanagementservicefull}} API.
{: important}
{: cli}

## Step 3: Create an authorization policy to allow the {{site.data.keyword.messagehub}} service to access the {{site.data.keyword.keymanagementservicefull}} service instance via the IAM REST API](#auth_policy_api)
#auth_policy_api}
{: api}

1. Refer to https://cloud.ibm.com/apidocs/iam-policy-management#create-policy

Example command (may be too specific, you know what you're doing when you're calling APIs):

curl -X POST 'https://iam.cloud.ibm.com/v1/policies' -H 'Authorization: Bearer $TOKEN' -H 'Content-Type: application/json' -d '{
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

## Step 4: Provision a {{site.data.keyword.messagehub}} Enterprise Gen2 instance using the Resource Controller REST API, while providing the encryption key CRN](#provision_instance_api)
#provision_instance_api}
{: api}

1. Refer to https://cloud.ibm.com/apidocs/resource-controller/resource-controller#create-resource-instance

Example command (may be worth providing to allow seeing how the provision parameter passthrough works for the broker):

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
  }'
