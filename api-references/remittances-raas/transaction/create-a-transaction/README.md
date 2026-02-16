---
description: Read for details on creating transaction
---

# Create a Transaction

This API can be used to initiate a transfer. A transaction can be created only after the following criteria are fulfilled.

* Sender's KYC status is other than UNVERIFIED or SUSPENDED.
* At least one sender funding account has been added by the sender.
* At least one Recipient has been added.

For transactions with funding\_source=`BANK`, a sender funding account with type=`BANK` must be verified and active. For transactions with funding\_source=`CARD`, a sender funding account with type=`CARD` must be active.

For transaction amount delivery, the recipient can either receive the amount directly in their bank account i.e. payout\_method=`BANK_DEPOSIT`, get the amount deposited in their wallet i.e. payout\_method= `WALLET` get the amount delivered to the recipient’s address i.e. payout\_method = `HOME_DELIVERY`, or the recipient can pickup the amount from various payout locations i.e. payout\_method=`CASH_PICKUP`.

[For payout\_method=`BANK_DEPOSIT`, the recipients bank account information must be added and for payout\_method=`WALLET`, the recipient wallet account information must be added. ](../../recipient-account/)However, for payout\_method=`CASH_PICKUP` and payout\_method = `HOME_DELIVERY`, the payer\_id must be specified during transaction creation. You can get the list the payers available for the recipient's country through the [payers API](../../data-population/payers.md).

Supports Idempotency Key as part of header to avoid duplicate transaction.

**`POST /v2/senders/{{senderId}}/transactions`**

{% hint style="info" %}
Supports Idempotency Key as part of header to avoid duplicate transaction. If you are creating a transaction with the same request and the same idempotency key again, the endpoint will consider this a duplicate request. It will not create a new transaction but will instead provide details of the initial transaction in the response.

**Response**

200 if a transaction is successfully created or transaction with the same idempotency key and request was created earlier

400 if the idempotency key is greater than 255 char

409 if the idempotency key is the same but the request is different
{% endhint %}

{% tabs %}
{% tab title="Parameters" %}
| Field                        | Required    | Type   | Description                                                                                                                                                            |
| ---------------------------- | ----------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| senderId                     | Yes, in URL | UUID   | Sender ID                                                                                                                                                              |
| sender\_funding\_account\_id | Yes         | UUID   | Sender's Funding Account ID                                                                                                                                            |
| sender\_amount               | Yes         | number | Amount sender wants to send                                                                                                                                            |
| fee\_amount                  | Yes         | number | Transaction fee to be charged to the sender                                                                                                                            |
| bonus\_amount                | No          | number | Bonus on the transaction. Refer to [this](https://raas.docs.machnetinc.com/api-references/transaction/create-a-transaction/bonus-discount) for additional information. |
| ip\_address                  | Yes         | String | IP address used to create the transaction                                                                                                                              |
| recipient\_id                | Yes         | UUID   | Recipient's ID                                                                                                                                                         |
| recipient\_account\_id       | No          | UUID   | Recipient's Account ID. Recipient account can be recipient bank account or recipient wallet account.                                                                   |
| funding\_source              | No          | String | Enumerated Value: `BANK` or `CARD`. It should be based on the sender funding account type. For now, default value =`BANK`                                              |
| payer\_id                    | No          | long   | Payer ID. Required if payout\_method=`CASH_PICKUP` or `HOME_DELIVERY`.                                                                                                 |
| payout\_method               | No          | String | Enumerated Value: `CASH_PICKUP` or `BANK_DEPOSIT` or `WALLET` or `HOME_DELIVERY`. Payout method for transaction amount delivery. Default value =`BANK_DEPOSIT`         |
| exchange\_rate               | Yes         | number | Exchange rate used in transaction. Accepts upto 4 decimal digits.                                                                                                      |
| recipient\_amount            | Yes         | number | Amount that recipient receives                                                                                                                                         |
| recipient\_currency          | Yes         | String | Currency of recipient currency                                                                                                                                         |
| remittance\_purpose          | Yes         | String | Purpose of remittance                                                                                                                                                  |
| note                         | No          | String | Note.                                                                                                                                                                  |
| remarks                      | No          | String | Remarks.                                                                                                                                                               |
| calculation\_mode            | No          | String | <p>Enumerated value : SENDER_AMOUNT, RECEIVER_AMOUNT</p><p>Default is SENDER_AMOUNT</p>                                                                                |


{% endtab %}

{% tab title="Request Sample" %}
**Transaction with funding\_source:`CARD` and payout\_method:`BANK_DEPOSIT`**

```
curl -X POST {{url}}/v2/senders/{{senderId}}/transactions \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret' \
  -H 'X-Idempotency-Key: idempotencykey' \
  -d { \
    "sender_amount": 0.11, \
    "exchange_rate": 0.1111, \
    "recipient_amount": 0.01, \
    "fee_amount": 0, \
    "bonus_amount": 0.00, \
    "recipient_currency": "string", \
    "sender_id": "string", \
    "sender_funding_account_id": "string", \
    "funding_source": "CARD", \
    "recipient_id": "string", \
    "recipient_account_id": "string", \
    "note": "string", \
    "remittance_purpose": "string", \
    "ip_address": "string", \
    "payout_method": "BANK_DEPOSIT" \
  }
```

**Transaction with funding\_source:`BANK` and payout\_method:`CASH_PICKUP`**

```
curl -X POST https://sandbox.api.machpay.com/v2/senders/{senderId}/transactions \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret' \
  -H 'X-Idempotency-Key: idempotencykey' \
  -d { \
    "sender_amount": 0.11, \
    "exchange_rate": 0.1111, \
    "recipient_amount": 0.01, \
    "fee_amount": 0, \
    "bonus_amount": 0.00, \
    "recipient_currency": "string", \
    "sender_id": "string", \
    "sender_funding_account_id": "string", \
    "funding_source": "BANK", \
    "recipient_id": "string", \
    "payer_id": 1, \
    "note": "string", \
    "remittance_purpose": "string", \
    "ip_address": "string", \
    "payout_method": "CASH_PICKUP" \
  }
```
{% endtab %}

{% tab title="Response Sample" %}
**Transaction with funding\_source:`CARD` and payout\_method:`BANK_DEPOSIT`**

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
  "funding_source": "CARD",
  "recipient_id": "string",
  "recipient_account_id": "string",
  "note": "string",
  "remittance_purpose": "string",
  "ip_address": "string",
  "status": "string",
  "delivery_status": "string",
  "reference_number": "string",
  "risk_score": 0,
  "payout_method": "BANK_DEPOSIT",
  "three_ds": {
        "enabled": true,
        "status": "string"
  },
}
```

**Transaction with funding\_source:`BANK` and payout\_method:`CASH_PICKUP`**

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
  "funding_source": "BANK",
  "recipient_id": "string",
  "payer_id": 1,
  "note": "string",
  "remittance_purpose": "string",
  "ip_address": "string",
  "status": "string",
  "delivery_status": "string",
  "reference_number": "string",
  "payout_reference_number" : "string",
  "risk_score": 0,
  "payout_method": "CASH_PICKUP"
}
```
{% endtab %}
{% endtabs %}

## Calculation Mode <a href="#calculation-mode" id="calculation-mode"></a>

There are two calculation modes available while creating a transaction. These modes are based on which amount (sending or receiving) the system will take as a base for the calculation.

1. SENDER\_AMOUNT: When a transaction is created with this calculation mode, you will pass the sending amount and exchange rate through the Transaction API. The receiving amount is calculated based on the provided sending amount and exchange rate. The default calculation mode is SENDER\_AMOUNT.
2.  RECEIVER\_AMOUNT: When a transaction is created with this calculation mode, you will pass the total receiving amount, sending amount, and exchange rate through the Transaction API. We will then calculate the sending amount based on the provided receiving amount and exchange rate.

    The calculated sending amount will then be compared with the sending amount you have provided in the request. If the difference between the calculated sending amount and the sending amount provided by you in the API request is greater than $0.01, the transaction API will provide the following error message.

    Copy

    ```
    {
      "test_field": "test_value",
      "status": 400,
      "message": "Sender amount is not equivalent with recipient amount."
    }
    ```

    For example, if the exchange rate today for USD to MEX, is 26.77 and the recipient\_amount is 946, the sender amount would be 943/26.77 = 35.338. Since our API will take the difference up to $0.01, $35.34, would be accepted for a successful transaction.

[<br>](https://raas.docs.machnetinc.com/api-references/transaction/get-transaction-limits)
