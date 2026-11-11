---

copyright:
  years: 2026
lastupdated: "2026-11-11"

keywords: event streams migration, gen1 to gen2, classic infrastructure, vpc, drain and switch, private endpoints, kafka, topics, producers, consumers

subcollection: EventStreams-gen2

content-type: tutorial

---

{{site.data.keyword.attribute-definition-list}}

# Migrating {{site.data.keyword.cloud_notm}} {{site.data.keyword.messagehub}} from Gen 1 to Gen 2
{: #migrating-event-streams-gen1-to-gen2}

[Gen 2]{: tag-purple}

Follow this tutorial to migrate your {{site.data.keyword.messagehub}} deployment from Gen 1 to Gen 2 by using the drain and switch process.
{: shortdesc}

This tutorial applies to both the Standard and Enterprise plans. Both plans follow the same drain and switch migration process.

The drain and switch cutover requires coordinated action across all application teams to stop producers, drain consumers, and restart applications on Gen 2. Plan the cutover window carefully before you start.

## Key considerations before you begin
{: #key-considerations}

Review these points before you start the migration.

### Drain and switch overview
{: #drain-and-switch-explained}

Drain and switch is a coordinated cutover approach for message streaming workloads:

1. You provision the Gen 2 instance with matching topic configurations.
1. You stop producers, which halts new message writes to Gen 1.
1. Consumers process all remaining messages from Gen 1 until consumer group lag reaches zero.
1. You update producer and consumer connection strings to point to Gen 2.
1. Message flow resumes on Gen 2.

A planned downtime window occurs during steps 2 to 4. The duration depends on how quickly consumers drain remaining Gen 1 messages and the time required to restart applications with updated connection details.

### Topic configuration
{: #topic-config-responsibility}

You must recreate topic names, partition counts, retention policies, and replication factors on the Gen 2 instance before the cutover begins. The migration does not transfer topic configurations automatically.

Recreate all topic configurations on Gen 2 and validate them before stopping producers on Gen 1.
{: important}

### Migration risks
{: #migration-risks}

Complete each stage of the drain and switch in sequence to avoid the following risks:

Message loss
:   If you stop producers before consumers fully drain Gen 1, unprocessed messages can be lost or skipped.

Duplicate processing
:   If you do not initialize consumer offsets correctly on Gen 2, consumers can process messages more than once.

Rollback complexity
:   After consumers drain Gen 1 messages and producers write to Gen 2, rolling back to Gen 1 requires manual offset reconciliation.

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
* Gen 2 supports private endpoints only. Plan your VPE gateway and any VPN or VSI connectivity before you start the migration.
* Schedule the cutover during a low-traffic period to minimize the volume of messages that consumers need to drain.
* Test the procedure end to end in a non-production environment first.

## Before you begin
{: #prereqs}

Confirm the following before you start the migration:

* You have a Gen 1 {{site.data.keyword.messagehub}} deployment to migrate.
* You have access to provision a Gen 2 deployment in the target account, resource group, and region.
* A maintenance window is approved and communicated to all application teams.
* You have a complete inventory of all producer and consumer applications and their consumer groups.
* You documented all topic configurations and are ready to recreate them on Gen 2.
* You have access to update connection strings and redeploy all producer and consumer applications.
* You documented a rollback plan.

Run through these checks on the source deployment:

* Confirm there are no in-progress operations on the Gen 1 instance.
* Review all topic configurations, including partition counts, retention policies, and replication factors.
* Identify the current consumer group offsets for all consumer groups.
* Identify whether applications connect over public endpoints, private endpoints, or both.

When you are ready, continue to [Drain and switch migration](#drain-switch-migration).

## Drain and switch migration
{: #drain-switch-migration}

The drain and switch migration provisions a new Gen 2 instance, then coordinates the cutover of producers and consumers from Gen 1 to Gen 2. The migration does not transfer topic configurations automatically. Recreate them on the Gen 2 instance before the cutover begins.

* [Phase 1: Prepare and provision](#ds-phase-1)

   * [Step 1: Assess Gen 1](#ds-assess-gen1)
   * [Step 2: Provision the Gen 2 instance](#ds-provision-gen2)

* [Phase 2: Configure and cut over](#ds-phase-2)

   * [Step 3: Set network access](#ds-set-network-access)
   * [Step 4: Cut over](#ds-cutover)
   * [Step 5: Finish migration](#ds-finish-migration)

### Phase 1: Prepare and provision
{: #ds-phase-1}

Phase 1 covers the preparation and provisioning steps required before the cutover can begin.

#### Step 1: Assess Gen 1
{: #ds-assess-gen1}
{: step}

Review your Gen 1 source deployment and confirm your preparation is complete before provisioning Gen 2:

* All topic configurations are documented.
* All producer and consumer applications are inventoried.
* The cutover window is confirmed with all application teams.

#### Step 2: Provision the Gen 2 instance
{: #ds-provision-gen2}
{: step}

Provision a new Gen 2 (VPC) {{site.data.keyword.messagehub}} instance. The migration does not transfer messages or topic configurations from Gen 1 automatically.

When provisioning, configure the following options:

Location and platform
:   Target region. The platform is set to Gen 2.

Service details
:   Instance name, resource group, and tags.

Resource allocation
:   Throughput and storage capacity.

Size the Gen 2 instance to match or exceed the Gen 1 source throughput and storage capacity.
{: note}

While the Gen 2 instance provisions, recreate all topic configurations on the new instance.

Recreate all topics on the Gen 2 instance before you proceed to the next step. The service does not replicate topic configurations automatically.
{: important}

### Phase 2: Configure and cut over
{: #ds-phase-2}

Phase 2 covers network access setup, the drain and switch cutover, and completing the migration.

#### Step 3: Set network access
{: #ds-set-network-access}
{: step}

Configure private connectivity to the Gen 2 (VPC) instance before cutover. All connection types require a virtual private endpoint (VPE) gateway for private access to the Gen 2 deployment. Create the gateway from the [{{site.data.keyword.cloud}} VPC networking console](https://cloud.ibm.com/infrastructure/provision/endpointGateway){: external}.

Validate that all producer and consumer applications can reach the Gen 2 instance through the VPE before you start the cutover. Application connectivity issues that you discover during the cutover window extend downtime.
{: tip}

The VPE gateway must be in place before you begin the cutover.
{: important}

#### Step 4: Cut over
{: #ds-cutover}
{: step}

Complete the following stages in order. Confirm that each stage is complete before you proceed to the next stage.
{: important}

##### Stage 1: Stop producers
{: #ds-stop-producers}

Stop all producer applications that connect to Gen 1. Verify that no new messages are written to the Gen 1 instance before you continue.

Stopping producers before consumers drain remaining messages creates the downtime window. Minimize the time between stopping producers and restarting them on Gen 2 to limit downtime.

##### Stage 2: Drain consumers
{: #ds-drain-consumers}

Allow all consumer applications to process remaining messages from Gen 1 until consumer group lag reaches zero. Monitor consumer group offsets to confirm that all messages are consumed and offsets are committed before you proceed.

Do not switch connections until all consumer groups drain fully. Switching while messages remain on Gen 1 causes those messages to be skipped.
{: important}

##### Stage 3: Update connection strings
{: #ds-update-connections}

Update all producer and consumer application configurations to use the Gen 2 (VPC) connection details. Retrieve the Gen 2 bootstrap server addresses, credentials, and SSL configuration from the service credentials of your new Gen 2 instance.

Gen 2 uses Let's Encrypt TLS certificates. Update any certificate validation or trust store configuration in your applications accordingly.
{: note}

###### Updating service credentials
{: #ds-update-creds}

If your applications already use Gen 2 service credentials, no changes are needed for authentication. Otherwise, create new service credentials for the Gen 2 instance and update your application configuration.

###### Initializing consumer offsets
{: #ds-consumer-offset-init}

Consumer groups starting on Gen 2 begin from the earliest or latest offset depending on the application `auto.offset.reset` configuration. Gen 2 is a new instance with no prior message history. Confirm that each consumer group offset initialization strategy is correct before you restart consumers to avoid unintended reprocessing or message skipping.

##### Stage 4: Restart and validate
{: #ds-restart-validate}

Redeploy or restart all producer and consumer applications with the updated Gen 2 connection details.

After applications are running, verify the following conditions:

* Producers are writing messages to Gen 2.
* Consumers are processing messages from Gen 2 and committing offsets.
* No unexpected errors or lag are present.
* Message throughput is consistent with expectations.

#### Step 5: Finish migration
{: #ds-finish-migration}
{: step}

After producers and consumers operate correctly on Gen 2, the migration cutover is complete.

You can view additional metrics in {{site.data.keyword.monitoringfull}}.

The Gen 1 instance remains available after cutover. Verify Gen 2 stability and confirm that no applications remain connected to Gen 1 before decommissioning. Allow a minimum observation period of seven days before you delete the Gen 1 instance.

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
