---

copyright:
  years: 2015, 2023
lastupdated: "2023-07-18"

keywords: rest producer api, producing messages

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Using the REST producer API
{: #rest_producer_using}

{{site.data.keyword.messagehub}} provides a REST API to help connect your existing systems to your {{site.data.keyword.messagehub}} Kafka cluster. By using the API, you can integrate {{site.data.keyword.messagehub}} with any system that supports RESTful APIs.
{: shortdesc}

The REST producer API is available as part of the {{site.data.keyword.messagehub}} Standard and Enterprise plans only.

The REST producer API is a scalable REST interface for producing messages to {{site.data.keyword.messagehub}} over a secure HTTP endpoint. Send event data to {{site.data.keyword.messagehub}}, use Kafka technology to handle data feeds, and take advantage of {{site.data.keyword.messagehub}} features to manage your data.

Use the API to connect existing systems to {{site.data.keyword.messagehub}}. Create produce requests from your systems into {{site.data.keyword.messagehub}}, including specifying the message key, headers, and the topics that you want to write messages to.

## Accessing the REST producer API
{: #rest_produce_access}

You must retrieve the URL and credential details that are needed to connect to the API from a Service credentials object or service key for the service instance. For more information about creating these objects, see [Connecting to {{site.data.keyword.messagehub}}](/docs/EventStreams?topic=EventStreams-connecting).

The URL for the API's endpoint is provided in the ```kafka_http_url``` property.

## Authentication
{: #rest_produce_authenticate}

The supported authentication mechanism is to use a bearer token. To obtain your token by using the IBM Cloud CLI, first log in to IBM Cloud and then run the following command: 

```sh
ibmcloud iam oauth-tokens
```
{: codeblock}

Place this token in the Authorization header of the HTTP request in the form `Bearer<token>`. Both API key or JWT tokens are supported. 

## Producing messages by using the REST producer API
{: #rest_produce_messages}

Use the v2 endpoint of the producer API to send messages of type `text`, `binary`, `JSON`, or `avro` to topics. With the v2 endpoint you can use the {{site.data.keyword.messagehub}} schema registry by specifying the schema for the avro data type.

The following code shows an example of sending a message of `text` type by using curl:

```sh
curl -v -X POST \
-H "Authorization: Bearer $token" -H "Content-Type: application/json" -H "Accept: application/json" \
-d '{
  "headers": [
    {
      "name": "colour",
      "value": "YmxhY2s="
    }
  ],
  "key": {
    "type": "text",
    "data": "Test Key"
  },
  "value": {
    "type": "text",
    "data": "Test Value"
  }
}' \
"$kafka_http_url/v2/topics/$topic_name/records"
```
{: codeblock}

For more information about the API, see [{{site.data.keyword.messagehub}} REST Producer API reference](https://cloud.ibm.com/apidocs/event-streams/restproducer){: external}.

## Producing messages that conform to a schema
{: #rest_producer_schema}

With the v2 endpoint of the REST producer API you can produce a message in such a way that the message key and value conform to a schema. You can specify different schemas for the key and value. The serializer that is supported is `confluent` and the data type supported is `avro`. The schemas are created and stored in the {{site.data.keyword.messagehub}} schema registry. For more information, see [{{site.data.keyword.messagehub}} schema registry](/docs/EventStreams?topic=EventStreams-ES_schema_registry).

The following schema naming strategies are allowed:

* Topic naming strategy: the name of the topic is used to derive the schema artifact ID. The ID takes the form "\<topicName\>-key" for key and "\<topicName\>-value" for value, where `topicName` is the name of topic.

* Record naming strategy: the name of the record in the schema is used to derive the schema artifact ID. The ID takes the form "\<composite-recordName\>-key" for key and "<composite-recordName\>-value" for value. If the schema namespace field is specified, the composite-recordName takes the value of "\<namespace\>.\<recordName\>", otherwise it takes the value of "\<recordName\>".

* TopicRecord naming strategy: Both the name of the topic and the name of the record are used to derive the schema artifact ID. The ID takes the form "\<topicName\>-\<recordName\>-key" for key and "\<topicName\>-\<composite-recordName\>-value" for value, where topicName is the name of topic. If the schema namespace field is specified, the composite-recordName takes the value of "\<namespace\>.\<recordName\>", otherwise it takes the value of "\<recordName\>".

The following code shows an example of sending a message that conforms to a schema that is specified under the `schema` field by using curl:

```sh
curl -v -X POST \
-H "Authorization: Bearer $token" -H "Content-Type: application/json" -H "Accept: application/json" \
-d '{
  "value": {
    "type": "avro",
    "schema": "{\"namespace\": \"com.eventstreams.samples\",\"type\": \"record\",\"name\": \"recordValueName\",\"fields\": [{\"name\": \"valueName\", \"type\": \"string\"}]}",
    "schema_name_strategy": "record",
    "data": "{\"valueName\": \"sampleValueName\"}"
  }
}' \
"$kafka_http_url/v2/topics/$topic_name/records?serializer=confluent"
```
{: codeblock}

## Migrating from existing endpoint to the v2 endpoint of the REST producer API
{: #migrating_endpoint}

Several improvements were made to the v2 endpoint for better usage and alignment with the API standards. To make the most of these improvements, make changes to your existing applications.

The following considerations can help you plan the migration: 

1. Accessing the REST producer API: 
    
    You can access the v2 endpoint in the same way as the existing URL, which is by obtaining the value of `kafka_http_url` property for the service instance. The path to use is `/v2/topics/<topic_name>/records`.
    
       Example URL: https://service-instance-adsf1234asdf1234asdf1234-0000.us-south.containers.appdomain.cloud/v2/topics/topic_name/records
       
2. Authentication: 
    
    The supported authentication mechanism is a bearer token. To enhance security, basic authentification by using API keys is no longer accepted.
    
       Example Header:  -H "Authorization: Bearer $token"
       
3. Headers: 
    
    Set the Content-Type and the Accept headers to `application/json`.
    
       Example Headers:  -H "Content-Type: application/json" -H "Accept: application/json"
       
4. Payload: 
    
    Provide the payload for the v2 endpoint in JSON format. The message key, headers, and data can be defined in the payload. You can specify the headers in the form of a list, with values that are base64 encoded. However, the message key and headers are optional.
    
       Example payload:
       {
         "headers": [
         {
          "name": "colour",
          "value": "YmxhY2s="
         },
         "key": {
          "type": "text",
          "data": "Test Key"
         },
         "value": {
          "type": "text",
          "data": "Test Value"
         }
        }
        
    You must specify one of the following supported data types for the field type under the key and value object:

    a.  Text: the data that is provided is validated as plain text format that consists of a linear sequence of characters.

    b.  Binary: the data that is provided is validated as base64-encoded binary format.

    c.  JSON: the data that is provided is validated as [JSON](https://www.json.org/json-en.html) format.

    d.  Avro: the data is validated as [Apache Avro data format](/docs/EventStreams?topic=EventStreams-ES_schema_registry&locale=en#ES_apache_avro_data_format).

5. Error response: 
    
    The error response contains `trace`,  `error message`, `error code`, `more_info`, and `target` properties.

       Example error response:
       {
        "trace": "a222e93c-e5f9-435d-b275-5d4919ea87ed",
        "error": {
         "code": "invalid_type",
         "message": "'type' field in 'value' object is required ...",
         "more_info": "https://cloud.ibm.com/apidocs/event-streams/restproducer_v2",
         "target": {
          "type": "field",
          "name": "type"
         }
        }
       }


## Limitations
{: #rest_producer_limitations}

When you use the REST producer API, a limitation exists on the maximum size of the message that is passed as request payload. The maximum size of the payload is limited to 64 K.

## API reference
{: #rest_api_reference}

For full details of the v2 endpoint API, see [{{site.data.keyword.messagehub}} REST Producer v2 API reference](https://cloud.ibm.com/apidocs/event-streams/restproducer_v2){: external}.

For full details of the existing endpoint API, see [{{site.data.keyword.messagehub}} REST Producer API reference](https://cloud.ibm.com/apidocs/event-streams){: external}.
