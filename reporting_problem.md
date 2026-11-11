---

copyright:
  years: 2026
lastupdated: "2026-11-11"

keywords: reporting a problem, problem report

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Reporting a problem with {{site.data.keyword.messagehub}} Standard and Enterprise plans
{: #report_problem_enterprise}

[Gen 2]{: tag-purple}

Encountering issues with {{site.data.keyword.messagehub}} on Standard or Enterprise plans? Check the {{site.data.keyword.cloud}} status page and report problems to the {{site.data.keyword.messagehub}} team.
{: shortdesc}

If you need help from the {{site.data.keyword.messagehub}} team, gather all the following information. The more information that you can provide upfront, the more efficiently the team can help with the problem:

1. What is the CRN ID of the {{site.data.keyword.messagehub}} service that you use? You can provide this ID by pasting the full {{site.data.keyword.cloud_notm}} console URL after you click the service, or by pasting the output from the following CLI command:

    ```sh
    ibmcloud resource service-instance NAME
    ```
    {: codeblock}

2. When did the problem first occur (include the time, date, and time zone)? How long did your app run before the problem occurred?
3. Is the problem still occurring? Can you replicate it?
4. Which Kafka client does your application use? What are the version details?
5. What are your client configuration details? Include your producer and consumer settings, and list any nondefault options that you passed to your producer or consumer.
6. Do you have application log snippets that display the problem?
7. What is the issue that you are seeing? Which topics, client IDs, group IDs, and transaction IDs are affected?
8. What impact does the problem have on your service?

You can provide the information that you gathered to {{site.data.keyword.IBM_notm}} in a support ticket by [creating a support case](/docs/support?topic=support-open-case).
