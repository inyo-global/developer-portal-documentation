# Authorizing a Transaction

Upon successful receipt of a payment token, the transaction can now be authorized. This endpoint allows for two types of authorization: pre-authorization and direct authorization. Pre-authorization confirms the availability of sufficient funds but does not secure them right away, providing flexibility to adjust the transaction amount later if needed. Direct authorization, on the other hand, secures the funds immediately, ensuring that the amount is held for the specific transaction.

The full description of the method, can be found in the [openapi repository](https://sandbox-gw.simpleps.com/q/swagger-ui/#/Payment%20Resource/post_payments).

In order to authorize the transaction, it's necessary to form three different components:

1. The token received in the previous step (one time or recurring)
   1. Note that if you are using a previous stored token, you will also need a previous paymentId (returned from the last authorization or null if it's the first transaction to be placed with the recurring token);
2. Payload of the transaction to be charged;
3. Access token generate from the oauth flow;

| Key               | Value                                                             | Required                                                                                                            | Default value |
| ----------------- | ----------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- | ------------- |
| capture           | true or false                                                     | yes                                                                                                                 |               |
| externalPaymentId | string                                                            | <p>yes<br><br><em>idenpotemcy key</em></p>                                                                          |               |
| installmentNumber | \[0-9]{2}                                                         | no                                                                                                                  | 1             |
| amount            | object                                                            | yes                                                                                                                 |               |
| total             | decimal                                                           | yes                                                                                                                 |               |
| currency          | string (ISO 4247 format)                                          | yes                                                                                                                 | USD           |
| ipAddress         | string (ipv4 or ipv6 format)                                      | yes                                                                                                                 |               |
| paymentType       | <p>enum {<br>REGULAR,<br>INSTALLMENT,<br>RECURRING<br>}<br></p>   | no                                                                                                                  | REGULAR       |
| items             | object                                                            | no                                                                                                                  |               |
| name              | string                                                            | \*yes if any item is filled in.                                                                                     |               |
| unitAmount        | decimal                                                           | \*yes if any item is filled in.                                                                                     |               |
| quantity          | integer                                                           | \*yes if any item is filled in.                                                                                     |               |
| customer          | object                                                            | yes                                                                                                                 |               |
| firstName         | string                                                            | yes                                                                                                                 |               |
| lastName          | string                                                            | yes                                                                                                                 |               |
| phoneNumber       | string                                                            | yes                                                                                                                 |               |
| email             | string                                                            | no                                                                                                                  |               |
| gender            | string                                                            | no                                                                                                                  |               |
| birthDate         | string                                                            | no                                                                                                                  |               |
| externalId        | string                                                            | no _(idenpotemcy key)_                                                                                              |               |
| document          | string                                                            | no                                                                                                                  |               |
| documentType      | string                                                            | no                                                                                                                  |               |
| shippingAddress   |                                                                   | <p><strong>no</strong> if digital service.<br><br><strong>yes</strong> if goods</p>                                 | accept null   |
| countryCode       | string (ISO 3166 format)                                          |                                                                                                                     | US            |
| stateCode         | string (ISO 3166-2 format)                                        |                                                                                                                     |               |
| city              | string                                                            |                                                                                                                     |               |
| line1             | string                                                            |                                                                                                                     |               |
| line2             | string                                                            |                                                                                                                     |               |
| state             | string                                                            |                                                                                                                     |               |
| zipCode           | string                                                            |                                                                                                                     |               |
| billingAddress    |                                                                   | yes                                                                                                                 |               |
| countryCode       | string (ISO 3166 format)                                          | yes                                                                                                                 |               |
| stateCode         | string (ISO 3166-2 format)                                        | yes                                                                                                                 |               |
| city              | string                                                            | yes                                                                                                                 |               |
| line1             | string                                                            | yes                                                                                                                 |               |
| line2             | string                                                            | no                                                                                                                  |               |
| state             | string                                                            | yes                                                                                                                 |               |
| zipCode           | string                                                            | yes                                                                                                                 |               |
| source            | object                                                            | yes                                                                                                                 |               |
| type              | <p>enum {<br>CARD,<br>BANK_ACCOUNT<br>}<br></p>                   | yes                                                                                                                 |               |
| card              | object                                                            | \*yes (card or bankAccount objects must be present)                                                                 |               |
| token             | string                                                            | \*yes                                                                                                               |               |
| previousPaymentId | string                                                            | <p>*no<br>if it's a transaction originated from a previous stored token, a previous payment id is required.<br></p> |               |
| bankAccount       | object                                                            | \*yes                                                                                                               |               |
| accountType       | <p>enum {<br>SAVINGS,<br>CHECKING,<br>CASH,<br>OTHER<br>}<br></p> | \*yes                                                                                                               |               |
| accountNumber     | string                                                            | \*yes                                                                                                               |               |
| bankCode          | string                                                            | \*yes                                                                                                               |               |
| accountHolder     | object                                                            | \*yes                                                                                                               |               |
| type              | string                                                            | \*yes                                                                                                               |               |
| firstName         | string                                                            | \*yes                                                                                                               |               |
| lastName          | string                                                            | \*yes                                                                                                               |               |
| companyLegalName  | string                                                            | \*no                                                                                                                |               |

**Sample authorization code of a transaction with auto-capture, using an one-time-only token:**

```bash
#generate the token
curl -H 'Accept: application/json'
	   -H 'Content-Type: application/json'
	   -d 'clientId=2131-21312-3312&secretID=12312-1231-123-112' 
	   https://sandbox-gw.simpleps.com/oauth/token

#make the authorization request, using an existing tokenized card
curl -H 'Content-Type: application/json'
		 -H 'Authorization: Bearer token12345667'
		 -d '{
            "capture": true,
            "externalPaymentId": "1234567",
            "amount": {
                "total": 100.10,
                "currency": "USD"
            },
            "ipAddress": "127.0.0.1", 
            "customer": {
                "firstName": "JOHN",
                "lastName": "DOE",
                "phoneNumber": "+12345678900",
                "email": "johndoe@gmail.com",
                "gender": "MALE",
                "birthDate": "1980-01-01",
                "externalId": "test123"
            },
            "billingAddress": {
                "countryCode": "US",
                "stateCode": "MA",
                "city": "BOSTON",
                "line1": "123 MAIN AVE",
                "state": "MASSACHUSSETTS",
                "zipCode": "12345"
            },
            "source": {
                "type": "CARD",
                "card": {
                    "token": "1234-456-3233-2323"                    
                }
            }
        }'
       	https://sandbox-gw.simpleps.com/payments
       
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

In the sample provided above, note that:

* the transaction was approved by the issuer bank, but important checks failed
* capture was set to true, which means the only way to reverse is to execute a refund
* It's important to design the proper flows during the authorization to mitigate fraud. In this case, cvv (the 3 digit code failed) and the AVS (to validate the cardholder address) wasn't checked
* In a real world scenario, you would possibly be cautious in this transaction, or configure gateway rules to decline it automatically.

#### Response Fields

*   **paymentId**

    Unique identifier for each payment request generated by the system.
*   **parentPaymentId**

    Groups all payment IDs associated with the same external payment ID under a single parent ID.
*   **externalPaymentId**

    A unique identifier used by the client for the payment.
*   **amount**

    The total amount of the payment.
*   **created**

    The date and time when the payment was created.
*   **approved**

    Indicates whether the payment was approved (`true`) or rejected (`false`).
*   **message**

    The message returned by the gateway regarding the payment.
*   **acquirerMessage**

    The message returned by the acquirer regarding the payment.
*   **automaticReversed**

    Indicates if the payment was automatically reversed. If the acquirer approves the transaction but a rule rejects it, a refund transaction will be sent to the acquirer, and the payment will be marked as rejected.
*   **status**

    The status of the payment, represented as an enum. Possible values include:

    * `CHALLENGE`
    * `AUTHORIZED`
    * `VOIDED`
    * `CAPTURED`
    * `PARTIALLY_CAPTURED`
    * `PARTIALLY_REFUNDED`
    * `REFUNDED`
    * `DECLINED`
*   **captured**

    Indicates whether the payment was captured (`true`) or not (`false`).
*   **voided**

    Indicates whether the payment was voided (`true`) or not (`false`).
*   **authCode**

    The authorization code provided by the issuer if the payment was approved.
*   **issuerName**

    The name of the issuer.
*   **issuerCountry**

    The country of the issuer.
