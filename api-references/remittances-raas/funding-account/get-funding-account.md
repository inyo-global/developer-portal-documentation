---
description: Read on obtaining information on funding accounts added by a user
---

# Get Funding Account

**Get list of all funding accounts**

You can view all the fundings accounts added by a sender.

**`GET /v2/senders/{{senderId}}/funding-sources`**

{% tabs %}
{% tab title="Parameters" %}
| Field    | Required | Type | Description |
| -------- | -------- | ---- | ----------- |
| senderId | Yes      | UUID | Sender ID.  |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X GET {{url}}/v2/senders/{{senderId}}/funding-sources \
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
      "sender_id": "string",
      "funding_source_name": "string",
      "institution_name": "string",
      "account_type": "string",
      "account_holder_name": "string",
      "verification_status": "string",
      "funding_source_type": "BANK"
    } , 
  {
      "id": "string",
      "sender_id": "string",
      "nick_name": "string",
      "funding_source_name": "string",
      "institution_name": "string",
      "funding_source_type": "CARD",
      "regulated": false,
      "network": "string",
      "avs_code": "Y",
      "duplicate_flagged": true,
      "expiry_status": "ACTIVE",
      "card_type": "DEBIT"
     }
  ]
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
For fetching all accounts according to funding source type, you can pass a query parameter.

For banks, `GET /v2/senders/{{senderId}}/funding-sources?type=BANK`

For cards, `GET /v2/senders/{{senderId}}/funding-sources?type=CARD`
{% endhint %}

**Get funding account by ID**

You can get details of a particular funding account of a sender.

**`GET /v2/senders/{{senderId}}/funding-sources/{{fundingAccountId}}`**

{% tabs %}
{% tab title="Parameters" %}
| Field            | Required | Type | Description         |
| ---------------- | -------- | ---- | ------------------- |
| senderId         | Yes      | UUID | Sender ID.          |
| fundingAccountId | Yes      | UUID | Funding Account ID. |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X GET {{ul}}/v2/senders/{{senderId}}/funding-sources/{{fundingSourceId}} \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
**Bank**

```
{
    "id": "string",
    "sender_id": "string",
    "funding_source_name": "string",
    "institution_name": "string",
    "account_type": "string",
    "account_holder_name": "string",
    "verification_status": "string",
    "funding_source_type": "BANK"
}
```

**Card**

```
{
    "id": "string",
    "sender_id": "string",
    "nick_name": "string",
    "funding_source_name": "string",
    "institution_name": "string",
    "funding_source_type": "CARD",
    "regulated": false,
    "network": "string",
    "avs_code": "Y",
    "expiry_status": "EXPIRING_SOON",
    "card_type": "DEBIT",
    "duplicate_flagged": true,
    "duplicate_reasons": [
        "string"
    ],
    "duplicate_account_info": [
        {
            "sender_id": "string",
            "sender_name": "string",
            "funding_account_id": "string",
            "funding_account_name": "string"
        }
    ]
}
```
{% endtab %}
{% endtabs %}
