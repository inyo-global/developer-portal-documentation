# Getting Started

This guide walks you through sending your first cross-border transaction on the Inyo sandbox. By the end, you'll have a working remittance from a US sender to an international recipient.

***

### Prerequisites

Before you begin, ensure you have:

* **Sandbox credentials** provided by Inyo during onboarding:
  * `tenant` — Your organization identifier
  * `x-api-key` — Primary API key (Tenant-level)
  * `x-agent-id` — Your agent UUID
  * `x-agent-api-key` — Agent-level API key
* **Node.js v18+** (if running the [sample project](https://github.com/inyo-global/remittance-sample))
* A REST client (cURL, Postman, or similar)

> 📬 Don't have credentials yet? Get in touch with our sales team to request sandbox access.

***

### Sandbox Environment

| Resource           | URL                                                                                |
| ------------------ | ---------------------------------------------------------------------------------- |
| Core API (sandbox) | `https://api.sandbox.inyoplatform.com`                                             |
| OpenAPI docs       | [https://dev-api.inyoglobal.com/sandbox/](https://dev-api.inyoglobal.com/sandbox/) |
| Developer portal   | [https://dev.inyoglobal.com](https://dev.inyoglobal.com)                           |

***

### Quick Start: Your First Transaction

This walkthrough covers the minimum steps to execute a USD → BRL remittance in the sandbox.

#### Step 1: Create the Sender

Register the person who will send money. This triggers KYC/AML screening.

```bash
curl --request POST \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/people \
  --header 'Content-Type: application/json' \
  --header "x-api-key: $API_KEY" \
  --data '{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com",
  "birthDate": "1990-01-15",
  "phoneNumber": "+15551234567",
  "gender": "Male",
  "address": {
    "countryCode": "US",
    "stateCode": "CA",
    "city": "San Francisco",
    "line1": "123 Market St",
    "zipcode": "94105"
  },
  "documents": [
    {
      "type": "SSN",
      "document": "123456789",
      "countryCode": "US"
    }
  ]
}'
```

> ⚠️ **Save the returned `id`** — this is the `senderId` used throughout the transaction lifecycle.

#### Step 2: Verify Compliance Level

Confirm the sender has reached at least **Level 1** before proceeding.

```bash
curl --request GET \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/participants/$SENDER_ID/complianceLevels \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY"
```

If the response shows `LEVEL_0`, the sender is missing required fields. Check the `nextLevel.requiredFields` array to see what's needed.

#### Step 3: Get Available Destinations

```bash
curl --request GET \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/payout/us/destinations \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY"
```

This returns the list of countries your tenant is enabled to send to, along with their currencies.

#### Step 4: Lock in an FX Quote

```bash
curl --request POST \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/payout/quotes \
  --header 'Content-Type: application/json' \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY" \
  --data '{
  "fromCurrency": "USD",
  "toCurrency": "BRL",
  "amount": 100.00
}'
```

> ⚠️ **Save the `quoteId`** from the response. Quotes expire in \~2 minutes.

#### Step 5: Create the Recipient

First, fetch the required schema for the destination country:

```bash
curl --request GET \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/payout/recipients/schema/br \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY"
```

Then create the recipient using the schema-required fields:

```bash
curl --request POST \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/people \
  --header 'Content-Type: application/json' \
  --header "x-api-key: $API_KEY" \
  --data '{
  "firstName": "Maria",
  "lastName": "Silva",
  "phoneNumber": "+5511999998888",
  "documents": [
    {
      "type": "CPF",
      "document": "12345678901",
      "countryCode": "BR"
    }
  ]
}'
```

> ⚠️ **Save the returned `id`** — this is the `recipientId`.

#### Step 6: Link the Recipient's Bank Account

Fetch the account schema, then link the bank account:

```bash
curl --request POST \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/payout/participants/$RECIPIENT_ID/recipientAccounts/gateway \
  --header 'Content-Type: application/json' \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY" \
  --data '{
  "externalId": "recipient-acct-001",
  "asset": "BRL",
  "payoutMethod": {
    "type": "BANK_DEPOSIT",
    "countryCode": "BR",
    "bankCode": "001",
    "routingNumber": "0001",
    "accountNumber": "123456",
    "accountType": "CHECKING"
  }
}'
```

> ⚠️ **Save the returned `id`** — this is the `recipientAccountId`.

#### Step 7: Register the Sender's Funding Source

```bash
curl --request POST \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/payout/participants/$SENDER_ID/fundingAccounts \
  --header 'Content-Type: application/json' \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY" \
  --data '{
  "externalId": "funding-001",
  "asset": "USD",
  "nickname": "My Debit Card",
  "paymentMethod": {
    "type": "CARD",
    "token": "tok_sandbox_test_token",
    "billingAddress": {
      "countryCode": "US",
      "stateCode": "CA",
      "city": "San Francisco",
      "line1": "123 Market St",
      "zipcode": "94105"
    }
  }
}'
```

> ⚠️ **Save the returned `id`** — this is the `fundingAccountId`.

#### Step 8: Execute the Transaction

Link all the IDs together and submit:

```bash
curl --request POST \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/fx/transactions \
  --header 'Content-Type: application/json' \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY" \
  --data '{
  "senderId": "'$SENDER_ID'",
  "recipientId": "'$RECIPIENT_ID'",
  "fundingAccountId": "'$FUNDING_ACCOUNT_ID'",
  "recipientAccountId": "'$RECIPIENT_ACCOUNT_ID'",
  "quoteId": "'$QUOTE_ID'",
  "additionalData": {},
  "deviceData": {
    "userIpAddress": "127.0.0.1"
  }
}'
```

A successful response returns HTTP `202` with the full transaction object, including `complianceStatus`, `payoutStatus`, exchange rate details, and the `receipt` object containing legally required disclosures.

***

### What's Next?

* [Set up webhooks](webhooks.md) to receive real-time transaction status updates
* Review the [transaction lifecycle](transaction.md) to understand status transitions
* Explore [compliance levels](sender/trust-level-limits.md) to build progressive KYC flows
* Check [test data](sender/test-data.md) for sandbox testing scenarios

***

### Common Issues

| Symptom                 | Cause                                                  | Fix                                                                    |
| ----------------------- | ------------------------------------------------------ | ---------------------------------------------------------------------- |
| `401 Unauthorized`      | Invalid API keys                                       | Verify `x-api-key`, `x-agent-id`, and `x-agent-api-key` in your `.env` |
| `403 Forbidden`         | Agent not approved, or sender below compliance Level 1 | Check agent approval status; verify sender's compliance level          |
| `400 Missing fields`    | Incomplete sender/recipient profile                    | Check schema endpoints for required fields per country                 |
| `422 Unprocessable`     | Quote expired, limit exceeded, or invalid references   | Refresh the quote; check limits via `GET /fx/participants/{id}/limits` |
| `429 Too Many Requests` | Rate limit exceeded                                    | Implement caching for destinations, banks, and schemas (24h TTL)       |

***

### Feedback & Support

We're continuously improving these docs. For feedback, questions, or technical issues:

* 📧 Get in touch with our sales team
* 📖 [Interactive API reference](https://dev-api.inyoglobal.com/sandbox/)
* 💻 [Sample project on GitHub](https://github.com/inyo-global/remittance-sample)
