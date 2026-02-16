---
description: Read on how to request for and get status of delivery of a transaction
---

# Delivery Request

Once the transaction has been created, you will need to request initialization of transaction delivery using this API. Delivery request can be initiated in the following scenario:

* Transaction Status is in PENDING or PROCESSED state and Delivery Status is PENDING

Until delivery is requested and approved, the transaction will not be forwarded for payout.

**`PATCH v2/senders/{{senderId}}/transactions/delivery-requests/{{transactionId}}`**

{% tabs %}
{% tab title="Parameters" %}
| Field         | Required | Type   | Description                          |
| ------------- | -------- | ------ | ------------------------------------ |
| senderId      | Yes      | UUID   | Sender ID                            |
| transactionId | Yes      | UUID   | Transaction ID                       |
| status        | Yes      | string | Value must be "DELIVERY\_REQUESTED". |
| comment       | No       | string | Comment.                             |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X PATCH {{url}}/v2/senders/{{senderId}}/transactions/delivery-requests/{{transactionId}}\
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret' \
  -d { \
  "status": "DELIVERY_REQUESTED", \
  "comment": "string" \
  }
```
{% endtab %}

{% tab title="Response Sample" %}
```
2XX
```
{% endtab %}
{% endtabs %}
