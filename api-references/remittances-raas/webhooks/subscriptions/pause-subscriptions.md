---
description: Read on pausing subscriptions
---

# Pause Subscriptions

**`POST /v2/subscriptions/{{subscriptionId}}`**

{% tabs %}
{% tab title="Parameters" %}
| Name           | Required    | Type    | Description                           |
| -------------- | ----------- | ------- | ------------------------------------- |
| subscriptionId | Yes, in URL | UUID    | Subscription ID.                      |
| pause          | Yes         | boolean | Set to true if subscription is paused |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X POST {{ulr}}/v2/subscriptions/{{subscriptionId}} \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret' \
  -d { \
      "pause": true \
  }
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
  "id": "string",
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
