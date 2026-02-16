# Sender Funding Account

A Funding Account represents the source of funds used by the sender to pay for a transaction. Inyo supports multiple payment methods, all abstracted through a unified ledger system for compliance, reconciliation, and settlement tracking.

***

### Supported Payment Methods

| Method | Type Value | Description | Speed |
| ------ | ---------- | ----------- | ----- |
| Debit Card | `CARD` | Tokenized card payment via AFT (Account Funding Transaction) | Instant |
| ACH | `ACH` | US bank transfer via the Automated Clearing House network | 1–3 business days |
| Wallet | `WALLET` | Internal wallet / P2P balance | Instant |

***

### Creating a Funding Account

**Endpoint:** `POST /organizations/{tenant}/payout/participants/{participantId}/fundingAccounts`\
**Authentication:** Agent-level (`x-api-key` + `x-agent-id` + `x-agent-api-key`)

#### Common Request Fields

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `externalId` | string | No | Your internal reference for this funding source |
| `asset` | string | Yes | Currency code (e.g., `USD`) |
| `nickname` | string | No | Display name (e.g., "My Visa Debit") |
| `paymentMethod` | object | Yes | Payment method details (varies by type) |

***

#### Option A: Debit Card

Card payments require a **tokenized card number**. Raw card details are never sent to the Inyo Core API — they are first sent to the tokenization service (see [Tokenizing Cards](../payments-gateway/apis/tokenizing-cards.md)) to obtain a secure token.

```bash
curl --request POST \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/payout/participants/$SENDER_ID/fundingAccounts \
  --header 'Content-Type: application/json' \
  --header "x-api-key: $API_KEY" \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY" \
  --data '{
  "externalId": "funding-card-001",
  "asset": "USD",
  "nickname": "My Debit Card",
  "paymentMethod": {
    "type": "CARD",
    "token": "tok_12345_from_tokenizer",
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

**3D Secure (3DS) Challenge:**

Some cards require additional authentication. If the response returns `status: 'ActionRequired'`:

1. Save the funding account locally with a `Pending` status
2. Display the provided `redirectAcsUrl` in an iframe for the user to complete the challenge
3. Listen for a `postMessage` event from the iframe confirming authorization
4. Call `GET /payout/fundingAccounts/{externalId}` to verify the status is `Verified`
5. Only then allow the card to be used for transactions

For full 3DS implementation details, see [Handling 3D Secure](../payments-gateway/apis/payment/pulling-funds/cards/authorizing/handling-3d-secure.md).

***

#### Option B: ACH (US Bank Account)

For ACH funding, provide the routing and account number directly. Before registration, you must verify the bank account through an ACH verification partner to ensure the account is valid, has sufficient funds, and is not closed.

```bash
curl --request POST \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/payout/participants/$SENDER_ID/fundingAccounts \
  --header 'Content-Type: application/json' \
  --header "x-api-key: $API_KEY" \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY" \
  --data '{
  "externalId": "funding-ach-001",
  "asset": "USD",
  "nickname": "My Checking",
  "paymentMethod": {
    "type": "ACH",
    "countryCode": "US",
    "bankCode": "US_ACH",
    "routingNumber": "123456789",
    "accountNumber": "000123456789",
    "accountType": "CHECKING"
  }
}'
```

> ⚠️ **Save the returned `id`** — this is the `fundingAccountId` required when creating a transaction.

***

### Listing Funding Accounts

**Endpoint:** `GET /organizations/{tenant}/payout/participants/{participantId}/fundingAccounts`\
**Authentication:** Agent-level

```bash
curl --request GET \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/payout/participants/$SENDER_ID/fundingAccounts \
  --header "x-api-key: $API_KEY" \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY"
```

Use this to show the sender their saved payment methods and let them select one for a transaction.

***

### Getting a Funding Account

**Endpoint:** `GET /organizations/{tenant}/payout/participants/{participantId}/fundingAccounts/{fundingAccountId}`\
**Authentication:** Agent-level

```bash
curl --request GET \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/payout/participants/$SENDER_ID/fundingAccounts/$FUNDING_ACCOUNT_ID \
  --header "x-api-key: $API_KEY" \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY"
```

Use this to check a funding account's verification status (e.g., after a 3DS challenge).

***

### How Funding Works in the Transaction Lifecycle

```
1. Sender selects payment method  →  Use existing or create new funding account
2. Transaction submitted          →  Funds are debit-locked from the funding source
3. Compliance approved            →  Funds are collected (captured)
4. Payout processed               →  Funds settled to recipient via payout network
5. Transaction recorded           →  Ledger entry created for reconciliation
```

***

### All Endpoints

| Operation | Method | Endpoint |
| --------- | ------ | -------- |
| Create funding account | `POST` | `/organizations/{tenant}/payout/participants/{participantId}/fundingAccounts` |
| List funding accounts | `GET` | `/organizations/{tenant}/payout/participants/{participantId}/fundingAccounts` |
| Get funding account | `GET` | `/organizations/{tenant}/payout/participants/{participantId}/fundingAccounts/{fundingAccountId}` |

[Interactive API Documentation](https://dev-api.inyoglobal.com/sandbox/#tag/payout_Funding-Account-Resource)
