---

copyright:
  years: 2015, 2024
lastupdated: "2024-02-20"

keywords: cli, api key, cli plug-in

subcollection: EventStreams

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Getting started with the {{site.data.keyword.messagehub}} CLI 
{: #cli}

To install and set up the {{site.data.keyword.messagehub}} CLI on the Lite, Standard, and Enterprise plans, complete the following steps:
{: shortdesc}

## Step 1. Install the {{site.data.keyword.Bluemix_notm}} CLI
{: #step1_install_cli}

For more information about how to install the CLI, see [Getting started with the {{site.data.keyword.Bluemix_notm}} CLI](/docs/cli?topic=cli-getting-started){: external}.

## Step 2. Log in to {{site.data.keyword.Bluemix_notm}} 
{: #step2_login}

Run the following command to log in to {{site.data.keyword.Bluemix_notm}}:
```sh
ibmcloud login -a cloud.ibm.com
```
{: codeblock}


## Step 3. Create an {{site.data.keyword.messagehub}} instance
{: #step3_es_instance}

Create an {{site.data.keyword.messagehub}} instance on {{site.data.keyword.Bluemix_notm}} by using the Lite, Standard, or Enterprise plans. 

Select one of the following methods:

* To create an instance from the {{site.data.keyword.Bluemix_notm}} console, go to the {{site.data.keyword.messagehub}} entry in the [catalog](https://cloud.ibm.com/eventstreams-provisioning/6a7f4e38-f218-48ef-9dd2-df408747568e/create){: external}.

* To create an instance from the CLI on the Enterprise plan, run the following command:
    ```sh
    ibmcloud resource service-instance-create <INSTANCE_NAME> messagehub enterprise-3nodes-2tb <REGION>
    ```
    {: codeblock}
   
   Because Enterprise has its own dedicated resources for each cluster, it requires more time for provisioning so a new Enterprise instance might take up to 3 hours.


* To create an instance from the CLI on the Standard plan, run the following command:

    ```sh
    ibmcloud resource service-instance-create <INSTANCE_NAME> messagehub standard <REGION>
    ```
    {: codeblock}

    Provisioning a new Standard instance is instantaneous because the underlying resources are already set up.

## Step 4. Optional: Create a service API key for this instance
{: #step4_es_api}

Optionally, you can create a service API key. Valid values for `ROLE_NAME` are Manager, Writer, and Reader. Their permissions are described in [Managing access to your {{site.data.keyword.messagehub}} resources](/docs/EventStreams?topic=EventStreams-security#security).

* To create an API key from the {{site.data.keyword.Bluemix_notm}} console, enter the Service credentials from the instance page, and click **New Credentials**.

* To create an API key from the CLI, run this command:
    ```sh
    ibmcloud resource service-key-create <KEY_NAME> <ROLE_NAME> --instance-name <INSTANCE_NAME>
    ```
    {: codeblock}

## Step 5. Install the {{site.data.keyword.messagehub}} CLI plug-in
{: #step5_es_cli}

Run the following command:
```sh
ibmcloud plugin install event-streams
```
{: codeblock}

## Step 6. Initialize the {{site.data.keyword.messagehub}} plug-in
{: #step6_es_init}

Before you can run any of the {{site.data.keyword.messagehub}} CLI commands, you must first initialize the plug-in. Run the following command then select your {{site.data.keyword.messagehub}} instance:

```sh
ibmcloud es init
```
{: codeblock}

For more information about the {{site.data.keyword.messagehub}} CLI commands, see [CLI reference](/docs/EventStreams?topic=EventStreams-cli_reference).










