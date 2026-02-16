---
description: Read on obtaining wallet/bank account details
---

# Get Account Information

This API provides information about account holder from the payer/bank if available. It can be mostly used to obtain wallet account holder name and status.

**`POST /v2/senders/{{senderId}}/recipients/{{recipientId}}/accounts/information`**

{% tabs %}
{% tab title="Parameters" %}
| Field                 | Required                              | Type   | Description                                                                                                                                                                                                                                                                                                                                                                                              |
| --------------------- | ------------------------------------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| senderId              | Yes, in URL                           | UUID   | Sender ID                                                                                                                                                                                                                                                                                                                                                                                                |
| recipientId           | Yes, in URL                           | UUID   | Recipient ID                                                                                                                                                                                                                                                                                                                                                                                             |
| identification\_value | Yes, for WALLET payout\_method        | String | Recipient wallet id                                                                                                                                                                                                                                                                                                                                                                                      |
| identification\_type  | No                                    | String | Wallet type. Enumerated value: `TEL`. The default value for identification\_type is `TEL`                                                                                                                                                                                                                                                                                                                |
| account\_number       | Yes, for BANK\_DEPOSIT payout\_method |        |                                                                                                                                                                                                                                                                                                                                                                                                          |
| payout\_method        | Yes                                   | String | Payout method type. Enumerated value: `WALLET, BANK_DEPOSIT`                                                                                                                                                                                                                                                                                                                                             |
| recipient\_id         | Response                              | UUID   | Recipient ID                                                                                                                                                                                                                                                                                                                                                                                             |
| payload               | Response                              | Object |                                                                                                                                                                                                                                                                                                                                                                                                          |
| payload.status        | Response                              | String | <p>Status of the recipient wallet. <code>AVAILABLE</code>, <code>UNAVAILABLE</code>, <code>UNREGISTER</code>, <code>SERVICE_NOT_PRESENT</code></p><p><strong>Details on the above values</strong></p><p>AVAILABLE : The wallet/bank is available. UNAVAILABLE: The wallet/bank is unavailable.</p><p>UNREGISTER : The MSISDN is not active. SERVICE_NOT_PRESENT : This API service is not available.</p> |
| payload.full\_name    | Response                              | String | Full name of the account holder                                                                                                                                                                                                                                                                                                                                                                          |
| payload.identifier    | Response                              | String | Recipient wallet ID                                                                                                                                                                                                                                                                                                                                                                                      |


{% endtab %}

{% tab title="Request Sample" %}
```
curl --location --request POST {{url}}/v2/senders/{{senderId}}/recipients/{{recipientId}}/accounts/information' \
--header 'X-Client-Id: clientid' \
--header 'X-Client-Secret: clientsecret' \
--header 'Content-Type: application/json' \
--data-raw '{
  "identification_value":"254723993187",
  "identification_type": "TEL",
  "payout_method": "WALLET"
  }
'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
    "recipient_id": "string",
    "payout_method": "WALLET",
    "payload": {
        "status": "AVAILABLE",
        "full_name": "JOSEPH MBUGUA NJEHU",
        "identifier": "254723993187"
    }
}
```
{% endtab %}
{% endtabs %}
