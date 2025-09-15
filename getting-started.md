---

copyright:
  years: 2015, 2024
lastupdated: "2024-02-20"

keywords: event streams, message hub

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with {{site.data.keyword.messagehub}} for {{site.data.keyword.cloud_notm}}
{: #getting-started}

{{site.data.keyword.messagehub_full}} is a high-throughput message bus that is built with Apache Kafka. To get started with this managed Kafka on {{site.data.keyword.cloud}} service and start sending and receiving messages, you can use the Java™ sample. The sample shows how a producer sends messages to a consumer by using a topic. The same sample program is used to consume messages and produce messages.
{: shortdesc}

To understand more about how {{site.data.keyword.messagehub}} works, see [{{site.data.keyword.messagehub}} overview](/docs/EventStreams?topic=EventStreams-about). {{site.data.keyword.messagehub}} was previously called Message Hub.

To access {{site.data.keyword.messagehub}} samples, see our primary sample repository at [{{site.data.keyword.messagehub}} samples](https://github.com/IBM/eventstreams-samples){: external}.  Other {{site.data.keyword.messagehub}} samples, including samples for Node.js and Python, are available at the previous [{{site.data.keyword.messagehub}} repository](https://github.com/ibm-messaging/event-streams-samples){: external}.

![Get started with {{site.data.keyword.messagehub_full}}](https://video.ibm.com/embed/recorded/131695471){: video output="iframe" data-script="none" id="youtubeplayer" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen}

## Prerequisites
{: #getting_started_prereqs}

1. **If you don't already have one, create an {{site.data.keyword.messagehub}} service instance.**

   a. Log in to the {{site.data.keyword.cloud_notm}} console.
  
   b. Click the [**{{site.data.keyword.messagehub}} service**](https://cloud.ibm.com/eventstreams-provisioning/6a7f4e38-f218-48ef-9dd2-df408747568e/create){: external} in the **Catalog**.
  
   c. Select the **Lite plan** on the service instance page.
  
   d. Enter a name for your service. You can use the default value.
  
   e. Click **Create**. The {{site.data.keyword.messagehub}} **Getting started** page opens. 

2. **If you don't already have them, install the following prerequisites:**
	
	- [Git](https://git-scm.com/){: external}
	- Java™  8 or higher 

## Tutorial steps
{: #getting_started_steps}

1. In the {{site.data.keyword.cloud_notm}} console (UI), go to your **Resource list**, select your {{site.data.keyword.messagehub}} resource, and click the **Get started with a sample application** tile.

2. Complete the **Configure and run starter application** steps:

     a. Click the **Download .JAR from GitHub** button to download the latest .JAR release. 
     
     b. Click the **Generate properties** button to generate and download the properties file. The **Topic** panel opens.
     
     c. Enter a name for a **New topic** that you want to create. Or, click **Existing topic** to select an existing topic that you want to use. 
     
     If you are on the Lite plan and a topic already exists, the **New topic** button is disabled. {: note}
     
     d. Select the **Service credential** that you want to use, or select **Create new** to create a new one. 
     
     e. Click the **Generate and download** button. The `kafka.properties` file is downloaded to your local machine. It contains the necessary configuration to connect to {{site.data.keyword.messagehub}}. You can open it in a text editor to see how it works.
     
     f. The panel shows your selected configuration. Clicking the **Regenerate** button starts the process again. Clicking the **Download** button downloads the properties file again.
     
3. Navigate to the folder that contains the downloaded .JAR file by using the command line or a terminal and run the command. Replace the value `<path-to-properties>` with the file path to the `kafka.properties` file.

4. When the application starts, click the **localhost:8080** link to open the starter app and see messages flowing through the topic.

## Next steps
{: #next_steps}

Now that you ran the starter application, you can try other [{{site.data.keyword.messagehub}} samples](https://github.com/ibm-messaging/event-streams-samples){: external}. Explore [other ways to connect](/docs/EventStreams?topic=EventStreams-kafka_connect){: external} to the {{site.data.keyword.messagehub}} service or find out more about 
[{{site.data.keyword.messagehub}} on {{site.data.keyword.satellitelong_notm}}](/docs/EventStreams?topic=EventStreams-satellite_about){: external}, a hybrid environment that combines public cloud services running in your secure private cloud.

To watch a video that walks you through getting a Java sample to run, see [Getting started with IBM {{site.data.keyword.messagehub}}](https://www.youtube.com/watch?v=XyNy7TcfJOc).{: external}
