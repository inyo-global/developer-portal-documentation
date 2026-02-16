# Refund a transaction

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
		 -d '{"amount": {"total": 80}}'
		 -X POST
      https://sandbox-gw.simpleps.com/payments/1234567/refund
       
 #sample json response with the refund result:
 {
	"paymentId": "1fc787d4-d612-41df-97c8-86077e171e16",
	"parentPaymentId": "f3d84a2d-da30-4b62-a58b-6f7683405077",
	"externalPaymentId": "52c068fa-c427-4a15-b0aa-fbf5085a6634",
	"amount": 80.00,
	"created": "2024-04-22 19:04:46",
	"approved": true,
	"message": "Payment Approved",
	"acquirerMessage":"N/A",
	"automaticReversed": false,
	"status":"PARTIALLY_REFUNDED",
	"captured":true,
	"voided":false,
	"authCode": "f3d84a2d-da30-4b62-a58b-6f7683405077",
	"issuerName": "N/A",
	"issuerCountry": "N/A",
	"cvcResult": "N/A",
	"avsResult": "N/A",
	"aavAddressResult": "N/A",
	"aavPostcodeResult": "N/A",
	"aavCardholderNameResult": "N/A",
	"aavTelephoneResult":"N/A"
}  

```

[Refund a transaction](https://www.notion.so/Refund-a-transaction-4d62a2445f134555a3cbaf90c6823c87?pvs=21)
