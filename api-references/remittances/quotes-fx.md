# Quotes (FX)

The FX Quote API allows you to request and lock in foreign exchange rates for a specific currency corridor and amount. The returned `quoteId` guarantees the rate for a fixed window, protecting both you and your customer from FX fluctuations during the transaction flow.

***

### How Pricing Works

Two components determine the cost to the end user:

* **FX Spread** — Your margin on the exchange rate, configured per corridor by Inyo during onboarding.
* **Transaction Fee** — A per-transaction fee, also configured during onboarding. Depending on your contract, you may be able to override fees at the quote level (e.g., for promotional "zero fee" offers).

#### Amount Types

When requesting a quote, the `amountType` field controls how fees are applied:

| Type | Behavior | Example (fee = $4, amount = $100) |
| ---- | -------- | --------------------------------- |
| `NET` | Fee is added on top of the amount. The full amount is converted. | Sender pays **$104**. $100 is converted to destination currency. |
| `GROSS` | Fee is deducted from the amount. Only the remainder is converted. | Sender pays **$100**. $96 is converted to destination currency. |

***

### Requesting a Quote

**Endpoint:** `POST /organizations/{tenant}/payout/quotes`\
**Authentication:** Agent-level (`x-api-key` + `x-agent-id` + `x-agent-api-key`)

#### Request Body

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `fromCurrency` | string | Yes | Source currency (ISO 4217), e.g., `USD` |
| `toCurrency` | string | Yes | Destination currency (ISO 4217), e.g., `BRL` |
| `amount` | number | Yes | Amount to convert |
| `fee` | object | No | Override the default fee (if allowed by your contract) |
| `fee.amount` | number | — | Fee amount |
| `fee.currency` | string | — | Fee currency (typically matches `fromCurrency`) |
| `amountType` | string | No | `GROSS` or `NET` (see above) |

#### Sample Request

```bash
curl --request POST \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/payout/quotes \
  --header 'Content-Type: application/json' \
  --header "x-api-key: $API_KEY" \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY" \
  --data '{
  "fromCurrency": "USD",
  "toCurrency": "BRL",
  "amount": 1000,
  "fee": {
    "amount": 4.99,
    "currency": "USD"
  },
  "amountType": "NET"
}'
```

#### Sample Response

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

***

### Response Fields

| Field | Description |
| ----- | ----------- |
| `id` | Unique quote identifier. **Save this** — it's required for the transaction. |
| `fromAsset` / `toAsset` | Source and destination currencies |
| `effectiveRate` | The FX rate applied for this conversion (includes your spread) |
| `sourceAmount` | The amount in the source currency |
| `destinationAmount` | The amount the recipient will receive |
| `totalCost` | Total fees/costs in the source currency |
| `product` | Quote context: `ACCOUNT` (bank deposit), `WALLET`, or `CARD` |
| `expireAt` | ISO 8601 timestamp when the quote becomes invalid |
| `createdAt` | ISO 8601 timestamp when the quote was generated |

***

### Example Conversion Walkthrough

**Scenario:** Convert 1,000 USD → BRL

| Field | Value |
| ----- | ----- |
| Source Currency | USD |
| Destination Currency | BRL |
| Source Amount | 1,000.00 USD |
| Effective Rate | 5.39023000 |
| Destination Amount | 5,390.23 BRL |
| Total Cost (fees) | 5.23 USD |
| Product | ACCOUNT |
| Quote Validity | ~2 minutes |

***

### Retrieving a Quote

**Endpoint:** `GET /organizations/{tenant}/payout/quotes/{quoteId}`\
**Authentication:** Agent-level

```bash
curl --request GET \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/payout/quotes/$QUOTE_ID \
  --header "x-api-key: $API_KEY" \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY"
```

Use this to check if a quote is still valid before submitting a transaction.

***

### Handling Quote Expiration

Quotes have a short validity window (typically ~2 minutes). Follow these guidelines:

* **Always check `expireAt`** before using a quote in a transaction.
* **Refresh proactively** — request a new quote when ~90% of the validity period has elapsed.
* **Never cache quotes** — unlike destinations and bank lists, quotes are time-sensitive and must be fetched fresh.
* **Expired quotes will be rejected** — if you submit a transaction with an expired `quoteId`, the API returns a `400` or `422` error.

#### Recommended UX Pattern

```
1. User selects amount and destination  →  Request quote
2. Display rate, fees, and receive amount
3. Start a countdown timer based on expireAt
4. If timer reaches ~80%, show "Rate expiring" warning
5. If expired, auto-refresh and update the display
6. On "Confirm", immediately submit the transaction with the quoteId
```

***

### All Endpoints

| Operation | Method | Endpoint |
| --------- | ------ | -------- |
| Request quote | `POST` | `/organizations/{tenant}/payout/quotes` |
| Get quote by ID | `GET` | `/organizations/{tenant}/payout/quotes/{quoteId}` |

[Interactive API Documentation](https://dev-api.inyoglobal.com/sandbox/#tag/payout_Quote-Resource)
