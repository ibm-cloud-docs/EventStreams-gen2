---

copyright:
  years: 2015, 2023
lastupdated: "2023-11-20"

keywords: replication, failover, scenario, disaster recovery, mirroring, setup, backup, geo-replication, bindings, disable

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Disabling mirroring
{: #mirroring_disable}

This information describes how to disable a {{site.data.keyword.messagehub}} mirroring Enterprise cluster.

Currently, disabling mirroring for an {{site.data.keyword.messagehub}} service instance requires the use of the {{site.data.keyword.Bluemix_notm}} CLI.

To install the CLI, see [Extending IBM Cloud CLI with plug-ins](/docs/cli?topic=cli-plug-ins).

The {{site.data.keyword.Bluemix_notm}} CLI uses the **service-instance-update** command to update your {{site.data.keyword.messagehub}} service instance resource. The user ID in the account used to run the **service-instance-update** command must be assigned the same access policies that are needed when you create resources. For information about access requirements, see [Required access for creating resources](/docs/account?topic=account-manage_resource#creating-resources).

The time required to disable mirroring for the {{site.data.keyword.messagehub}} service instance varies, but under normal circumstances it does not exceed 2 hours.

## Step 1: Disable mirroring
{: #step1_command}


To disable mirroring, you need to run a **service-instance-update** command against your target cluster by using the CLI. Run the following command:

```sh
ibmcloud resource service-instance-update "Event Streams resource instance name" -p '{"mirroring":{"disable":true}}'
```
{: codeblock}

If the cluster is provisioned with or scaled up to a throughput higher than the default value of 150, the **service-instance-update** command must also include "throughput":"_current throughput value_" in the update parameter body.
{: note}

## Step 2: Validation
{: #step2_validation}

You can get the current service instance information by running the following command:

```sh
ibmcloud resource service-instance "Event Streams resource instance name" --output=json
```
{: codeblock}

Review the **last operation** section of the output. The information is continuously updated as the update proceeds. When the mirroring disablement process has completed, the last operation information indicates whether the update succeeded or the sync succeeded.

```sh
"last_operation": {
  "type": "update",
  "state": "in progress",
  "description": "Update in progress.",
  "updated_at": null,
  "cancelable": false
}
```
{: screen}

Run the command again until success is indicated as follows:

```sh
"last_operation": {
  "type": "update",
  "state": "succeeded",
  "description": "Update succeeded.",
  "updated_at": null,
  "cancelable": false
}
```
{: screen}

The {{site.data.keyword.mon_full_notm}} dashboard **{{site.data.keyword.messagehub}} Mirroring** shows the state of mirroring.
