---
description: Read for details on added receivers
---

# Get Recipient Details

## Get All Recipients <a href="#get-all-recipients" id="get-all-recipients"></a>

This API returns all recipients associated with the sender along with the recipient's information.

**`GET /v2/senders/{{senderId}}/recipients`**

{% tabs %}
{% tab title="Parameters" %}
| Field    | Required | Type | Description |
| -------- | -------- | ---- | ----------- |
| senderId | Yes      | UUID | Sender ID   |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X GET {{url}}/v2/senders/{{senderId}}/recipients \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
  "result": [
    {
      "id": "UUID",
      "sender_id": "string",
      "first_name": "string",
      "middle_name": "string",
      "last_name": "string",
      "country": "string",
      "address_line1": "string",
      "address_line2": "string",
      "city": "string",
      "postal_code": "string",
      "province": "string",
      "email": "string",
      "document": "string",
      "home_phone": "string",
      "mobile_phone": "string",
      "work_phone": "string",
      "date_of_birth": "string",
      "occupation": "string",
      "sender_relationship": "string",
      "status": "string"
    }
  ]
}
```
{% endtab %}
{% endtabs %}

## Get recipient by Id <a href="#get-recipient-by-id" id="get-recipient-by-id"></a>

Returns information of a specific recipient of the sender.

**`GET /v2/senders/{{senderId}}/recipients/{{recipientId}}`**

{% tabs %}
{% tab title="Parameters" %}
| Field       | Required | Type | Description                   |
| ----------- | -------- | ---- | ----------------------------- |
| senderId    | Yes      | UUID | ID of sender                  |
| recipientId | Yes      | UUID | ID of recipient of the sender |


{% endtab %}

{% tab title="Request Sample" %}
```
curl -X GET {{url}}/v2/senders/{{senderId}}/recipients/{{recipientId}} \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
  "id": "UUID",
  "sender_id": "string",
  "first_name": "string",
  "middle_name": "string",
  "last_name": "string",
  "country": "string",
  "address_line1": "string",
  "address_line2": "string",
  "city": "string",
  "postal_code": "string",
  "province": "string",
  "email": "string",
  "document": "string",
  "home_phone": "string",
  "mobile_phone": "string",
  "work_phone": "string",
  "date_of_birth": "string",
  "occupation": "string",
  "sender_relationship": "string",
  "status": "string",
  "available_payout_methods": [
       "BANK_DEPOSIT",
       "CASH_PICKUP",
       "WALLET",
       "HOME_DELIVERY"
    ]
}
```
{% endtab %}
{% endtabs %}
