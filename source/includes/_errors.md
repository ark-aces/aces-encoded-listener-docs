# Errors

ACES Encoded Listener API instances can return a set of standard HTTP errors classified by
the response HTTP Status code.

An request body validation error returns a `400` HTTP Status Code and an error response body
with a detailed description of field validation errors. Consumers should correct the request
before resubmitting the request.

Any `5xx` level errors can be retried without making changes to the request since they
are returned when there is something wrong with the server and not the consumer's request.


### Status Codes

ACES Encoded Listener API instances return errors responses with the following HTTP Status codes:

HTTP Code | Description
---------- | -----------
400 | The request was invalid. The consumer should read error messages and correct the request body before resending request.
404 | The requested resource was not found.
405 | The URL in the request does not support the requested HTTP method.
429 | Too many requests have been sent over a given time frame and the server has limited the consumer.
500 | The server had an unexpected error. Consumers should try the request again later.
503 | The server is running, but not currently accepting traffic. Consumers should try the request again later.


### General Errors

> Example error response:

```json
{
  "code": "alreadyUnsubscribed",
  "message": "The Subscription has already been unsubscribed."
}
```

Errors always return an error code and human readable error message.
Consumers should handle any expected errors for an API call using the error `code` value.



### General Error Codes

Code | Description
---------- | -------
notFound   | The requested resource was not found.
invalidRequest    | The request body is invalid. See `Validation Errors` below.



### Validation Errors

> Example error request:

```shell
curl 'http://localhost/subscriptions' \
  -X POST \
  -d '{
     "callbackUrl": "not-a-url"
  }'
```

> Example error response:

```json
{
  "code": "invalidRequest",
  "message": "Request is invalid.",
  "fieldErrors": [
    {
      "field": "callbackUrl",
      "code": "invalidUrl",
      "message": "The url given is invalid."
    },
    {
      "field": "minConfirmations",
      "code": "required",
      "message": "Min confirmations is required."
    }
  ]
}
```

Validation errors have http status code = `400` and a response body error code = `invalidRequest`.
Any field validation errors are returned in the `fieldErrors` list and include the field name corresponding
to the request parameter, validation error code, and human-readable message for the field error.


### Field Error Codes

Code | Description
---------- | -------
required   | The field value is required.
invalidUrl | The given URL value is invalid.
tooShort   | The field value given is too short. 
tooLong    | The field value given is too long.
