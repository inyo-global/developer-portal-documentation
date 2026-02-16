---
description: Read on obtaining information on added recipient accounts
---

# Get Recipient Account

## **Get list of receive accounts** <a href="#get-list-of-receive-accounts" id="get-list-of-receive-accounts"></a>

You can fetch all the recipient accounts created under a recipient.

**`GET /v2/senders/{{senderId}}/recipients/{{recipientId}}/accounts`**

{% tabs %}
{% tab title="Parameters" %}
| Field       | Required | Type | Description  |
| ----------- | -------- | ---- | ------------ |
| senderId    | Yes      | UUID | Sender ID    |
| recipientId | Yes      | UUID | Recipient ID |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X GET {{url}}/v2/senders/{{senderId}}/recipients/{{recipientId}}/accounts \
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
      "id": "string",
      "recipient_id": "string",
      "account_number": "string",
      "account_type": "checking",
      "bank_id": 0,
      "branch_id": 0,
      "branch_location": "string",
      "flagged": false,
      "payout_method": "BANK_DEPOSIT",
      "status": "UNVERIFIED",
      "account_holder_name": {
        "first_name": "string",
        "middle_name": "string",
        "last_name": "string"
      },
    },
    {
       "id": "string",
       "recipient_id": "string",
       "identification_value": "254723993187",
       "identification_type": "TEL",
       "status": "VERIFIED",
       "flagged": false,
       "payout_method": "WALLET",
       "account_holder_name": {
        "first_name": "string",
        "middle_name": "string",
        "last_name": "string"
      },
    }

  ]
}
```
{% endtab %}
{% endtabs %}

## **Get receive account by ID** <a href="#get-receive-account-by-id" id="get-receive-account-by-id"></a>

You can fetch details of a particular recipient user account linked to a recipient.

**`GET /v2/senders/{{senderId}}/recipients/{{recipientId}}/accounts/{{accountId}}`**

{% tabs %}
{% tab title="Parameters" %}
| Field       | Required | Type | Description          |
| ----------- | -------- | ---- | -------------------- |
| senderId    | Yes      | UUID | Sender ID            |
| recipientId | Yes      | UUID | Recipient ID         |
| accountId   | Yes      | UUID | Recipient Account ID |


{% endtab %}

{% tab title="Request Sample" %}
```
curl -X GET {{url}}/v2/senders/{{senderId}}/recipients/{{recipientId}}/accounts/{{accountId}} \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
**Bank Account**

```
{
  "id": "string",
  "recipient_id": "string",
  "account_number": "string",
  "account_type": "checking",
  "bank_id": 0,
  "branch_id": 0,
  "branch_location": "string",
  "status": "UNVERIFIED",
  "flagged": false,
  "payout_method": "BANK_DEPOSIT",
  "account_holder_name": {
      "first_name": "string",
      "middle_name": "string",
      "last_name": "string"
  }
}
```

**Wallet**

```
{
  "id": "string",
  "recipient_id": "string",
  "identification_value": "254723993187",
  "identification_type": "TEL",
  "status": "VERIFIED",
  "flagged": false,
  "payout_method": "WALLET",
  "account_holder_name": {
      "first_name": "string",
      "middle_name": "string",
      "last_name": "string"
  }
}
```
{% endtab %}
{% endtabs %}
