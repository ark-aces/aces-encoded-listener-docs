
# API Reference


# Subscriptions

The Subscribers endpoint allows subscriber to register their node to receive 
blockchain events from the Encoded Listener.

## Create a Subscription

> Example create Subscription request:

```shell
curl 'http://localhost/subscriptions' \
  -X POST \
  -d '{
    "callbackUrl": "http://consumer.domain.com/handler",
    "minConfirmations": 5
  }'
```

```java
EncodedListenerClient client = new EncodedListenerClient("http://localhost");

SubscriptionRequest request = new SubscriptionRequest();
request.setCallbackUrl("http://consumer.domain.com/handler");
request.setMinConfirmations(5);

Subscription subscription = client.createSubscription(request);
```

> Returned response:

```json
{
  "id": "329857298735983",
  "createdAt": "2017-11-08T05:34:54.267Z",
  "status": "active"
}
```

The request to create a new Subscription.

### Parameters

Parameter        | Type    | Description 
---------------- | ------- | ----------- 
callbackUrl      | string  | Target target URL to POST Encoded Listener events to.
minConfirmations | integer | Confirmations required before event is sent to subscriber.

### HTTP Request

`POST http://localhost/subscriptions`


## Get a Subscription

> Example get Subscription request:

```shell
curl 'http://localhost/subscriptions/1c2ddeb7-cf74-478d-8f77-2f326b8c2db4'
```

```java
EncodedListenerClient client = new EncodedListenerClient("http://localhost:8080");
Subscription subscription = client.getSubscription("1c2ddeb7-cf74-478d-8f77-2f326b8c2db4");
```

> Returned response:

```json
{
  "id": "1c2ddeb7-cf74-478d-8f77-2f326b8c2db4",
  "callbackUrl": "http://consumer.domain.com/handler",
  "minConfirmations": "5"
}
```

Get a Subscription by identifier.

### Parameters

Parameter       | In   | Type    | Required | Description             
--------------- | ---- | ------- | -------- | ----------------------- 
id              | path | string  | True     | Subscription identifier

### HTTP Request
  
`GET http://localhost:8080/subscriptions/{id}`










# Event

## List Subscription Events

> Example list Subscriptions request:

```shell
curl 'http://localhost/subscriptions/1c2ddeb7-cf74-478d-8f77-2f326b8c2db4/events'
```

```java
EncodedListenerClient client = new EncodedListenerClient("http://localhost");
Page<Subscription> subscriptionsPage = 
    client.getSubscriptionsPage("1c2ddeb7-cf74-478d-8f77-2f326b8c2db4");
```

> Returned response:

```json
{
  "pageSize": "100",
  "page": "0",
  "continuation": "933502938509238509283059",
  "items": [
    {
      "id": "329857298735983",
      "createdAt": "2017-11-08T05:34:54.267Z",
      "status": "delivered",
      "tries": "1",
      "data": {
          "hex": "01000000019a9aa57825106e72c92c0c771b88a9f7bbdd399d12e405e29757964f9a387ef0000000006b483045022100fdac2e51068717da7f564ae676d84f04aa6e5157b72c168301809518bc8e733902200b39ba9d0ee8cd0c5f0ed7a4d51ec2aaf5976252aeca6ffd5b9794076898c463012102892589b5b0e2751bd2500a71f06b2d851439678eb0e976be5b5a0cc8f3e49895ffffffff021caa3900000000001976a914ddaccd2403cfffad5936ca66c2c6a7c98146936888ac9b593001000000001976a91461752641b0bf1cecd08341224f83e690853abd0588ac00000000",
          "txid": "fdcd3564c09ddd33dafd9d1bd2f5dc583f8c818fff631397c7f6d0d12ecf17b4",
          "hash": "fdcd3564c09ddd33dafd9d1bd2f5dc583f8c818fff631397c7f6d0d12ecf17b4",
          "size": 226,
          "vsize": 226,
          ...
        }
    }
  ]
}
```

Gets a page of Subscription Events.

### Parameters

Parameter       | In    | Type    | Required | Default | Description             
--------------- | ----  | ------- | ----     | ----    | ----------------------- 
name            | path  | string  | True     |         | Subscription identifier 
pageSize        | query | integer |          | 100     | Number of items to return per page. 
page            | query | integer |          |         | Zero-offset page number to return.
continuation    | query | string  |          |         | Continuation param for fetching next page.

### HTTP Request

`GET http://localhost/subscriptions/{id}/events`









# Unsubscribe

## Create an Unsubscribe

```shell
curl -X POST 'http://localhost/subscriptions/{id}/unsubscribes'
```

```java
EncodedListenerClient client = new EncodedListenerClient("http://localhost");
Unsubscribe unsubscribe = 
    client.createUnsubscription("1c2ddeb7-cf74-478d-8f77-2f326b8c2db4");
```

> Returned response:

```json
{
  "id": "329857298735983",
  "created_at": "2017-11-08T05:34:54.267Z"
}
```

Unsubscribes an active Subscription.

### Parameters

Parameter       | In    | Type    | Required | Description             
--------------- | ----  | ------- | ----     | ----------------------- 
id              | path  | string  | True     | Subscription Identifier

### HTTP Request

`POST http://localhost/subscriptions/{id}/unsubscribes`





# Health

## Get Health of node.

> Example get Health request:

```shell
curl -X GET 'http://localhost/status'
```

```java
EncodedListenerClient client = new EncodedListenerClient("http://localhost");
Health health = client.getHealth();
```

> Returned response:

```json
{
  "status": "up"
}
```

Get application health information.


### HTTP Request

`GET http://localhost/status`


