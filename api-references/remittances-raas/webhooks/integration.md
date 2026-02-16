---
description: Read on how to deal with our webhooks
---

# Integration

After the Subscription API has been setup the webhooks is ready to use. Webhooks will be fired when any event listed is triggered from our end. We will trigger a POST request to the URL provided on the Subscription API.

## **Webhooks Request** <a href="#webhooks-request" id="webhooks-request"></a>

When an event is created on our end we POST following details as part of the webhook to the URL mentioned on Subscription API.

**HEADER x-raas-webhook-signature :** d2b730bba0de481fb079fff1478435231a9b410005ee599e67428930b7f340c3 x-raas-event : transaction\_completed POST PAYLOAD

```
{
  "persisted_object_id": "82646f2c-447a-4214-a6ad-7d43e87d7fc4",
  "resource_id": "dab8da2c-61ea-42cf-a7c5-223967bebb81",
  "sender_id": "ee3aa0ac-8d18-4a7b-adaf-c5cc8a8db62f",
  "event_name": "transaction_completed",
  "subscription_id": "caec6725-bf0a-4ca4-9f8c-153f4da87259",
  "timestamp": "2017-07-02T22:88:12.120Z"
}
```

### **Webhook PayLoad Request For Receiver Profile Edit**

```
{
  "persisted_object_id": "185bb745-ca07-4e49-984c-7573fd1230b1",
  "resource_id": "4c737c13-a452-4f39-82b1-83c15369bcdd",
  "sender_id": "534a5efe-64f8-4dc6-a6ce-14cfb213e1ab",
  "receiver_id": "38297cec-cb9c-4af6-aff7-f1334990b73c",
  "event_name": "receiver_profile_edit_submitted",
  "subscription_id": "e85db7bc-c14a-484b-8ce9-6d6b48fda4f0",
  "timestamp": "2021-09-01T09:29:38.969"
}
```

## Webhook Details <a href="#webhook-details" id="webhook-details"></a>

| Parameter                | Description                                                                                                                                                                                    |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| x-raas-webhook-signature | We generate a signature using the secret mentioned on Subscription API.                                                                                                                        |
| x-raas-event             | Event Name                                                                                                                                                                                     |
| persisted\_object\_id    | Webhook unique identifier                                                                                                                                                                      |
| event\_name              | Event name. Same as that of header x-raas-event                                                                                                                                                |
| resource\_id             | Id of resource for which the event was generated. If transaction\_completed is triggered the resource\_id will be transaction id.You can fetch the particular resource using the resource\_id. |
| sender\_id               | If sender related resources are triggered as part of event then sender\_id will be sent as part of the payload.                                                                                |
| subscription\_id         | Subscription Id for which this event was generated.                                                                                                                                            |
| receiver\_id             | If receiver related resources are triggered as part of an event then receiver\_id will be sent as part of the payload. This is only visible in the receiver profile edit webhook.              |
| timestamp                | Timestamp when the webhook is posted                                                                                                                                                           |

## **Securing Webhooks** <a href="#securing-webhooks" id="securing-webhooks"></a>

We allow you to set secret as part of the Subscription API. Secret used on Subscription API will be used to create hash which will be sent as part of the webhooks request i.e. x-raas-webhook-signature and is a SHA256 HMAC hash of the request body with the key being your webhooks secret. You can validate the webhooks request by generating the same SHA256 HMAC hash and comparing it to the x-raas-webhook-signature sent with the payload. This step is optional but we highly recommend you to do so.

## **Responding to Webhooks** <a href="#responding-to-webhooks" id="responding-to-webhooks"></a>

When you receive the webhooks events, you can respond back with following HTTP Status after the processing has been completed on your end.

| HTTP Status        | Description                                                                                                                                                                                                                                                                                         |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2xx HTTP           | This will acknowledge that the webhooks event was successfully captured and no further webhooks event will be generated from our end.                                                                                                                                                               |
| 409 Conflict       | If this HTTP code is returned from your end, we will trigger the webhooks on fixed interval until a success response is received from your end.                                                                                                                                                     |
| Rest of HTTP Codes | Any other response during webhooks response including 3xx codes will be marked as failure. Consecutive Webhooks Failures will pause the subscriptions for which the webhooks failed. You will have to update the paused subscriptions if you want to receive further webhooks on that subscription. |

## **Retry Case** <a href="#retry-case" id="retry-case"></a>

If a webhooks is not successfully received for any reason, we will continue trying to send the webhooks 10 more times every hour.

## **Duplicate Events** <a href="#duplicate-events" id="duplicate-events"></a>

Subscriptions endpoints may occasionally receive the same event more than once. We advise you to properly handle such cases on your end.

{% hint style="info" %}
Along with the webhooks, we recommend polling of our service.
{% endhint %}
