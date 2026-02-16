---
description: Details on 3DS verification to mitigate risks
---

# 3DS Verification

3DS is a messaging protocol to authenticate card information when processing card-not-present payments. This authentication helps prevent fraud and provides additional layers of security. It is provided by all four major card networts. The issuer authenticates the card hold information using risk-based authentication which can result in a frictionless flow or a challenge flow for the user.

* A frictionless flow is triggered if the data is satisfactory for the issuer to confirm that the actual cardholder is creating the transaciton. Authentication is completed without the need for additional inputs from the cardholder. The reasons for success or failure of frictionless 3DS verification is dependent on the issuer.
* If further validation is required by the issuer, the user will be prompted through the challenge flow which will require the user to provide additional information (e.g. OTP, out-of-wallet question, etc.). This information will allow the issuer to make a better decision on the required 3DS verification.

In order to use 3DS verification, it needs to be enabled for you by Machnet. Once 3DS verification is enabled, only successfully verified transactions (either through frictionless or challenge flow) will be forwarded for processing. You will receive the following additional fields in the response of the Create and Get Transaction API, which will provide you details on the 3DS verification status and thus, the next steps.

| **Field Name**    | **Type** | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ----------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| three\_ds.enabled | boolean  | True if 3DS is enabled for a client and funding source is CARD , else false.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| three\_ds.status  | enum     | <p>UNVERIFIED: 3DS operation has not been initiated. Client needs to initialize the Widget with type = 3ds.</p><p>IN_PROGRESS: Client has opened the 3DS widget and 3DS is in progress. If the user is routed through the challenge flow, they will remain in this status unless they answer the challenge prompt. It is advised to continue to keep the widget open in such cases.</p><p>HOLD: 3DS verification is on HOLD due to some error. Please contact Machnet customer support.</p><p>VERIFIED: 3DS verification has been successfully completed through challenge or frictionless flow.</p><p>FAILED: 3DS verification has failed.Transaction is cancelled.</p> |

**Response Sample**

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

**Response Sample**

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

## **Transaction Status**

The 3DS status of the transaction also impacts the transaction’s status. The following is a mapping of the 3DS Status and the Transaction Status.

| **3DS Status** | **Transaction Status**                                |
| -------------- | ----------------------------------------------------- |
| UNVERIFIED     | INITIATED                                             |
| IN\_PROGRESS   | INITIATED                                             |
| HOLD           | HOLD                                                  |
| FAILED         | CANCELED                                              |
| VERIFIED       | Transaction will be forwarded for further processing. |

## 3DS Integration <a href="#id-3ds-integration" id="id-3ds-integration"></a>

For 3DS verification process to be initiated, you will need to open the Machnet Widget with type: 3ds, senderId, token and transactionId. If the user is directed through frictionless flow, there will be no user input required, however, if the user is routed through the challenge flow, the challenge question will be prompted to the user on the same widget and the user will have to complete it. Only once 3DS is verified successfully through the frictionless or the challenge flow, will the transaction be forwarded for further processing. If the verification fails, the transaction is canceled.

### **Initialize 3DS widget**

Before you initialize the 3DS widget, please make sure you complete the other steps of setting up [Machnet Widget v1.0](../../widget/widget-v1.0/).

```
<script>
  var widget = new MachnetWidget({
    elementId     : "widget-root",
    senderId      : "sender UUID",
    width         : "100%",
    height        : "2000px",
    type          : "3ds",
    locale        : "en",
    transactionId : "transaction UUID",
    token         : "very.long.token"
 });
 widget.init();
</script>
```

## Javascript event <a href="#javascript-event" id="javascript-event"></a>

The following events related to 3DS verification can be listened to for further information on the activity of the user on the 3DS widget. The widget will trigger an event anytime there are changes in a resource. To listen to the events generated by the widget, please refer to [this ](../../widget/widget-v1.0/)guide.

| **Event Name**          | **Description**                                                                                                                                                                                                                                                                                                                                   |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| THREE\_DS\_VERIFIED     | 3DS verification has been successfully completed through challenge or frictionless flow. The transaction will be forwarded for processing.                                                                                                                                                                                                        |
| THREE\_DS\_HOLD         | <p>3DS verification is on hold due to an error in the 3DS verification process. Machnet’s admin needs to review the transaction. Please contact Machnet customer support.</p><p>If an admin chooses to unhold such transactions, 3DS verification will need to be conducted again. Please review the webhooks section for additional details.</p> |
| THREE\_DS\_FAILED       | 3DS verification failed as the issuer was not able to verify the transaction. Transaction will be canceled.                                                                                                                                                                                                                                       |
| THREE\_DS\_IN\_PROGRESS | 3DS verification was initiated and is currently in progress.                                                                                                                                                                                                                                                                                      |

## Webhooks <a href="#webhooks" id="webhooks"></a>

There is an important webhook related to 3DS verification, called 3ds\_verification\_required, that also requires your attention. This webhook is sent when the Machnet admin chooses to unhold a transaction that was on HOLD due to error in the 3DS process. When such transactions are removed from hold, 3DS status will be moved from HOLD to UNVERIFIED. 3DS needs to be rerun in which case you will have to inform the user and open the widget with type: 3ds so they can complete 3DS verification.
