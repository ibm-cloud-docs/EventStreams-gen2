---

copyright:
  years: 2015, 2024
lastupdated: "2024-03-01"

keywords: restrictions, failover, jvm ttl, creating topic, deleting topic

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Known restrictions for {{site.data.keyword.messagehub}}
{: #restrictions}

If you find a problem while you use {{site.data.keyword.messagehub}}, review the following known restrictions and workarounds. 
{: shortdesc}

## Java Kafka calls don’t failover if a Kafka bootstrap server fails
{: #calls_failover}

### Problem
{: #calls_failover_problem notoc}

The Java™ Virtual Machine (JVM) caches DNS lookups. When the JVM resolves an IP address for a hostname, it caches the IP address for a specified period, which is known as the time to live (TTL). Some Java configurations set the JVM TTL so that it never refreshes a hostname’s IP address until the JVM is restarted. An example configuration is one that has a security manager.

### Workaround
{: #calls_failover_workaround notoc}

Because {{site.data.keyword.messagehub}} uses Kafka bootstrap server URLs with multiple IP addresses for high availability, not all the broker IP addresses are known to the Kafka client, which prevents failover to a working broker. In these cases, a failover requires a requery of the IP addresses for the broker URLs to get a working IP address. You are recommended to configure your JVM with a TTL value of 30 to 60 seconds. This value ensures that if a bootstrap server’s IP address has issues, the Kafka client is able to look up and use a new IP address by querying the DNS.

See the following example from the `java.security` file: 

```text
# The Java-level namelookup cache policy for successful lookups:
#
# any negative value: caching forever
# any positive value: the number of seconds to cache an address for
# zero: do not cache
#
# default value is forever (FOREVER). For security reasons, this
# caching is made forever when a security manager is set. When a security
# manager is not set, the default behavior in this implementation
# is to cache for 30 seconds.
#
# NOTE: setting this to anything other than the default value can have
#       serious security implications. Do not set it unless
#       you are sure you are not exposed to DNS spoofing attack.
#
#networkaddress.cache.ttl=-1
```

### How to modify the JVM's TTL
{: #jvm_ttl notoc}

- To modify the JVM's TTL for all applications, set the `networkaddress.cache.ttl` value in the `<$JAVA_HOME>/jre/lib/security/java.security` file.
- To modify the JVM TTL for a specific application, set the `networkaddress.cache.ttl` in your application code.

   ```text
   java.security.Security.setProperty("networkaddress.cache.ttl" , "30");
   ```

## Java Kafka calls might time out
{: #calls_timeout_kafka}

### Problem
{: #calls_timeout_problem}
{: notoc}

Sometimes a Kafka Java client call fails to find Kafka. The cause of failure is that the Kafka client determined the same failing IP address for each of the bootstrap servers. The Kafka client tries each broker’s IP address (which is the same failing IP address) and incorrectly determines that Kafka is down. The Kafka client uses the first IP address that is returned in the list if multiple IP addresses are returned in the DNS query.

### Workaround
{: #calls_timeout_workaround}
{: notoc}

Retry your calls after you wait long enough for the JVM DNS cache for the broker URLs to expire. On subsequent Kafka calls, a working broker IP address is returned from the DNS query. 

A Kafka Improvement Proposal (KIP) #302 (available as an option from Kafka 2.1.1 and included by default from 2.6.0) ensures that Kafka clients try all available broker IP addresses 
and not a subset, so a failure in a single IP address does not cause a failure. 

You need to opt into this functionality by using one of the following methods:

- Specify a new allowed value in the Consumer and Producer properties of the configuration parameter `client.dns.lookup`:

    ```text
    client.dns.lookup: "use_all_dns_ips" 
    ```
    {: codeblock}

- Use the constants `CommonClientConfigs.CLIENT_DNS_LOOKUP_CONFIG: ClientDnsLookup.USE_ALL_DNS_IPS`.

## Topics and partitions
{: #topics_partitions}

- Topic names are restricted to a maximum of 200 characters.
- The default number of partitions for a topic is one.

## Creating and deleting topics in Kafka
{: #create_delete}

In Kafka, topic creation and deletion are asynchronous operations
that might take some time to complete. Avoid usage patterns that rely on the rapid creation and deletion
of topics, or on the rapid deletion and re-creation of topics.

