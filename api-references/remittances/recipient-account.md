# Recipient Account

A Recipient Account represents the bank account or payout destination where funds will be delivered. Before creating a recipient account, you must first [create the recipient](recipient.md) as a participant and [fetch the account schema](data-population/recipient-account-schema.md) for the destination country.

***

### Schema-Driven Creation

Recipient account requirements **vary by country**. Always fetch the account schema before building your form or API call:

```bash
# Fetch account schema for Colombia
curl --request GET \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/payout/recipientAccounts/schema/co \
  --header "x-api-key: $API_KEY" \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY"
```

The schema returns a JSON Schema (draft-07) object that defines required fields, allowed values, and validation rules. Common fields include:

| Field | Description | Examples |
| ----- | ----------- | -------- |
| `asset` | Currency code (ISO 4217) | `BRL`, `MXN`, `COP`, `PEN` |
| `payoutMethod.type` | Payout mechanism | `BANK_DEPOSIT` |
| `payoutMethod.countryCode` | Destination country | `BR`, `MX`, `CO` |
| `payoutMethod.bankCode` | Bank identifier | Varies by country (use [Banks endpoint](data-population/banks-in-a-country.md)) |
| `payoutMethod.routingNumber` | Routing/SWIFT/BIC code | Country-specific |
| `payoutMethod.accountNumber` | Account number | CLABE (MX), IBAN, or local account number |
| `payoutMethod.accountType` | Account type | `CHECKING`, `SAVINGS` |

> 💡 **Tip:** For countries that require a `bankCode`, use the [Banks in a Country](data-population/banks-in-a-country.md) endpoint to populate a dropdown in your UI.

***

### Creating a Recipient Account

**Endpoint:** `POST /organizations/{tenant}/payout/participants/{participantId}/recipientAccounts/gateway`\
**Authentication:** Agent-level (`x-api-key` + `x-agent-id` + `x-agent-api-key`)

#### Request Body

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `externalId` | string | No | Your internal identifier for this account |
| `asset` | string | Yes | Currency code (ISO 4217) for the recipient's account |
| `payoutMethod` | object | Yes | Bank account details (country-specific, see schema) |

#### Example: Colombia (Bank Deposit)

```bash
curl --request POST \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/payout/participants/$RECIPIENT_ID/recipientAccounts/gateway \
  --header 'Content-Type: application/json' \
  --header "x-api-key: $API_KEY" \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY" \
  --data '{
  "externalId": "acct-co-001",
  "asset": "COP",
  "payoutMethod": {
    "type": "BANK_DEPOSIT",
    "countryCode": "CO",
    "bankCode": "1001",
    "routingNumber": "1234",
    "accountNumber": "9876543210",
    "accountType": "SAVINGS"
  }
}'
```

#### Example: Mexico (CLABE)

```bash
curl --request POST \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/payout/participants/$RECIPIENT_ID/recipientAccounts/gateway \
  --header 'Content-Type: application/json' \
  --header "x-api-key: $API_KEY" \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY" \
  --data '{
  "externalId": "acct-mx-001",
  "asset": "MXN",
  "payoutMethod": {
    "type": "BANK_DEPOSIT",
    "countryCode": "MX",
    "bankCode": "002",
    "routingNumber": "002",
    "accountNumber": "012345678901234567",
    "accountType": "CHECKING"
  }
}'
```

#### Example: Peru (Bank Deposit)

```bash
curl --request POST \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/payout/participants/$RECIPIENT_ID/recipientAccounts/gateway \
  --header 'Content-Type: application/json' \
  --header "x-api-key: $API_KEY" \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY" \
  --data '{
  "externalId": "acct-pe-001",
  "asset": "PEN",
  "payoutMethod": {
    "type": "BANK_DEPOSIT",
    "countryCode": "PE",
    "bankCode": "BINPPEPL",
    "routingNumber": "1234",
    "accountNumber": "1345",
    "accountType": "CHECKING"
  }
}'
```

> ⚠️ **Save the returned `id`** — this is the `recipientAccountId` required when creating a transaction.

***

### Getting a Recipient Account

**Endpoint:** `GET /organizations/{tenant}/payout/participants/{participantId}/recipientAccounts/{recipientAccountId}`\
**Authentication:** Agent-level

```bash
curl --request GET \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/payout/participants/$RECIPIENT_ID/recipientAccounts/$ACCOUNT_ID \
  --header "x-api-key: $API_KEY" \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY"
```

***

### Deleting a Recipient Account

**Endpoint:** `DELETE /organizations/{tenant}/payout/participants/{participantId}/recipientAccounts/{recipientAccountId}`\
**Authentication:** Agent-level

```bash
curl --request DELETE \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/payout/participants/$RECIPIENT_ID/recipientAccounts/$ACCOUNT_ID \
  --header "x-api-key: $API_KEY" \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY"
```

***

### Country-Specific Requirements

| Country | Currency | Key Field | Document Type | Notes |
| ------- | -------- | --------- | ------------- | ----- |
| Brazil (BR) | BRL | `accountNumber` | CPF | PIX keys may be supported depending on your corridor config |
| Mexico (MX) | MXN | `accountNumber` (CLABE, 18 digits) | CURP / INE | CLABE is the standard interbank identifier |
| Colombia (CO) | COP | `bankCode` + `accountNumber` | CC (Cédula) | Use the Banks endpoint for valid `bankCode` values |
| Peru (PE) | PEN | `bankCode` + `accountNumber` | DNI | Bank codes use SWIFT/BIC format |
| India (IN) | INR | `routingNumber` (IFSC) | PAN / Aadhaar | IFSC code is mandatory for Indian bank transfers |
| Philippines (PH) | PHP | `bankCode` + `accountNumber` | — | — |

> 💡 Always use the schema endpoint as the source of truth — the table above is for guidance only.

***

### Integration Workflow

```
1. Fetch account schema  →  GET /payout/recipientAccounts/schema/{countryCode}
2. Fetch bank list        →  GET /payout/{countryCode}/banks (if bankCode is required)
3. Collect user input     →  Build form from schema
4. Create account         →  POST /payout/participants/{id}/recipientAccounts/gateway
5. Save account ID        →  Use in POST /fx/transactions
```

***

### All Endpoints

| Operation | Method | Endpoint |
| --------- | ------ | -------- |
| Create recipient account | `POST` | `/organizations/{tenant}/payout/participants/{participantId}/recipientAccounts/gateway` |
| Get recipient account | `GET` | `/organizations/{tenant}/payout/participants/{participantId}/recipientAccounts/{recipientAccountId}` |
| Delete recipient account | `DELETE` | `/organizations/{tenant}/payout/participants/{participantId}/recipientAccounts/{recipientAccountId}` |

[Interactive API Documentation](https://dev-api.inyoglobal.com/sandbox/#tag/payout_Recipient-Account-Resource)
