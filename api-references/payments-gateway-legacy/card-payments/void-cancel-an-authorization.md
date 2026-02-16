# Void - cancel an authorization

Voiding a transaction authorization is possible only if the transaction has not yet been captured, i.e., the settlement process has not yet begun. This allows merchants to cancel the transaction without transferring any funds. If the transaction has been captured, it cannot be voided, and the merchant must process a refund instead.

```bash
#generate the token
curl -H 'Accept: application/json'
	   -H 'Content-Type: application/json'
	   -d 'clientId=2131-21312-3312&secretID=12312-1231-123-112' 
	   https://sandbox-gw.simpleps.com/oauth/token

#capture an authorization placed with the external id 123456
#the external id is an idempotency key provided by the merchant
curl -H 'Content-Type: application/json'
		 -H 'Authorization: Bearer token12345667'
		 -X POST
      https://sandbox-gw.simpleps.com/payments/1234567/void
       
 #sample json response with the authorization result:
 {
	"paymentId": "bfb9eacb-7c72-4cc8-9cae-afd9164ec792",
	"parentPaymentId": "bfb9eacb-7c72-4cc8-9cae-afd9164ec792",
	"externalPaymentId": "1234567",
	"amount": 100.10,
	"created": "2024-04-22 16:22:06",
	"approved": true,
	"message": "Payment Approved",
	"acquirerMessage": "IN_PROCESS_AUTHORISED",
	"automaticReversed": false,
	"status": "CAPTURED",
	"captured": false,
	"voided": true,
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
