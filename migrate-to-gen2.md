---

copyright:
  years: 2026
lastupdated: "2026-10-19"

keywords: event streams migration, gen1 to gen2, classic infrastructure, vpc, drain and switch, migration hub, private endpoints, kafka, topics, producers, consumers

subcollection: event-streams-gen2

content-type: tutorial

---

{{site.data.keyword.attribute-definition-list}}

# Migrating {{site.data.keyword.messagehub}} from Gen 1 to Gen 2
{: #migrating-event-streams-gen1-to-gen2}

[Gen 2]{: tag-purple}

Use this tutorial to migrate your {{site.data.keyword.messagehub}} deployment from Gen 1 (Classic) to Gen 2 (VPC) by using the Migration hub in the {{site.data.keyword.cloud_notm}} console.
{: shortdesc}

This tutorial applies to both the Standard and Enterprise plans. Both plans follow the same drain and switch migration process.

The Migration hub guides you through provisioning the Gen 2 instance and setting up network access. The drain and switch cutover — stopping producers, draining consumers, and restarting on Gen 2 — requires coordinated action across all application teams. Plan the cutover window carefully before you start.

## Key considerations before you begin
{: #key-considerations}

Review these points before you start the migration wizard.

### Drain and switch overview
{: #drain-and-switch-explained}

Drain and switch is a coordinated cutover approach for message streaming workloads:

1. The Gen 2 instance is provisioned with matching topic configurations.
2. Producers are stopped, halting new message writes to Gen 1.
3. Consumers process all remaining messages from Gen 1 until consumer group lag reaches zero.
4. Producer and consumer connection strings are updated to point to Gen 2.
5. Message flow resumes on Gen 2.

There is a planned downtime window during steps 2 through 4. Duration depends on how quickly your consumers drain the remaining Gen 1 messages and how long your application deployments take to restart with the new connection details.

### Topic configuration
{: #topic-config-responsibility}

The Migration hub provisions the Gen 2 instance and assists with network setup. Topic names, partition counts, retention policies, replication factors, and any custom topic configurations must be recreated on the Gen 2 instance before the cutover begins. The wizard does not replicate topic configurations automatically.

Recreate all topic configurations on Gen 2 and validate them before stopping producers on Gen 1.
{: important}

### Migration risks
{: #migration-risks}

Complete each stage of the drain and switch in order to avoid the following risks:

Message loss
:   Stopping producers before consumers have fully drained Gen 1 may result in unprocessed messages not being processed.

Duplicate processing
:   If consumer offsets are not correctly initialized on Gen 2, messages may be processed more than once.

Rollback difficulty
:   After consumers have drained Gen 1 messages, rolling back to Gen 1 is not straightforward.

Test the full drain and switch procedure in a non-production environment before scheduling a production cutover.
{: important}

### Private endpoints
{: #private-endpoints-requirement}

Gen 2 deployments use private endpoints only. Public endpoints are not available on Gen 2. Before you start the migration, confirm that your producer and consumer applications can reach private service endpoints in the target VPC. If your applications currently connect over public endpoints, plan the network changes before you begin.

## Planning considerations
{: #planning-considerations}

Keep these points in mind when planning your migration:

* Complete a full inventory of all producer and consumer applications and their consumer groups before starting. Every application that connects to Gen 1 needs its connection string updated.
* Recreate all topic configurations on the Gen 2 instance before the cutover window.
* Coordinate the cutover window with all teams that own producer and consumer applications.
* Gen 2 supports private endpoints only. Plan your VPE gateway and any VPN or VSI connectivity before you start the wizard.
* Schedule the cutover during a low-traffic period to minimize the volume of messages that consumers need to drain.
* Test the procedure end to end in a non-production environment first.

## Before you begin
{: #prereqs}

Confirm the following before you start the wizard:

* You have a Gen 1 {{site.data.keyword.messagehub}} deployment to migrate.
* You have access to provision a Gen 2 deployment in the target account, resource group, and region.
* A maintenance window is approved and communicated to all application teams.
* You have a complete inventory of all producer and consumer applications and their consumer groups.
* All topic configurations are documented and ready to be recreated on Gen 2.
* You have access to update connection strings and redeploy all producer and consumer applications.
* A rollback plan is documented.

Run through these checks on the source deployment:

* Confirm there are no in-progress operations on the Gen 1 instance.
* Review all topic configurations, including partition counts, retention policies, and replication factors.
* Identify the current consumer group offsets for all consumer groups.
* Identify whether applications connect over public endpoints, private endpoints, or both.

When you are ready, continue to [Drain and switch migration](#drain-switch-migration).

## Drain and switch migration
{: #drain-switch-migration}

The drain and switch migration provisions a new Gen 2 instance, then coordinates the cutover of producers and consumers from Gen 1 to Gen 2. Topic configurations are not transferred automatically and must be recreated on the Gen 2 instance before the cutover begins.

* [Phase 1: Prepare and provision](#ds-phase-1)

   * [Step 1: Select migration path](#ds-select-path)
   * [Step 2: Assess Gen 1](#ds-assess-gen1)
   * [Step 3: Create Gen 2 instance](#ds-provision-gen2)

* [Phase 2: Configure and cut over](#ds-phase-2)

   * [Step 4: Set network access](#ds-set-network-access)
   * [Step 5: Cut over](#ds-cutover)
   * [Step 6: Finish migration](#ds-finish-migration)

### Phase 1: Prepare and provision
{: #ds-phase-1}

Phase 1 covers the preparation and provisioning steps required before the cutover can begin.

#### Step 1: Select migration path
{: #ds-select-path}
{: step}

Open the Migration hub in the {{site.data.keyword.cloud_notm}} console. Under **Data services**, select **Eligible data instances** to see the Gen 1 Classic deployments in your account that are ready to migrate. Find your {{site.data.keyword.messagehub}} instance, click **Open wizard**, then select **Drain and switch** and click **Start**.

#### Step 2: Assess Gen 1
{: #ds-assess-gen1}
{: step}

After you select the drain and switch path, the wizard runs an automated assessment of your Gen 1 source deployment and surfaces any configuration items and contextual information about what changes between Gen 1 and Gen 2. Review these before proceeding.

Use this step to confirm your preparation is complete:

* All topic configurations are documented.
* All producer and consumer applications are inventoried.
* The cutover window is confirmed with all application teams.

After you address any flagged items, **Refresh** to update the assessment results. When the assessment is clear, click **Next: Create Gen 2** to continue.

#### Step 3: Create Gen 2 instance
{: #ds-provision-gen2}
{: step}

This step provisions a new Gen 2 (VPC) {{site.data.keyword.messagehub}} instance. No messages or topic configurations are transferred from Gen 1 automatically.

The wizard shows a **Preview {{site.data.keyword.messagehub}} on Gen 2** summary with the proposed configuration. Fields marked **Changed** highlight differences from the Gen 1 source.

Click **Edit configurations** to adjust the Gen 2 settings before provisioning begins. The edit panel shows your Gen 1 resource allocation for reference and lets you configure:

* **Location and platform**: Target region. Platform is locked to Gen 2.
* **Service details**: Instance name, resource group, and tags.
* **Resource allocation**: Throughput and storage capacity.

Size the Gen 2 instance to match or exceed the Gen 1 source throughput and storage capacity.
{: note}

After you confirm, the wizard starts provisioning and a progress bar tracks completion. While the Gen 2 instance provisions, use this time to recreate all topic configurations on the new instance.

Recreate all topics on the Gen 2 instance before proceeding to the next step. The wizard does not replicate topic configurations automatically.
{: important}

### Phase 2: Configure and cut over
{: #ds-phase-2}

Phase 2 covers network access setup, the drain and switch cutover, and finishing the migration.

#### Step 4: Set network access
{: #ds-set-network-access}
{: step}

Configure private connectivity to the Gen 2 (VPC) instance before cutover. All connection types require a Virtual Private Endpoint (VPE) gateway for private access to the Gen 2 deployment.

Click **Open VPE gateway** to create the gateway from the VPC networking console. **Refresh** after setup to confirm the configuration is detected by the wizard.

Validate that all producer and consumer applications can reach the Gen 2 instance through the VPE before starting the cutover. Application connectivity issues discovered during the cutover window extend downtime.
{: tip}

The VPE gateway is required. The **Next: Cutover** button is not available until this step is complete.
{: important}

#### Step 5: Cut over
{: #ds-cutover}
{: step}

Complete the following stages in order. Confirm each stage is complete before proceeding to the next.
{: important}

##### Stage 1: Stop producers
{: #ds-stop-producers}

Gracefully stop all producer applications connected to Gen 1. Verify that no new messages are being written to the Gen 1 instance before continuing.

Stopping producers before consumers have drained is what creates the downtime window. Minimize the time between stopping producers and restarting them on Gen 2 to limit impact.

##### Stage 2: Drain consumers
{: #ds-drain-consumers}

Allow all consumer applications to process the remaining messages from Gen 1 until each consumer group's lag reaches zero. Monitor consumer group offsets to confirm all messages are consumed and offsets are committed before proceeding.

Do not switch connections until all consumer groups have fully drained. Switching while messages remain on Gen 1 results in those messages not being processed.
{: important}

##### Stage 3: Update connection strings
{: #ds-update-connections}

Update all producer and consumer application configurations to use the Gen 2 (VPC) connection details. The wizard provides the Gen 2 bootstrap server addresses, credentials, and SSL configuration for reference.

Gen 2 uses Let's Encrypt TLS certificates. Update any certificate validation or trust store configuration in your applications accordingly.
{: note}

**Update service credentials**

If your applications already use Gen 2 service credentials, no changes are needed for authentication. Otherwise, create new service credentials for the Gen 2 instance and update your application configuration.

**Consumer offset initialization**

Consumer groups starting on Gen 2 begin from the earliest or latest offset depending on the application's `auto.offset.reset` configuration. Because Gen 2 is a new instance with no prior message history, confirm that each consumer group's offset initialization strategy is correct before restarting consumers to avoid unintended reprocessing or message skipping.

##### Stage 4: Restart and validate
{: #ds-restart-validate}

Redeploy or restart all producer and consumer applications with the updated Gen 2 connection details.

After applications are running, verify the following:

* Producers are successfully writing messages to Gen 2.
* Consumers are processing messages from Gen 2 and committing offsets.
* No unexpected errors or lag is present.
* Message throughput is consistent with expectations.

#### Step 6: Finish migration
{: #ds-finish-migration}
{: step}

After producers and consumers are confirmed to be operating correctly on Gen 2, click **Next: Finish migration** to complete the workflow.

The Finish migration screen shows a summary of both instances. Additional metrics are available through IBM Cloud Monitoring.

After finishing, the Gen 1 instance remains available. Verify Gen 2 stability and confirm no applications are still connected to Gen 1 before decommissioning. Allow a minimum observation period of seven days before deleting the Gen 1 instance.

Continue to [Post-migration tasks](#post-migration) to wrap up.

## Post-migration tasks
{: #post-migration}

After migration is complete:

* Monitor message throughput, consumer lag, and error rates on the Gen 2 instance for at least seven days.
* Confirm that no producer or consumer applications are still connected to Gen 1.
* Review and update any monitoring, alerting, or runbook configurations that reference Gen 1 endpoints.
* Update internal documentation and inventory records to reflect the new Gen 2 connection details.
* Delete any Gen 1-specific service credentials that are no longer needed.
* Decommission the Gen 1 instance only after the observation period is complete and no applications remain connected.
