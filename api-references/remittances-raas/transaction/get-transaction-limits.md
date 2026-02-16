---
description: Read on obtaining transaction limits for a sender
---

# Get Transaction Limits

This API provides information on the next transaction limits for the user based on the tier that the user is currently placed.

**`GET /v2/senders/{{senderId}}/transaction-limit`**

{% tabs %}
{% tab title="Parameters" %}
```
curl -X GET {{url}}/v2/senders/{{senderId}}/transaction-limit \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Request Sample" %}
| Name     | Required | Type | Description |
| -------- | -------- | ---- | ----------- |
| senderId | Yes      | UUID | Sender's ID |
{% endtab %}

{% tab title="Response Sample" %}


```
{
  "current_tier": "string",
  "sender": {
    "limit": 0
  },
  "receiver": [
    {
      "country": "string",
      "payment_type": "string",
      "limit": 0
    }
  ]
}
```

{% hint style="info" %}
The receiver information in the response of this API will be deprecated soon.
{% endhint %}
{% endtab %}
{% endtabs %}
