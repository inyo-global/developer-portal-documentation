---
description: >-
  Clients can enable webhooks for specific transaction status changes, when a
  transaction reaches one of the configured statuses.
---

# Webhooks

Here are the supported transaction events that can trigger a webhook:

* **AUTHORIZED**
* **CAPTURED**
* **CHALLENGE**
* **DECLINED**
* **VOIDED**
* **REFUNDED**

By enabling webhooks, clients can integrate seamlessly with their internal systems and automate responses to transaction updates.

## AUTHORIZED&#x20;

This event is triggered when a transaction has been successfully authorized by the payment provider, but not yet captured.

Use this status to:

* Notify the client that the payment method is valid and funds are reserved.

## CAPTURED

Indicates that the transaction has been successfully captured, and funds have been transferred or committed.

Common actions:

* Confirm the order.
* Release goods or services to the customer.

## CHALLENGE

Triggered when the transaction enters the **3D Secure (3DS)** flow and requires additional user verification. This status indicates that the cardholder must complete a security challenge—such as entering a one-time password (OTP), biometric confirmation, or app-based authentication—before the transaction can proceed to authorization.

Clients should:

* Wait for a follow-up status (e.g., `AUTHORIZED` or `DECLINED`) after challenge completion.

## DECLINED

The transaction was rejected by the issuer or acquiring bank.\
Common reasons:

* Insufficient funds.
* Card expired.
* Fraud suspicion.

Clients can:

* Prompt the user to try another payment method.
* Log the reason for analytics or customer support.

## VOIDED

A previously authorized transaction has been voided before capture.\
Typical use cases:

* Order was canceled before fulfillment.
* Authorization was no longer needed or expired.

## REFUNDED

Funds for a previously captured transaction were successfully refunded to the customer.\
Clients should:

* Update refund status in their system.
* Notify the customer of the successful return.

## Webhook Body Format

For all webhook events, the client will receive a payload in the following JSON format:

```json
{
    "paymentId": "469670e2-31e3-4700-b9f2-abee99418dc4",
    "parentPaymentId": "469670e2-31e3-4700-b9f2-abee99418dc4",
    "externalPaymentId": "1231223221399923139",
    "redirectAcsUrl": "",
    "amount": 95,
    "created": "2025-01-21 16:31:02",
    "approved": true,
    "message": "Payment Approved",
    "acquirerMessage": "IN_PROCESS_AUTHORISED",
    "automaticReversed": false,
    "status": "AUTHORIZED",
    "captured": false,
    "voided": false,
    "authCode": "469670e2-31e3-4700-b9f2-abee99418dc4",
    "issuerName": "UNKNOWN",
    "issuerCountry": "N/A",
    "cvcResult": "NOT_SENT",
    "avsResult": "NOT_SENT",
    "aavAddressResult": "NOT_CHECKED",
    "aavPostcodeResult": "NOT_CHECKED",
    "aavCardholderNameResult": "N/A",
    "aavTelephoneResult": "NOT_CHECKED"
}
```
