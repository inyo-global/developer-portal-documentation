# Quotes (fx)

#### FX Quote API Overview

The Foreign Exchange (FX) Quote API allows you to request and lock in foreign exchange rates for a specific source currency, destination currency, and amount. This ensures predictable conversion results and helps you avoid FX fluctuations during the transaction lifecycle.

***

### Request Payload Example

To request a quote, specify the source currency, destination currency, and the amount to be converted:

```json
{
  "fromCurrency": "USD",
  "toCurrency": "BRL",
  "amount": 1000
}
```

***

### Response Payload Example

The API responds with a quote object containing all relevant details:

```json
{
  "quotes": [
  {
    "id": "f28be3f9-ad4d-4a6c-a85d-ae5515a4a158",
    "agentId": "8500786a-68e9-45fd-b1d8-c590f1e06450",
    "fromAsset": "USD",
    "toAsset": "BRL",
    "effectiveRate": "5.39023000",
    "totalCost": {
      "amount": "5.23",
      "currency": "USD"
    },
   "product": "ACCOUNT",
   "expireAt": "2025-09-15T11:39:01Z",
   "createdAt": "2025-09-15T11:37:01Z",
   "sourceAmount": {
     "amount": "1000.00",
     "currency": "USD"
   },
   "destinationAmount": {
     "amount": "5390.23",
     "currency": "BRL"
    }
  }
 ]
}
```

#### Key Fields Explained

* id: Unique identifier of the quote.
* fromAsset / toAsset: The source and destination currencies.
* effectiveRate: The FX rate applied for this conversion.
* sourceAmount / destinationAmount: Exact converted amounts.
* totalCost: Associated fees/costs, expressed in source currency.
* product: Indicates whether the quote is tied to an ACCOUNT, WALLET, or CARD.
* expireAt: Timestamp when the quote becomes invalid.
* createdAt: When the quote was generated.

***

### Example Conversion Walkthrough

Let’s assume a request to convert 1,000 USD → BRL.

| Field                | Value                                  |
| -------------------- | -------------------------------------- |
| Source Currency      | USD                                    |
| Destination Currency | BRL                                    |
| Source Amount        | 1,000.00 USD                           |
| Effective Rate       | 5.39023000                             |
| Destination Amount   | 5,390.23 BRL                           |
| Total Cost           | 5.23 USD                               |
| Product              | BANK\_ACCOUNT                          |
| Quote Validity       | 2 minutes (until 2025-09-15T11:39:01Z) |

***

### Handling Quote Expiration

* Always check the expireAt timestamp.
* Refresh quotes before they expire (we recommend refreshing once \~90% of the validity period has elapsed).
* If the quote expires, request a new one before executing the transaction.



[Documentation](https://dev-api.inyoglobal.com/sandbox/#tag/payout_Quote-Resource/operation/RequestQuote)

Endpoint: /payout/quotes
