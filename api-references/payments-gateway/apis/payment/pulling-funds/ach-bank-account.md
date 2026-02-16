# ACH (Bank account)

#### ✅ Example – BANK\_ACCOUNT request

```json
{
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
        "lastName": "Trump",
        "address": {
            "countryCode": "US",
            "stateCode": "NY",
            "city": "New York",
            "line1": "123 Main Street",
            "line2": "Apt 25B",
            "zipcode": "10001"
        },
        "documents": [
            {
                "type": "",
                "document": ""
            }
        ],
        "payoutMethod": {
            "type": "BANK_DEPOSIT",
            "countryCodeAlpha3": "BRA",
            "bankCode": "001",
            "routingNumber": "1234",
            "accountNumber": "123456789",
            "accountType": "CHECKING"
        }
    },
    "additionalData": {}
}
```
