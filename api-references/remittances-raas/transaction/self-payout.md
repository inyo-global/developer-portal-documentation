---
description: Read on how to instructions for self-payout of transactions
---

# Self-Payout

Once a transaction has been successfully created and has moved to pending state, you will be able to request for delivery through the [delivery-request API](delivery-request.md). Upon receiving the delivery request, Machnet will verify if the transaction has passed all the compliance checks. Once all checks are completed, the delivery status will be changed to \``DELIVERY_AUTHORIZED`\`.‘`transaction_delivery_authorized`’ webhook event will also be sent upon authorization.

## Update Transaction Delivery Status <a href="#update-transaction-delivery-status" id="update-transaction-delivery-status"></a>

If you have an agreement with Inyo for self-payout, once you receive the authorization, the transaction can be paid out. Depending on the results of the payout, payout status needs to be updated using the below API.

**`POST /senders/{{senderId}}/transactions/{{transactionId}}/delivery-details`**

{% tabs %}
{% tab title="Parameters" %}
| **Parameter**   | **Type**      | **Required** | **Description**                                                                                                                                                                                           |
| --------------- | ------------- | ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| date\_delivered | LocalDateTime | Yes          | <p>The time of delivery/ failure of payout</p><p>YYYY-MM-DD HH:MM:SS</p><p>e.g.2019-07-06 00:00:00</p>                                                                                                    |
| status          | String        | Yes          | <p>Delivery Status:</p><p>“DELIVERED”, “DELIVERY_FAILED”</p>                                                                                                                                              |
| note            | String        | Conditional  | <p>Note about the delivery.</p><p>1. Note is not</p><p>required in case the delivery is successful.</p><p>2. If the payout is not successful, you will need to update the failure reason in the note.</p> |
| referenceNumber | String        | No           | Reference number of the Payout Partner                                                                                                                                                                    |
| file            | Base64        | No           | Proof of delivery may be required for transactions based on compliance requirements.                                                                                                                      |
| file\_name      | String        | No           | File name                                                                                                                                                                                                 |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X POST {{url}}/senders/{{senderId}}/transactions/{{transactionId}}/delivery-details \
--header 'X-Client-Id: client_id' \
--header 'X-Client-Secret: client_secret' \
--header 'Content-Type: application/json' \
--data '{
    "date_delivered" : "2019-07-06 00:00:00",
    "status" : "delivery_status",
    "reference_number" : 5657575,
    "file_name" : "fileName.pdf",
    "file" : "base64 file"
}'
```
{% endtab %}

{% tab title="Response Sample" %}
```
204
```
{% endtab %}
{% endtabs %}
