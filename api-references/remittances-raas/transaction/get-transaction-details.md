# Get Transaction Details

## Get All Transactions of a Sender <a href="#get-all-transactions-for-sender" id="get-all-transactions-for-sender"></a>

This API provides the list of all the transactions associated with a particular sender.

**`GET /v2/senders/{{senderId}}/transactions`**

ParametersRequest SampleResponse Sample

{% tabs %}
{% tab title="Parameters" %}
| Field    | Required | Type | Description |
| -------- | -------- | ---- | ----------- |
| senderId | Yes      | UUID | Sender ID.  |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X GET {{url}}/v2/senders/{{senderId}}/transactions \
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
      "sender_amount": 0.11,
      "exchange_rate": 0.1111,
      "recipient_amount": 0.01,
      "fee_amount": 0,
      "bonus_amount": 0.00,
      "recipient_currency": "string",
      "funding_source": "BANK",
      "sender_id": "string",
      "sender_funding_account_id": "string",
      "recipient_id": "string",
      "payer_id": 1,
      "payout_method": "CASH_PICKUP",
      "note": "string",
      "remittance_purpose": "string",
      "ip_address": "string",
      "status": "string",
      "delivery_status": "string",
      "reference_number": "string",
      "payout_reference_number" : "string",
      "risk_score": 0
    },
    {
      "id": "string",
      "sender_amount": 0.11,
      "exchange_rate": 0.1111,
      "recipient_amount": 0.01,
      "fee_amount": 0,
      "bonus_amount": 0.00,
      "recipient_currency": "string",
      "funding_source": "CARD",
      "sender_id": "string",
      "sender_funding_account_id": "string",
      "recipient_id": "string",
      "recipient_account_id": "string",
      "payout_method": "BANK_DEPOSIT",
      "note": "string",
      "remittance_purpose": "string",
      "ip_address": "string",
      "status": "HOLD",
      "delivery_status": "HOLD",
      "reference_number": "string",
      "risk_score": 0,
      "three_ds": {
        "enabled": true,
        "status": "VERIFIED"
      },
      "hold_reasons": [
                        {
                          "code": "T002",
                          "message": "Service not supported in the location"
                        }
                      ]
    },
    {
      "id": "string",
      "sender_amount": 0.11,
      "exchange_rate": 0.1111,
      "recipient_amount": 0.01,
      "fee_amount": 0,
      "bonus_amount": 0.00,
      "recipient_currency": "string",
      "funding_source": "CARD",
      "sender_id": "string",
      "sender_funding_account_id": "string",
      "recipient_id": "string",
      "recipient_account_id": "string",
      "payout_method": "WALLET",
      "note": "string",
      "remittance_purpose": "string",
      "ip_address": "string",
      "status": "string",
      "delivery_status": "string",
      "reference_number": "string",
      "risk_score": 0,
      "three_ds": {
        "enabled": true,
        "status": "string"
      },
    },
    {
      "id": "string",
      "sender_amount": 0.11,
      "exchange_rate": 0.1111,
      "recipient_amount": 0.01,
      "fee_amount": 0,
      "bonus_amount": 0.00,
      "recipient_currency": "string",
      "funding_source": "BANK",
      "sender_id": "string",
      "sender_funding_account_id": "string",
      "recipient_id": "string",
      "payer_id": 2,
      "payout_method": "HOME_DELIVERY",
      "note": "string",
      "remittance_purpose": "string",
      "ip_address": "string",
      "status": "string",
      "delivery_status": "string",
      "reference_number": "string",
      "risk_score": 0
    }

  ]
}
```
{% endtab %}
{% endtabs %}

## Get Transaction by ID <a href="#get-transaction-by-id" id="get-transaction-by-id"></a>

This API can be used to fetch a particular transaction's information.

**`GET /v2/senders/{{senderId}}/transactions/{{transactionId}}`**

{% tabs %}
{% tab title="Parameters" %}
| Field         | Required | Type | Description     |
| ------------- | -------- | ---- | --------------- |
| senderId      | Yes      | UUID | Sender ID.      |
| transactionId | Yes      | UUID | Transaction ID. |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X GET {{url}}/v2/senders/{{senderId}}/transactions/{{transactionId}} \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
  "id": "string",
  "sender_amount": 0.11,
  "exchange_rate": 0.1111,
  "recipient_amount": 0.01,
  "fee_amount": 0,
  "bonus_amount": 0.00,
  "recipient_currency": "string",
  "sender_id": "string",
  "sender_funding_account_id": "string",
  "funding_source": "string",
  "recipient_id": "string",
  "recipient_account_id": "string",
  "note": "string",
  "remittance_purpose": "string",
  "ip_address": "string",
  "status": "string",
  "delivery_status": "string",
  "reference_number": "string",
  "risk_score": 0,
  "receipt_number": "string",
  "payout_method": "BANK_DEPOSIT",
  "status_history":  [
  { 
  "changed_at": "2023-04-28 05:29:47",  
  "status": "INITIATED" 
  },
  { 
  "changed_at": "2023-04-28 05:31:03",
  "status": "PENDING" 
  },  
  { 
  "changed_at": "2023-04-28 05:31:34",  
  "status": "PROCESSED",  
  "comment": "Transaction completed processing successfully."  
  }
  ],  
  "authorization_code": "053134" 
}
```
{% endtab %}
{% endtabs %}
