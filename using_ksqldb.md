---

copyright:
  years: 2026
lastupdated: "2026-11-11"

keywords: ksqlDB, ksql

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Stream processing with ksqlDB on {{site.data.keyword.cloud_notm}} {{site.data.keyword.messagehub}}
{: #ksql_using}

[Gen 2]{: tag-purple}

Use ksqlDB with {{site.data.keyword.messagehub}} for stream processing on {{site.data.keyword.cloud}}. Learn how to set up and run ksqlDB for event streaming applications.
{: shortdesc}

ksqlDB is an event streaming database built on Apache Kafka that you can use to build stream processing applications by using familiar SQL syntax.

You can run ksqlDB with {{site.data.keyword.messagehub}} by using Docker as described in the [ksqlDB quickstart](https://www.confluent.io/product/ksqldb/){: external}.

Because ksqlDB requires topics with an unlimited `retention.ms` setting, you can use ksqlDB only with the Enterprise plan.
{: important}

Complete the following steps to configure ksqlDB. After configuration is complete, you can run the queries described in the [ksqlDB Quick Start Guide](https://www.confluent.io/product/ksqldb/#quickstart-content){: external}.

1. In the {{site.data.keyword.cloud_notm}} console, navigate to your {{site.data.keyword.messagehub}} instance and create a topic named `_confluent-ksql-default__command_topic` with one partition and the default retention period.

2. To run ksqlDB in a Docker container:
   
    a. Create a `docker-compose.yml` file with the following configuration:
    
    ```yaml
    version: '2'

    services:
      ksqldb-server:
        image: confluentinc/ksqldb-server:latest
        hostname: ksqldb-server
        container_name: ksqldb-server
        ports:
          - "8088:8088"
        environment:
          KSQL_LISTENERS: http://0.0.0.0:8088
          KSQL_BOOTSTRAP_SERVERS: <BOOTSTRAP_SERVERS>
          KSQL_KSQL_SERVICE_ID: default_
          KSQL_KSQL_SINK_REPLICAS: 3
          KSQL_KSQL_STREAMS_REPLICATION_FACTOR: 3
          KSQL_SECURITY_PROTOCOL: SASL_SSL
          KSQL_KSQL_INTERNAL_TOPIC_REPLICAS: 3
          KSQL_SASL_JAAS_CONFIG: org.apache.kafka.common.security.plain.PlainLoginModule required username=\"token\" password=\"<PASSWORD>\";
          KSQL_SASL_MECHANISM: PLAIN

      ksqldb-cli:
        image: confluentinc/ksqldb-cli:latest
        container_name: ksqldb-cli
        depends_on:
          - ksqldb-server
        entrypoint: /bin/sh
        tty: true
    ```
    {: codeblock}
    
    Replace `<BOOTSTRAP_SERVERS>` and `<PASSWORD>` with the connection values from your service credential.

    b. From the directory that contains the file, start the ksqlDB server:
    
    ```sh
    docker-compose up -d
    ```
    {: pre}

    c. Connect to the ksqlDB server and launch an interactive CLI session:
    
    ```sh
    docker exec -it ksqldb-cli ksql http://ksqldb-server:8088
    ```
    {: pre}

3. To run ksqlDB natively from source:
 
    a. Clone the ksqlDB repository:
    
    ```sh
    git clone https://github.com/confluentinc/ksql.git
    ```
    {: pre}

    b. Navigate to the repository directory and compile the project:
    
    ```sh
    cd ksql
    mvn clean install -Dmaven.test.skip=true
    ```
    {: pre}
    
    c. Create an {{site.data.keyword.messagehub}} ksqlDB configuration file (for example, `config/ibm-eventstreams.properties`):
    
    ```properties
    bootstrap.servers=<BOOTSTRAP_SERVERS>
    listeners=http://localhost:8088
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="token" password="<PASSWORD>";
    ksql.sink.replications.default=3
    ```
    {: codeblock}
    
    Replace `<BOOTSTRAP_SERVERS>` and `<PASSWORD>` with your service credential values.

    d. Start the ksqlDB server:
    
    ```sh
    ./bin/ksql-server-start ./config/ibm-eventstreams.properties
    ```
    {: pre}
    
    e. Start the ksqlDB CLI:
    
    ```sh
    ./bin/ksql
    ```
    {: pre}

When you complete these steps, you can run stream processing queries in ksqlDB.
