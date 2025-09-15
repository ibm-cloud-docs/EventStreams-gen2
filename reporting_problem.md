---

copyright:
  years: 2015, 2023
lastupdated: "2023-07-24"

keywords: reporting a problem, problem report

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Reporting a problem to the {{site.data.keyword.messagehub}} team - Standard and Enterprise plans
{: #report_problem_enterprise}


If you encounter a problem with {{site.data.keyword.messagehub}}, first check the [{{site.data.keyword.Bluemix_notm}} status page](https://cloud.ibm.com/status?selected=status){: external}.
{: shortdesc}

If you need help from the {{site.data.keyword.messagehub}} team, gather all the following information. The more information that you can provide upfront, the more efficiently the team can help with the problem:
{: shortdesc}

1. What is the CRN ID of the {{site.data.keyword.messagehub}} service that you use? You can provide this ID by pasting the full {{site.data.keyword.Bluemix_notm}} console URL after you click the service, or by pasting the output from the following CLI command:
2. 
    ```sh
    ibmcloud resource service-instance NAME
    ```
    {: codeblock}
    
2. When did the problem first occur (specifically time, date, and time zone)? How long did your app run before the problem occurred?
3. Is the problem still occurring? Can you replicate it?
4. Which Kafka client does your application use? What are the version details?
5. What are your client configuration details? We need to know your producer and consumer settings, so list any nondefault options that you passed to your producer or consumer. 
6. Do you have application log snippets that display the problem?
7. What is the issue that you are seeing? Which topics, client IDs, group IDs, and transaction IDs are affected?
8. What impact does the problem have on your service?

You can provide the information that you gathered to IBM in a support ticket by [creating a support case](/docs/get-support?topic=get-support-open-case){: external}.

