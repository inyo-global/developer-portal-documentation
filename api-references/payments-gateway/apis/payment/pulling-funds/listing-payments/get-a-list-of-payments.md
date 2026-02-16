---
description: >-
  The Inyo Gateway allows clients to search for transactions in a paginated
  manner.
---

# Get a list of payments

This feature is useful for retrieving large sets of transaction data in smaller, manageable chunks, improving performance and usability.

Endpoint: `https://sandbox-gw.simpleps.com/payments?resultsPerPage=10&page=0`

```json
[
{
    "parentPaymentId": "9a8dd6f3-105c-40a9-be13-ed95b2b2274b",
    "externalId": "00620c28-8833-4bc8-b893-03b456ed0122",
    "requestedOn": "2025-01-21 17:15:14",
    "capturedOn": "2025-01-18 14:59:53",
    "refundedOn": "2025-01-21 17:15:14",
    "amount": 57.0000,
    "capturedAmount": 57.0000,
    "refundedAmount": 57.0000,
    "amountRequested": 57.0000,
    "currency": "USD",
    "status": "REFUNDED",
    "approved": true,
    "billing": {
        "stateCode": "FL",
        "city": "Orlando",
        "line1": "12516 Britwell Ct",
        "line2": "298 Main Street Clinton  mass01510",
        "state": "FL",
        "zipCode": "32837"
    },
    "ipAddress": "35.145.20.96",
    "customer": {
        "firstName": "MIKE",
        "lastName": "JOSEPH",
        "phoneNumber": "+1231232123",
        "email": "mike.joseph@hotmail.com"
    },
    "transactionFees": [
        {
            "sourceId": "9a8dd6f3-105c-40a9-be13-ed95b2b2274b",
            "source": "PAYMENT",
            "eventId": "PRE_AUTH",
            "dtCreated": "2025-01-18 14:59:48",
            "amount": 0.5700
        },
        {
            "sourceId": "0e444b74-db6a-4585-aa2f-4a9ea73aa599",
            "source": "PAYMENT",
            "eventId": "CAPTURE",
            "dtCreated": "2025-01-18 14:59:53",
            "amount": 0.0000
        },
        {
            "sourceId": "9989afba-ca01-4308-8e6a-91cf3de21909",
            "source": "PAYMENT",
            "eventId": "REFUND",
            "dtCreated": "2025-01-21 17:15:14",
            "amount": 0.0000
        }
    ],
    "history": [
        {
            "paymentId": "9a8dd6f3-105c-40a9-be13-ed95b2b2274b",
            "requestedOn": "2025-01-18 14:59:47",
            "code": "PAYMENT",
            "status": "AUTHORIZED",
            "description": "Payment Approved",
            "requestedAmount": 57
        },
        {
            "paymentId": "0e444b74-db6a-4585-aa2f-4a9ea73aa599",
            "requestedOn": "2025-01-18 14:59:53",
            "code": "CAPTURE",
            "status": "CAPTURED",
            "description": "Payment Approved",
            "requestedAmount": 57
        },
        {
            "paymentId": "9989afba-ca01-4308-8e6a-91cf3de21909",
            "requestedOn": "2025-01-21 17:15:14",
            "code": "REFUND",
            "status": "REFUNDED",
            "description": "Payment Approved",
            "requestedAmount": 57.0000
        }
    ],
    "card": {
        "lastFourDigits": "2929",
        "bin": "479213",
        "schemeId": "VISA",
        "issuer": "TD BANK, NATIONAL ASSOCIATION",
        "country": "UNITED STATES",
        "countryCode": "US",
        "binNumber": "479213",
        "cardType": "DEBIT",
        "cardCategory": "CLASSIC",
        "currencyCode": "USD",
        "countryCode3": "USA"
    }
}
]
```
