---

copyright:
  years: 2021, 2025
lastupdated: "2025-09-15"

keywords: service endpoints, VSIs, VPC, CSE, disruptive, iam ip address, granting access

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Using IAM IP address access restrictions with {{site.data.keyword.messagehub}}
{: #restricting_access_iam}

If you want to enable [IAM IP address access restrictions](/docs/account?topic=account-ips), you must ensure that the {{site.data.keyword.iamshort}} (IAM) IP allowlist is configured so that the {{site.data.keyword.messagehub}} authentication and authorization service, which is used to authenticate and authorize the Kafka client, can still function.
{: shortdesc}

## Granting access if you use {{site.data.keyword.messagehub}} over the public network
{: #restricting_access_iam_public}

If you use {{site.data.keyword.messagehub}} over the public network, you must add the IP addresses of any computers that access Kafka API endpoints and REST API endpoints to the {{site.data.keyword.iamshort}} (IAM) IP allowlist.

## Granting access if you use {{site.data.keyword.messagehub}} over the private network (Enterprise plan only)
{: #restricting_access_iam_private}

If you use {{site.data.keyword.messagehub}} over the private network, you must add the {{site.data.keyword.messagehub}}'s own private IP addresses for the appropriate cluster to the {{site.data.keyword.iamshort}} (IAM) IP allowlist. You can find these IP addresses with the following command: 

```text
ibmcloud resource service-instance <event-streams-instance-name> --output json
```

Find the `extensions.virtual_private_endpoints.endpoints.ip_address` section from the output, add the three `166.9.x.x` IP addresses to the {{site.data.keyword.iamshort}} (IAM) IP allowlist, as in the following example. These addresses are static and remain for the life of the service instance.

```json
    {
        "extensions": {
            "virtual_private_endpoints": {
                "dns_domain": "private.us-south.messagehub.appdomain.cloud",
                "dns_hosts": [
                    "kafka-0-mh-int-pipe-mis-cfxdrmzjgs",
                    "kafka-1-mh-int-pipe-mis-cfxdrmzjgs",
                    "kafka-2-mh-int-pipe-mis-cfxdrmzjgs",
                    "mh-int-pipe-mis-cfxdrmzjgs"
                ],
                "endpoints": [
                    {
                        "ip_address": "166.9.12.189",
                        "zone": "dal10"
                    },
                    {
                        "ip_address": "166.9.16.225",
                        "zone": "dal13"
                    },
                    {
                        "ip_address": "166.9.15.61",
                        "zone": "dal12"
                    }
                ],
                "origin_type": "cse",
                "ports": [
                    {
                        "port_max": 9093,
                        "port_min": 443
                    }
                ]
            }
        }
    }
```

This action is required for private connections because the source IP address that {{site.data.keyword.messagehub}} receives is the IP address of the {{site.data.keyword.cloud_notm}} service endpoint and not the IP from where the request originated.

You can also restrict access on the {{site.data.keyword.messagehub}} Enterprise instance using context-based restrictions. For more information, see [Restricting network access by using the Enterprise plan](/docs/EventStreams?topic=EventStreams-restrict_access#configuring_cbr).
{: note}

