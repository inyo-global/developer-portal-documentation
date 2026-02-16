# Capture an authorization

The process of capturing a transaction means that it's ready for financial settlement. Usually, authorizations that are not captured automatically are placed to check funds and other cardholder information, as well as internal processes, before entering into the settlement.

**Important notes**:

* If a transaction, for example, is not captured within 7 days of the authorization, Inyo will automatically cancel.
* Settlement times start from the moment transaction is captured.

Capturing a transaction authorization is possible only if the transaction has not yet been captured. If a transaction was created with the flag “capture: true", or was previously captured, the system will return an error message. It's possible to capture a smaller that is equal or less the authorized transaction, and only once.

It no amount is informed during the capture process, the original amount set in the authorization will be used.

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
		 -d '{
						  "amount": {
						    "total": 100.10,
						    "currency": "USD"
						  }
						}'
      https://sandbox-gw.simpleps.com/payments/1234567/capture
       
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
	"captured": true,
	"voided": false,
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
