# Refund

Refunding a transaction means reversing a previously captured payment, partially or totally. This action returns funds to the customer and the amount of time varies by issuing bank.

### Key Considerations

* Refunds are only possible for **captured** transactions.
* You can refund:
  * The **full amount**, or
  * A **partial amount**, as long as it's **less than or equal** to the captured value.
* Multiple partial refunds are allowed **until the full amount is refunded**.
* Once the full amount is refunded, further refunds will return an error.
* The `externalPaymentId` used in capture also identifies the transaction to refund.

### Generate an Access Token

Use the same OAuth token generation process as for capture and void.

```
curl -X POST https://sandbox-gw.simpleps.com/oauth/token \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
    "clientId": "2131-21312-3312",
    "secretID": "12312-1231-123-112"
  }'
```

## Refund a Captured Transaction

```
POST https://sandbox-gw.simpleps.com/payments/{externalPaymentId}/refund
```

#### Request Body

> Optional. If not informed, the full captured amount will be refunded.
>
> ```
> {
>   "amount": {
>     "total": 50.00,
>     "currency": "USD"
>   }
> }
> ```

```
curl -X POST https://sandbox-gw.simpleps.com/payments/1234567/refund \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer token12345667' \
  -d '{
    "amount": {
      "total": 50.00,
      "currency": "USD"
    }
  }'

```

## Sample Success Response

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
	"status": "REFUNDED",
	"captured": false,
	"authCode": "bfb9eacb-7c72-4cc8-9cae-afd9164ec792",
	"issuerName":"NAME OF THE ISSUER BANK",
	"issuerCountry":"US",
	"cvcResult":"FAILED",
	"avsResult":"NOT_SENT",
	"aavAddressResult":"NOT_CHECKED",
	"aavPostcodeResult":"NOT_CHECKED",
	"aavCardholderNameResult":"N/A",
	"aavTelephoneResult":"NOT_CHECKED"
}  
```
