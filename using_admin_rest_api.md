---

copyright:
  years: 2015, 2024
lastupdated: "2024-03-01"

keywords: admin rest api, administration, api, basic auth, bearer token, authenticate, api key

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Using the Administration REST API
{: #admin_api}

{{site.data.keyword.messagehub}} provides an Administration RESTful API that you can use to create, delete, list, and update topics.
{: shortdesc}

You must retrieve the URL and credential details that are needed to connect to the API from a service credentials object or service key for the service instance. For information about creating these objects, see [Connecting to {{site.data.keyword.messagehub}}](/docs/EventStreams?topic=EventStreams-connecting).

The URL for the API's endpoint is provided in the `kafka_admin_url` property.

The credentials depend on the authentication method and three types of credential are supported.

## Authenticate by using Basic Auth
{: #admin_api_basic_auth}

Use the `user` and `api_key` properties of the objects as the username and password. Place these values into the `Authorization` header of the HTTP request in the form `Basic <base64 encoding of username and password joined by a single colon (:)>`.
    
## Authenticate by using a bearer token
{: #admin_api_bearer_token}

To obtain your token by using the IBM Cloud CLI, first log in to IBM Cloud then run the following command: 

```sh
ibmcloud iam oauth-tokens
```
{: codeblock}

Place this token in the Authorization header of the HTTP request in the form `Bearer <token>`. Both API key or JWT tokens are supported. 
    
## Authenticate by using the api_key
{: #admin_api_api_key}

Place the key directly as the value of the `X-Auth-Token` HTTP header.

## Next steps
{: #admin_api_next_steps}

For a description of the API with examples, see [{{site.data.keyword.messagehub}} admin-rest](https://github.com/ibm-messaging/event-streams-docs/tree/master/admin-rest-api){: external}.

You can download the full specification for the API from the [{{site.data.keyword.messagehub}} Admin REST API YAML file](https://github.com/ibm-messaging/event-streams-docs/blob/master/admin-rest-api/admin-rest-api.yaml){: external}. To view the swagger file, use Swagger tools, for example [Swagger editor](https://editor.swagger.io:443/){: external}.
