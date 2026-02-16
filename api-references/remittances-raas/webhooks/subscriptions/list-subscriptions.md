---
description: Read on obtaining list of all subscribed webhooks
---

# List Subscriptions

**`GET /v2/subscriptions`**

{% tabs %}
{% tab title="Request Sample" %}
```
curl -X GET {{url}}/v2/subscriptions
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
  "results": [
    {
      "id": "string",
      "end_point": "string",
      "secret": "string",
      "active_from": "2019-07-04T04:45:05.125Z",
      "expired_at": "2019-07-04T04:45:05.126Z",
      "is_paused": false,
      "is_active": true
    }
  ]
}
```
{% endtab %}
{% endtabs %}

```
GET /v2/subscriptions?isActive=true
```

{% tabs %}
{% tab title="Parameters" %}
| Field    | Required | Type    | Description                                                                                   |
| -------- | -------- | ------- | --------------------------------------------------------------------------------------------- |
| isActive | No       | Boolean | If true, only active webhooks are listed. If false or null all registered webhook are listed. |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X GET {{url}}/v2/subscriptions?isActive=true
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
  "results": [
    {
      "id": "string",
      "end_point": "string",
      "secret": "string",
      "active_from": "2019-07-04T04:45:05.125Z",
      "expired_at": "2019-07-04T04:45:05.126Z",
      "is_paused": false,
      "is_active": true
    }
  ]
}
```
{% endtab %}
{% endtabs %}
