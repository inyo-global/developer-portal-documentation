---
description: Read on obtaining information about a registered sender
---

# Get Sender by ID

You can retrieve detailed information about a particular user using their user ID.

`GET /v2/senders/{{senderId}}`

{% tabs %}
{% tab title="Parameters" %}
| Name     | Required | Type | Description             |
| -------- | -------- | ---- | ----------------------- |
| senderId | Yes      | UUID | Unique ID of the sender |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X GET {{url}}/v2/senders/{{senderId}} \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
  "id": "UUID",
  "first_name": "string",
  "middle_name": "string",
  "last_name": "string",
  "email": "user@example.com",
  "state": "string",
  "current_tier": "string",
  "status": "string",
  "mobile_phone": "string",
  "requested_fields": [
          "SENDER_SOURCE_OF_FUND"
      ],
  "cip_failure_reasons": [
          "No public record found"
      ]
}
```
{% endtab %}
{% endtabs %}
