# Transaction

The Transaction endpoint is the culmination of the remittance flow. It links together the sender, recipient, funding source, recipient account, and quote to execute a cross-border payment.

***

### Creating a Transaction

**Endpoint:** `POST /organizations/{tenant}/fx/transactions`\
**Authentication:** Agent-level (`x-api-key` + `x-agent-id` + `x-agent-api-key`)

#### Request Body

| Field                      | Type          | Required    | Description                                                                                                                                            |
| -------------------------- | ------------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `senderId`                 | string (UUID) | Yes         | The participant ID of the sender (from `POST /people`)                                                                                                 |
| `recipientId`              | string (UUID) | Yes         | The participant ID of the recipient (from `POST /people`)                                                                                              |
| `fundingAccountId`         | string (UUID) | Yes         | The sender's funding source (from `POST /fundingAccounts`)                                                                                             |
| `recipientAccountId`       | string (UUID) | Yes         | The recipient's bank account (from `POST /recipientAccounts/gateway`)                                                                                  |
| `quoteId`                  | string (UUID) | Yes         | The locked FX quote (from `POST /payout/quotes`)                                                                                                       |
| `externalId`               | string (UUID) | No          | Your internal reference ID for idempotency and reconciliation                                                                                          |
| `additionalData`           | object        | Yes         | Country-specific data (see [Transaction Schema](data-population/transaction-schema.md)). Pass `{}` if no additional data is required for the corridor. |
| `deviceData`               | object        | No          | Device fingerprinting data for fraud prevention                                                                                                        |
| `deviceData.userIpAddress` | string        | Recommended | The end user's IP address                                                                                                                              |

#### Sample Request

```bash
curl --request POST \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/fx/transactions \
  --header 'Content-Type: application/json' \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY" \
  --data '{
  "senderId": "422a55b2-78c3-4509-8654-16e7375d3e40",
  "recipientId": "4b9dcb4e-2e2a-4e96-b2a9-fcd868b2f4ed",
  "fundingAccountId": "3fe5f23f-65d3-41b6-b34c-33db3c4d8ef9",
  "recipientAccountId": "67203d69-dc48-41d4-b2ac-52682d39b032",
  "quoteId": "30bf05d2-b416-4e71-b8e8-1ef158a2b414",
  "additionalData": {},
  "deviceData": {
    "userIpAddress": "200.123.131.112"
  }
}'
```

#### Sample Response (HTTP 202)

```json
{
  "id": "9035ee18-052b-4176-a940-ccfe599a1829",
  "sender": {
    "id": "ce06773d-6472-4c93-bdf5-8eb68ab01c7f",
    "type": "PERSON",
    "name": "Rob Dalas",
    "phoneNumber": "+1 123 456435"
  },
  "recipient": {
    "id": "156675d5-1d1b-4b4d-997e-5c15ae129b82",
    "type": "PERSON",
    "name": "Bob Danilo",
    "phoneNumber": "+55 71 91234-5678"
  },
  "tenantId": "master",
  "externalId": null,
  "agentId": "41f55211-1eea-4747-9a03-68e99f07ede5",
  "createdAt": "2025-05-29T17:26:27.264312",
  "anchorCurrencyAmount": {
    "amount": "102.53",
    "currency": "USD"
  },
  "complianceStatus": "Approved",
  "payoutStatus": "Pending",
  "status": {
    "id": "0097c1fb-56ac-4314-afa1-eaf00e712d5b",
    "transactionId": "e58796a9-e809-4503-8ce3-2c96c0a0e170",
    "complianceStatus": "Approved",
    "payoutStatus": "Pending",
    "messages": [
      {
        "id": "7122d9ab-2e60-41a4-94aa-e4231b1ca4f7",
        "message": "STANDARD - PAYMENT_RECEIVED",
        "createdBy": "system",
        "createdAt": "2025-11-21T11:48:10.268817",
        "type": "SystemUpdate"
      }
    ],
    "createdAt": "2025-11-21T14:48:05.891186"
  },
  "quoteId": "af2d55d8-1d40-426a-8d4a-7454ccdcbe8a",
  "fundingAccountId": "61bc14a1-b3a2-40b4-b395-0496e4f554a1",
  "recipientAccountId": "4fbc57a6-f673-44ba-88aa-c6e71eb9181b",
  "exchangeRate": "5.571062480000000",
  "totalAmount": {
    "amount": "102.53",
    "currency": "USD"
  },
  "receivingAmount": {
    "amount": "571.20",
    "currency": "BRL"
  },
  "fee": {
    "amount": "1.54",
    "currency": "USD"
  },
  "conversionAmount": {
    "amount": "102.53",
    "currency": "USD"
  },
  "originExchangeRate": {
    "fromCurrency": "USD",
    "toCurrency": "USD",
    "rate": "1.000000000000000"
  },
  "destinationExchangeRate": {
    "fromCurrency": "USD",
    "toCurrency": "BRL",
    "rate": "5.571062480000000"
  },
  "type": "FX",
  "senderId": "ce06773d-6472-4c93-bdf5-8eb68ab01c7f",
  "recipientId": "156675d5-1d1b-4b4d-997e-5c15ae129b82"
}
```

#### Response Fields

| Field                     | Description                                                        |
| ------------------------- | ------------------------------------------------------------------ |
| `id`                      | Unique transaction ID                                              |
| `complianceStatus`        | Current compliance screening result                                |
| `payoutStatus`            | Current payout delivery status                                     |
| `status.messages`         | Audit trail of status transitions with timestamps                  |
| `exchangeRate`            | The applied FX rate                                                |
| `totalAmount`             | Total amount charged to the sender (in source currency)            |
| `receivingAmount`         | Amount to be received by the beneficiary (in destination currency) |
| `fee`                     | Transaction fee in source currency                                 |
| `conversionAmount`        | Amount that was converted (source currency)                        |
| `originExchangeRate`      | Exchange rate from source currency to anchor currency              |
| `destinationExchangeRate` | Exchange rate from anchor currency to destination currency         |
| `type`                    | Transaction type: `FX`, `TOP_UP`, or `BILLPAY`                     |

***

### Transaction Status Lifecycle

Every transaction has two independent status dimensions:

#### Compliance Status

| Status     | Description                                            |
| ---------- | ------------------------------------------------------ |
| `Approved` | Passed all compliance checks — transaction can proceed |
| `Hold`     | Flagged for manual review by the compliance team       |
| `Rejected` | Failed compliance screening — transaction is blocked   |

#### Payout Status

| Status      | Description                                                         |
| ----------- | ------------------------------------------------------------------- |
| `Pending`   | Transaction accepted, awaiting processing                           |
| `Paid`      | Funds successfully delivered to the recipient                       |
| `Cancelled` | Transaction was cancelled (by user request or compliance rejection) |
| `Failed`    | Transaction was failed (by payment gateway)                         |
| `Refunded`  | Transaction was refunded                                            |

#### Compliance Status Transitions

```
                    ┌──────────┐
              ┌─────│          │─────┐
              │     └────┬─────┘     │
              │          │           │
              │          │           │
              │     ┌────▼─────┐     │  
              │┌────│   Hold   │────┐│
              ││    └──────────┘    ││
              ││                    ││
          ┌────▼──────┐       ┌─────▼▼────┐
          │ Rejected  │       │ Approved  │
          │(Cancelled)│       └───────────┘
          └───────────┘
```

***

### Querying Transactions

#### Get Transaction by ID

```bash
curl --request GET \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/fx/transactions/$TRANSACTION_ID \
  --header "x-api-key: $API_KEY"
```

#### List Transactions with Filters

```bash
curl --request GET \
  --url "https://api.sandbox.inyoplatform.com/organizations/$TENANT/fx/transactions?page=0&size=20&type=FX&complianceStatus=Approved" \
  --header "x-api-key: $API_KEY"
```

**Query Parameters:**

| Parameter          | Type    | Description                               |
| ------------------ | ------- | ----------------------------------------- |
| `page`             | integer | Page number (0-indexed)                   |
| `size`             | integer | Results per page                          |
| `type`             | string  | Filter by type: `FX`, `TOP_UP`, `BILLPAY` |
| `complianceStatus` | string  | Filter by compliance status               |
| `payoutStatus`     | string  | Filter by payout status                   |
| `senderName`       | string  | Filter by sender name                     |
| `receiverName`     | string  | Filter by receiver name                   |
| `from`             | string  | Start date filter                         |
| `to`               | string  | End date filter                           |

#### Get Transaction Status History

Returns the full audit trail of status changes for a transaction.

```bash
curl --request GET \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/fx/transactions/$TRANSACTION_ID/status \
  --header "x-api-key: $API_KEY"
```

**Sample Response:**

```json
[
  {
    "id": "2f6b3f12-a63f-43f7-9bb4-4e98dfc75fe5",
    "complianceStatus": "Approved",
    "payoutStatus": "Pending",
    "transactionId": "46b4c393-3768-4ba4-b6d2-a6398e17dd78",
    "tenantId": "master",
    "externalId": "GMT000648512199",
    "createdAt": "2024-01-03T12:54:09.455",
    "messages": [
      {
        "id": "0fbbddb8-283d-466d-b2e6-42dcbb3e67de",
        "message": "STANDARD - PAYMENT_RECEIVED",
        "createdBy": "system",
        "createdAt": "2025-11-19T13:29:52.221882",
        "type": "SystemUpdate"
      }
    ]
  }
]
```

***

### Cancelling a Transaction

Cancel a transaction that has not yet been paid out. Cancellation is allowed when the payout status is `Created`, `Hold`, `Authorized`, or `Transmitted`.

**Endpoint:** `PUT /organizations/{tenant}/fx/transactions/{transactionId}/status/cancel`\
**Authentication:** Tenant-level (`x-api-key`)

```bash
curl --request PUT \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/fx/transactions/$TRANSACTION_ID/status/cancel \
  --header "x-api-key: $API_KEY"
```

**Responses:**

| Status | Description                                               |
| ------ | --------------------------------------------------------- |
| `204`  | Transaction successfully cancelled                        |
| `400`  | Cannot cancel — transaction is in a non-cancellable state |
| `404`  | Transaction not found                                     |

> ⚠️ Once a transaction reaches `Paid` status, it cannot be cancelled — use the refund endpoint instead.

***

### Refunding a Transaction

Refund a transaction that has already been paid.

**Endpoint:** `PUT /organizations/{tenant}/fx/transactions/{transactionId}/status/refund`\
**Authentication:** Tenant-level (`x-api-key`)

```bash
curl --request PUT \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/fx/transactions/$TRANSACTION_ID/status/refund \
  --header "x-api-key: $API_KEY"
```

**Responses:**

| Status | Description                                                |
| ------ | ---------------------------------------------------------- |
| `204`  | Refund initiated                                           |
| `400`  | Cannot refund — transaction is not in `Paid` payout status |
| `404`  | Transaction not found                                      |

***

### Updating Transaction Metadata

Attach arbitrary key-value data to a transaction for your internal tracking.

**Endpoint:** `PUT /organizations/{tenant}/fx/transactions/{transactionId}/metadata`\
**Authentication:** Tenant-level (`x-api-key`)

```bash
curl --request PUT \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/fx/transactions/$TRANSACTION_ID/metadata \
  --header 'Content-Type: application/json' \
  --header "x-api-key: $API_KEY" \
  --data '{
  "internalRef": "INV-2025-001",
  "department": "treasury"
}'
```

***

### Transaction Limits

Before executing a transaction, verify the sender has not exceeded their limits.

**Endpoint:** `GET /organizations/{tenant}/fx/participants/{participantId}/limits`\
**Authentication:** Agent-level

```bash
curl --request GET \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/fx/participants/$SENDER_ID/limits \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY"
```

**Sample Response:**

```json
{
  "oneDayLimit": {
    "limit": { "amount": "2999.00", "currency": "USD" },
    "used": { "amount": "0.00", "currency": "USD" },
    "available": { "amount": "2999.00", "currency": "USD" }
  },
  "thirtyDaysLimit": {
    "limit": { "amount": "6000.00", "currency": "USD" },
    "used": { "amount": "2959.99", "currency": "USD" },
    "available": { "amount": "3040.01", "currency": "USD" }
  },
  "oneHundredAndEightyDaysLimit": {
    "limit": { "amount": "9999.00", "currency": "USD" },
    "used": { "amount": "5000.00", "currency": "USD" },
    "available": { "amount": "4999.00", "currency": "USD" }
  },
  "complianceLevel": {
    "level": "LEVEL_1",
    "requiredFields": ["birthDate", "lastName", "name", "occupation", "phoneNumber", "residentialAddress"]
  }
}
```

***

### Receipts (Regulatory Requirement)

As an agent of a licensed Money Service Business, you are **legally required** to issue a receipt to the sender immediately after transaction submission. The transaction response includes a `receipt` object with mandatory regulatory disclosures that **must be displayed verbatim** to the end user.

The receipt must include:

1. **Exchange rate** — the locked-in rate from the quote
2. **Fees** — total fees charged to the customer
3. **Total amount** — full amount paid by the sender
4. **Receive amount** — exact amount to be received by the beneficiary
5. **Regulatory disclosures** — dynamic legal text specific to the corridor (e.g., right to refund, cancellation policy)

> ⚠️ You **must not** alter the financial data or regulatory text returned by the API. Display it as-is.

***

### Error Responses

| HTTP Status | Error                | Cause                                                              |
| ----------- | -------------------- | ------------------------------------------------------------------ |
| `400`       | Bad request          | Missing required fields, invalid data format, or expired quote     |
| `403`       | Forbidden            | Agent not approved, or sender doesn't meet compliance requirements |
| `422`       | Unprocessable entity | Business rule violation (e.g., limit exceeded, invalid corridor)   |

***

### Additional Endpoints

| Operation              | Method | Endpoint                                                                |
| ---------------------- | ------ | ----------------------------------------------------------------------- |
| Create transaction     | `POST` | `/organizations/{tenant}/fx/transactions`                               |
| Get transaction        | `GET`  | `/organizations/{tenant}/fx/transactions/{transactionId}`               |
| List transactions      | `GET`  | `/organizations/{tenant}/fx/transactions`                               |
| Get status history     | `GET`  | `/organizations/{tenant}/fx/transactions/{transactionId}/status`        |
| Cancel transaction     | `PUT`  | `/organizations/{tenant}/fx/transactions/{transactionId}/status/cancel` |
| Refund transaction     | `PUT`  | `/organizations/{tenant}/fx/transactions/{transactionId}/status/refund` |
| Update metadata        | `PUT`  | `/organizations/{tenant}/fx/transactions/{transactionId}/metadata`      |
| Check limits           | `GET`  | `/organizations/{tenant}/fx/participants/{participantId}/limits`        |
| Get transaction schema | `GET`  | `/organizations/{tenant}/fx/transactions/schema?countryCode={iso2}`     |

[Interactive API Documentation](https://dev-api.inyoglobal.com/sandbox/#tag/fx_Transaction-Resource)
