---
description: Details on cancelling a transaction that has been created in our system
---

# Cancel Transaction

Bank transactions that have not been submitted for processing can be canceled using this API. You will receive appropriate error messages if a transaction cannot be canceled.

Transactions can be cancelled in INITIATED, PENDING and HOLD status.

**`DELETE /v2/senders/{{senderId}}/transactions/{{transactionId}}`**

{% tabs %}
{% tab title="Parameters" %}
| Field         | Required | Type | Description     |
| ------------- | -------- | ---- | --------------- |
| senderId      | Yes      | UUID | Sender ID.      |
| transactionId | Yes      | UUID | Transaction ID. |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X DELETE {{url}}/v2/senders/{{senderId}}/transactions/{{transactionId}} \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
Body is empty. (2XX)
```
{% endtab %}
{% endtabs %}
