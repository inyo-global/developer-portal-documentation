---
description: Read on how to get Webhooks
---

# Subscribe

Create a webhook subscription to receive POST requests from the platform (called webhooks) when events associated with your application occur. Webhooks are sent to a URL which you provide when creating a webhook subscription.

**`POST /v2/subscriptions`**

{% tabs %}
{% tab title="Parameters" %}
| Field      | Required | Type   | Description                      |
| ---------- | -------- | ------ | -------------------------------- |
| end\_point | Yes      | URL    | Subscription URL.                |
| secret     | Yes      | secret | String used to hash the message. |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X POST {{url}}/v2/subscriptions \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret' \
  -d { \
      "end_point": "string", \
      "secret": "string" \
  }
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
  "id": "uuid",
  "end_point": "string",
  "secret": "string",
  "active_from": "2019-07-04T04:45:05.125Z",
  "expired_at": "2019-07-04T04:45:05.126Z",
  "is_paused": false,
  "is_active": true
}
```
{% endtab %}
{% endtabs %}
