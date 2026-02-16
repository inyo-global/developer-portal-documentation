# Authorizing

Upon successful receipt of a payment token, the transaction can now be authorized. This endpoint allows for two types of authorization: pre-authorization and direct authorization. Pre-authorization confirms the availability of sufficient funds but does not secure them right away, providing flexibility to adjust the transaction amount later if needed. Direct authorization, on the other hand, secures the funds immediately, ensuring that the amount is held for the specific transaction.

In order to authorize the transaction, it's necessary to form three different components:

1. The token received in the previous step (one time or recurring)
   1. Note that if you are using a previous stored token, you will also need a previous paymentId (returned from the last authorization or null if it's the first transaction to be placed with the recurring token);
2. Payload of the transaction to be charged;
3. Access token generated from the oauth flow;

**Generating an access token:**

```bash
curl -X POST https://sandbox-gw.simpleps.com/oauth/token \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
    "clientId": "2131-21312-3312",
    "secretId": "12312-1231-123-112"
  }'
```

#### ✅ Example - CARD request&#x20;

```bash
curl -X POST 
-H 'Authorization: Bearer <token>' 
-H 'Accept: application/json' \
-H 'Content-Type: application/json' \
-d '{
    "externalPaymentId": "21121231312312",
    "ipAddress": "1.2.3.4",
    "paymentType": "PULL",
    "capture": true,
    "amount": {
        "total": 19.99,
        "currency": "USD"
    },
    "sender": {
        "firstName": "John",
        "lastName": "Johny",
        "address": {
            "countryCode": "US",
            "stateCode": "NY",
            "city": "New York",
            "line1": "123 Main Street",
            "line2": "Apt 25B",
            "zipCode": "10001"
        },
        "documents": [
            {
                "type": "",
                "document": ""
            }
        ],
        "paymentMethod": {
            "type": "CARD",
            "cardTokenId": "ab5fc589-8b48-4531-94c0-68b0629c13fe",
            "previousPaymentId": "5b9407aa-2f28-47f9-811f-537408574d4d"
        }
    },
    "additionalData": {}
}'
https://sandbox-gw.simpleps.com/v2/payment

```

**CARD response:**

```json
{
  "status": 200,
  "data": {
    "paymentId": "dce568c6-98ec-456c-bb33-4a6809c4fff8",
    "parentPaymentId": "dce568c6-98ec-456c-bb33-4a6809c4fff8",
    "externalPaymentId": "c6bfd9ac-906f-4218-b9ec-aabf0f83fd0c",
    "redirectAcsUrl": "https://sandbox-gw.simpleps.com/secure-code/start-challenge?token=dce568c6-98ec-456c-bb33-4a6809c4fff8",
    "amount": 10.25,
    "created": "2025-03-31 03:49:05",
    "approved": false,
    "message": "Payment awaiting 3DS challenge verification",
    "automaticReversed": false,
    "status": "CHALLENGE",
    "captured": false,
    "voided": false,
    "responseCode": "00",
    "issuerName": "BANK ISSUER",
    "issuerCountry": "ITALY",
    "cvcResult": "N/A",
    "avsResult": "N/A",
    "avsCardholderNameResult": "N/A",
    "avsTelephoneResult": "N/A"
  }
}
```



| Field Name          | Description                                                                                                                                                                                                                                                | Example Value                                                                                            |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| `status`            | <p>HTTP response status code<br>200 if success<br>400 if validation error<br>500 if error</p>                                                                                                                                                              | 200                                                                                                      |
| `paymentId`         | Unique identifier for the payment                                                                                                                                                                                                                          | `dce568c6-98ec-456c-bb33-4a6809c4fff8`                                                                   |
| `parentPaymentId`   | Identifier linked to a parent payment if applicable                                                                                                                                                                                                        | `dce568c6-98ec-456c-bb33-4a6809c4fff8`                                                                   |
| `externalPaymentId` | Identifier for the external payment source sent                                                                                                                                                                                                            | `c6bfd9ac-906f-4218-b9ec-aabf0f83fd0c`                                                                   |
| `redirectAcsUrl`    | URL for 3D Secure challenge verification, if status = 'CHALLENGE'                                                                                                                                                                                          | `https://sandbox-gw.simpleps.com/secure-code/start-challenge?token=dce568c6-98ec-456c-bb33-4a6809c4fff8` |
| `amount`            | Total amount of the payment                                                                                                                                                                                                                                | 10.25                                                                                                    |
| `created`           | Timestamp of when the payment was created (EST)                                                                                                                                                                                                            | `2025-03-31 03:49:05`                                                                                    |
| `approved`          | Status indicating if the payment is approved. true or false                                                                                                                                                                                                | `false`                                                                                                  |
| `message`           | Status message for the payment process                                                                                                                                                                                                                     | `Payment awaiting 3DS challenge verification`                                                            |
| `automaticReversed` | Indicates if payment was automatically reversed                                                                                                                                                                                                            | `false`                                                                                                  |
| `status`            | <p>Current status of the payment.<br><br>CHALLENGE - if the transaction requires 3ds step up.<br><br>AUTHORIZED - if the transaction was automatically authorized.<br><br>DECLINED - if the transaction was declined by the network or internal rules.</p> | `CHALLENGE`                                                                                              |
| `captured`          | <p>Indicates if the payment was captured.<br><br>true - for automatic capture<br>false - if pre-auth was requested</p>                                                                                                                                     | `false`                                                                                                  |
| `voided`            | Indicates if the payment was voided                                                                                                                                                                                                                        | `false`                                                                                                  |
| `responseCode`      | Response code from the issuer                                                                                                                                                                                                                              | `00`                                                                                                     |
| `issuerName`        | Name of the issuing bank                                                                                                                                                                                                                                   | `BANK SA`                                                                                                |
| `issuerCountry`     | Country of the issuer                                                                                                                                                                                                                                      | `UNITED STATES`                                                                                          |
| `cvcResult`         | <p>Result of CVC verification<br><br>APPROVED <br>FAILED (verification failed)<br>NOT_SENT (acquirer or issuer doesn't support)<br>N/A (if challenge)</p>                                                                                                  | `N/A`                                                                                                    |
| `avsResult`         | <p>Result of AVS verification<br><br>APPROVED <br>FAILED (verification failed)<br>NOT_SENT (acquirer or issuer doesn't support)<br>N/A (if challenge)</p>                                                                                                  | `N/A`                                                                                                    |

####
