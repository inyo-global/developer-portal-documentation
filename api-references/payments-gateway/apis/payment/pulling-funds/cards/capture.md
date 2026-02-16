---
description: >-
  Capturing a transaction means confirming that it's ready for financial
  settlement. This process transfers the authorized amount from the customer's
  account to the merchant.
---

# Capture

### Key Considerations

* If a transaction is **not captured within 7 days**, the system (`Inyo`) will **automatically cancel** it.
* Settlement time begins from the **moment of capture**, not authorization.
* **A transaction can be captured only once.**
* If the transaction was created with the flag `"capture": true` or has already been captured, an attempt to capture again will return an error.
* It's possible to **capture a smaller amount**, as long as it is **equal to or less than** the authorized value.
* If no amount is informed, the system will capture the **original authorized amount**.

### Generate an Access Token

Use this endpoint to obtain a Bearer token for authorization.

```
curl -X POST https://sandbox-gw.simpleps.com/oauth/token \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
    "clientId": "2131-21312-3312",
    "secretID": "12312-1231-123-112"
  }'

```

### Capture an Authorized Transaction

Captures a transaction that has already been authorized but not yet settled (in a pre-authorized state, where capture was request as false).

```
POST https://sandbox-gw.simpleps.com/payments/{externalPaymentId}/capture
```

### Sample Success Response

```
{
  "paymentId": "bfb9eacb-7c72-4cc8-9cae-afd9164ec792",
  "parentPaymentId": "bfb9eacb-7c72-4cc8-9cae-afd9164ec792",
  "externalPaymentId": "1234567",
  "amount": 100.10,
  "created": "2024-04-22 16:22:06",
  "approved": true,
  "message": "Payment Approved",
  "automaticReversed": false,
  "status": "CAPTURED",
  "captured": true,
  "voided": false,
  "authCode": "bfb9eacb-7c72-4cc8-9cae-afd9164ec792",
  "issuerName": "NAME OF THE ISSUER BANK",
  "issuerCountry": "US",
  "cvcResult": "FAILED",
  "avsResult": "NOT_SENT",
  "aavAddressResult": "NOT_CHECKED",
  "aavPostcodeResult": "NOT_CHECKED",
  "aavCardholderNameResult": "N/A",
  "aavTelephoneResult": "NOT_CHECKED"
} 
```
