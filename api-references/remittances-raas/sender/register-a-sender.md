---
description: Read on how to create a new sender
---

# Register a Sender

This endpoint creates a new sender with just the user's basic information. Only users in originating corridors enabled for you can be created with send capabilities. A unique ID will be generated as a response which will be required to initialize the widget for KYC verification, adding funding accounts, creating receivers for the sender and creating transactions by the sender.

**`POST /v2/senders`**

{% tabs %}
{% tab title="Request Sample" %}
```
curl -X POST {{url}}/v2/senders \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret' \
  -d { \
      "first_name": "string", \
      "middle_name": "string", \
      "last_name": "string", \
      "mobile_phone": "string", \
      "email": "user@example.com", \
      "state": "string" \
      "nationality": "string" \
  }
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
  "id": "UUID",
  "first_name": "string",
  "middle_name": "string",
  "last_name": "string",
  "mobile_phone": "string",
  "email": "user@example.com",
  "state": "string",
  "current_tier": "string",
  "status": "string",
  "nationality": "string" 
}
```
{% endtab %}
{% endtabs %}
