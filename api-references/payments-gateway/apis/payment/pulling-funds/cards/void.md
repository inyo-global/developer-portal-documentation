# Void

Voiding an authorized transaction cancels the payment before settlement occurs. This ensures that no funds are transferred from the customer to the merchant.

### Key Considerations

* A **void** is only possible **if the transaction has not yet been captured**.
* Voiding a transaction cancels it immediately and **prevents funds from being settled**.
* If the transaction was already captured, you must issue a **refund** instead.
* The `externalPaymentId` acts as an **idempotency key**, provided during the original authorization.
* Voids are **irreversible**.

### Generate an Access Token

Use the OAuth endpoint to obtain an access token (Bearer) required for authorization.

```
curl -X POST https://sandbox-gw.simpleps.com/oauth/token \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
    "clientId": "2131-21312-3312",
    "secretID": "12312-1231-123-112"
  }'

```

## Void an Authorized Transaction

```
POST https://sandbox-gw.simpleps.com/payments/{externalPaymentId}/void
```

Replace `{externalPaymentId}` with the ID used during authorization.

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
  "captured": false,
  "voided": true,
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
