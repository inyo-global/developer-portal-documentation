# Recipient

A **Recipient** (beneficiary) is the person or company who receives the funds. Recipients are created using the same participant endpoints as senders — the system distinguishes roles based on how the `participantId` is used in a transaction (`recipientId` field).

***

### Schema-Driven Recipient Creation

Recipient data requirements **vary by destination country**. Before collecting user data, always fetch the recipient schema:

```bash
curl --request GET \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/payout/recipients/schema/co \
  --header "x-api-key: $API_KEY" \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY"
```

This returns a JSON Schema (draft-07) defining exactly which fields are required for that country. See [Recipient Schema](data-population/recipient-schema.md) for details.

***

### Creating a Recipient

**Endpoint:** `POST /organizations/{tenant}/people`\
**Authentication:** Tenant-level (`x-api-key`)

#### Example: Brazilian Recipient

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

#### Example: Colombian Recipient

```bash
curl --request POST \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/people \
  --header 'Content-Type: application/json' \
  --header "x-api-key: $API_KEY" \
  --data '{
  "firstName": "Carlos",
  "lastName": "Gutierrez",
  "phoneNumber": "+573001234567",
  "address": {
    "countryCode": "CO",
    "city": "Bogotá",
    "line1": "Calle 100 #15-20"
  },
  "documents": [
    {
      "type": "CC",
      "document": "1234567890",
      "countryCode": "CO"
    }
  ]
}'
```

> ⚠️ **Save the returned `id`** — this is the `recipientId` used in the transaction.

***

### Country-Specific Document Types

| Country | Document Type | Code | Format |
| ------- | ------------- | ---- | ------ |
| Brazil | CPF (Cadastro de Pessoas Físicas) | `CPF` | 11 digits |
| Colombia | Cédula de Ciudadanía | `CC` | 8–10 digits |
| Mexico | CURP / INE | Varies | Per schema |
| Peru | DNI (Documento Nacional de Identidad) | `DNI` | 8 digits |
| India | PAN / Aadhaar | Varies | Per schema |
| USA | SSN / ITIN | `SSN`, `ITIN` | 9 digits |

> 💡 Always consult the recipient schema for the authoritative list of accepted document types per country.

***

### Reusing Participants

Participants are reusable entities:

* A person created as a **recipient** can later be used as a **sender** (if they meet compliance requirements)
* A person can act as **both sender and recipient** in the same transaction (self-send) by passing the same `participantId` for both `senderId` and `recipientId`
* A participant only needs to be created **once** and can be referenced in unlimited future transactions

***

### After Creating the Recipient

Once you have the `recipientId`, the next step is to link a bank account:

1. **Fetch the account schema** — `GET /payout/recipientAccounts/schema/{countryCode}`
2. **Fetch the bank list** (if required) — `GET /payout/{countryCode}/banks`
3. **Create the recipient account** — `POST /payout/participants/{recipientId}/recipientAccounts/gateway`

See [Recipient Account](recipient-account.md) for full details.

***

### Best Practices

* **Always use the schema endpoint** to determine required fields — don't hardcode field requirements per country.
* **Dynamically render forms** based on the schema response to automatically support new countries.
* **Collect only required fields** — submitting unnecessary data may trigger additional compliance checks.
* **Validate input client-side** using the schema's `pattern`, `minLength`, and `enum` constraints before submitting.
* **Handle the `address` requirement** — some countries require a recipient address, others don't. The schema is your source of truth.

[Interactive API Documentation](https://dev-api.inyoglobal.com/sandbox/#tag/account_Person-Resource)
