---

copyright:
  years: 2025
lastupdated: "2025-12-04"

keywords: security, gdpr, hipaa, compliance, personal data, personal information, privacy policy, iso, financial services, ismap, c5, soc 2, type 2, pci, irap

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Understanding compliance for {{site.data.keyword.messagehub}}
{: #compliance}

[Gen 2]{: tag-purple}

{{site.data.keyword.messagehub_full}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see the [Beta reference](/docs/EventStreams-gen2?topic=EventStreams-gen2-gen2-beta).
{: beta}

{{site.data.keyword.messagehub_full}} is a secure and highly available message bus that is built with Apache Kafka. The service is built on the following best industry standards.
{: shortdesc}

## {{site.data.keyword.cloud_notm}} for Financial Services
{: #cloud_fs}

{{site.data.keyword.messagehub}} is {{site.data.keyword.cloud}} for [Financial Services](https://www.ibm.com/cloud/financial-services) certified. For information about requesting an {{site.data.keyword.cloud_notm}} for Financial Services report, see [{{site.data.keyword.cloud_notm}} industry compliance programs](https://www.ibm.com/cloud/compliance).

The Enterprise plan is {{site.data.keyword.cloud}} Financial Services certified.

## General Data Protection Regulation (GDPR)
{: #gdpr}

The GDPR seeks to create a harmonized data protection law framework across the EU. It aims to give citizens back the control of their personal data, while it imposes strict rules on the ones who host and "process" this data, anywhere in the world. The Regulation also introduces rules that relate to the free movement of personal data within and outside the EU.

With the [General Data Protection Regulation](https://gdpr.eu/), {{site.data.keyword.messagehub}} customers can rely on the {{site.data.keyword.messagehub}} team's understanding and compliance with emerging data privacy standards and legislation. Customers can also rely on IBM's wider ability to provide a comprehensive suite of solutions to assist businesses of all sizes with their own internal data governance requirements.

The Enterprise and Standard plans are GDPR certified.

## Health Insurance Portability and Accountability Act (HIPAA)
{: #hipaa}

{{site.data.keyword.messagehub}} meets the required IBM controls that are commensurate with the Health Insurance Portability and Accountability Act of 1996 (HIPAA) Security and Privacy Rule requirements. These requirements include the appropriate administrative, physical, and technical safeguards required of Business Associates in 45 CFR Part 160 and Subparts A and C of Part 164. HIPAA must be requested at the time of provisioning and requires a representative to sign a Business Associate Addendum (BAA) agreement with IBM.

The Enterprise plan meets HIPAA requirements.

## ISO 27001/27017/27018 and ISO 27701
{: #iso}

{{site.data.keyword.messagehub}} is ISO 27001, 27017, 27018, and ISO 27701 certified. ISO 27001 (International Organisation for Standardisation) is an international standard for information security. ISO 27017/27018 is an information security framework for Cloud service providers. ISO 27701 is a privacy add on and requires developing and managing a privacy information management system (PIMS). 

The certificates and the certified cloud products listing can be found in the [IBM Trust Center](https://www.ibm.com/trust) in section [IBM Cloud Compliance Programs - Global](https://www.ibm.com/cloud/compliance).

The Enterprise and Standard plans are ISO 27001, 27017, 27018, and ISO 27701 certified.

## Information System Security Management and Assessment Program (ISMAP) 
{: #ismap}

{{site.data.keyword.messagehub}} Standard and Enteprise plans are certified by ISMAP (Information System Security Management and Assessment Program), the Japanese government program for assessing the security of public cloud services.

The Enterprise and Standard plans are ISMAP certified.

## Cloud Computing Compliance Controls Catalogue (C5)
{: #c5}

{{site.data.keyword.messagehub}} Standard and Enterprise plans are certified to C5 (Cloud Computing Compliance Controls Catalogue) and meet the requirements for cloud security and the adoption of public cloud solutions by German government agencies.

The Enterprise and Standard plans are C5 certified.

## SOC 1 Type 2, SOC 2 Type 2, and SOC 3 Certification
{: #soc_type}

{{site.data.keyword.IBM}} provides a Service Organization Controls (SOC) report for {{site.data.keyword.messagehub}}. The reports evaluate {{site.data.keyword.IBM_notm}}'s operational controls according to the criteria set by the American Institute of Certified Public Accountants (AICPA) Trust Services Principles. The Trust Services Principles define adequate control systems and establish industry standards for service providers such as {{site.data.keyword.cloud_notm}} to safeguard their customers' data and information.

You can request a SOC report from the customer portal or contact your sales representative. Alternatively, you can open a support ticket with [IBM Cloud support](https://cloud.ibm.com/unifiedsupport/supportcenter).

The Enterprise and Standard plans are SOC certified.

## Payment Card Industry Data Security Standard (PCI DSS)
{: #pci_dss}

{{site.data.keyword.messagehub}} is compliant with the Payment Card Industry Data Security Standard (PCI DSS). {{site.data.keyword.cloud_notm}} completes annual PCI DSS assessments by using an approved Qualified Security Assessor (QSA), and the resulting Attestations of Compliance (AOCs) and Service Responsibility Matrix (SRM) guides are available upon customer request. Auditors reviewed {{site.data.keyword.messagehub}} for compliance under PCI DSS version 3.2.1 at Service Provider Level 1.

If you intend to store sensitive information in an IBM Cloudant database, you must use client-side encryption to render data unreadable to IBM Cloudant operators. For example, for PCI DSS compliance, you must encrypt the Primary Account Number (PAN) before sending a document that contains it to the database.
{: tip}

Customers are responsible for the storing, processing, and transmission of their cardholder data, and can create cardholder data environments (CDEs) that can store, transmit, or process cardholder data by using {{site.data.keyword.messagehub}}. Customers can request and use the {{site.data.keyword.cloud_notm}} AOCs and SRM guides when they seek their own PCI DSS certifications. It is the responsibility of the customer to document and operate CDEs and applications that are built by using {{site.data.keyword.cloud_notm}} Platform services in a PCI DSS-compliant manner.

{{site.data.keyword.messagehub}} documentation on service security and deletion of data covers methods to manage cardholder data within the environment in accordance with PCI requirements. It is the customer’s responsibility to familiarize themselves with these processes and to manage data retention and removal from the service according to the customer’s policies. To facilitate this process, no cardholder data can be used in an {{site.data.keyword.messagehub}} document ID. If PAN data is to be stored in {{site.data.keyword.messagehub}}, they must be rendered unreadable (in accordance with PCI requirement 3.4) before transmission to the {{site.data.keyword.messagehub}} service.

A full list of PCI DSS-ready {{site.data.keyword.cloud_notm}} Platform services, and options to request a PCI DSS AOC and SRM guide, can be found at the [IBM Cloud compliance page](https://www.ibm.com/cloud/compliance).

The Enterprise and Standard plans are PCI DSS certified.

##  Information Security (InfoSec) Registered Assessors Program (IRAP)
{: #irap}

The Information Security (InfoSec) Registered Assessors Program (IRAP) outlines a framework for assessing the implementation and effectiveness of {{site.data.keyword.messagehub}} and {{site.data.keyword.Bluemix}}'s security controls against the Australian government's Information Security Manual (ISM). The {{site.data.keyword.messagehub_full}} Enterprise plan has been assessed as compliant with IRAP Protected. For reports, documentation, and further details about {{site.data.keyword.Bluemix}} IRAP Protected Offerings, see [IBM Cloud compliance: IRAP (Australia)](https://www.ibm.com/cloud/compliance/irap-australia). 

The Enterprise Plan is IRAP Protected. 
