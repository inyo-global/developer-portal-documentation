# Sender

The Sender is the participant who initiates and pays for a transaction. Every sender must pass through KYC (Know Your Customer) screening, and their transaction limits are determined by their compliance level.

***

### Initial Compliance Setup

At the beginning of your integration, the Inyo compliance team works with your organization to define a custom compliance framework tailored to your product and customer profile. This framework determines:

* **Compliance levels** — tiers that define how much a customer can send within set timeframes (24h, 30d, 180d)
* **Validation rules** — the required data and documents for each level (e.g., name, SSN, proof of income)
* **Risk controls** — thresholds that trigger enhanced due diligence

This configuration is unique per tenant and directly impacts how participants are verified and which operations they can perform.

***

### Creating a Sender

**Endpoint:** `POST /organizations/{tenant}/people`\
**Authentication:** Tenant-level (`x-api-key`)

No fields are technically *required* to create a person — but to use them as a sender, they must reach at least **Compliance Level 1**, which typically requires first name, last name, address, and phone number.

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
  "externalId": "your-internal-id-001",
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
  ],
  "occupation": "Software Engineer"
}'
```

#### Request Fields

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `firstName` | string | For Level 1 | First name |
| `lastName` | string | For Level 1 | Last name |
| `phoneNumber` | string | For Level 1 | Phone with country code (e.g., `+15551234567`) |
| `email` | string | For US persons | Email address |
| `gender` | string | For US persons | `Male`, `Female`, or `Other` |
| `birthDate` | string | For US persons | Format: `yyyy-MM-dd` |
| `externalId` | string | No | Your internal reference ID |
| `address` | object | For Level 1 | Residential address |
| `address.countryCode` | string | Yes (in address) | ISO 3166-1 alpha-2 |
| `address.stateCode` | string | For US | US state code (e.g., `CA`) |
| `address.city` | string | Yes (in address) | City name |
| `address.line1` | string | Yes (in address) | Street address |
| `address.line2` | string | No | Additional address info |
| `address.zipcode` | string | Yes (in address) | Postal code |
| `documents` | array | For Level 2+ | Identity documents |
| `documents[].type` | string | Yes (in doc) | `SSN`, `ITIN` (US), `CPF` (BR), etc. |
| `documents[].document` | string | Yes (in doc) | Document number |
| `documents[].countryCode` | string | Yes (in doc) | Issuing country |
| `occupation` | string | For Level 2 | Person's occupation |
| `employerName` | string | No | Employer name |

#### Sample Response

```json
{
  "id": "48066496-9445-41b7-acbe-85e069a77cb7",
  "firstName": "John",
  "lastName": "Doe",
  "mainAddressId": "9c2ea7e5-51a2-4ea1-83cf-8948754486f8",
  "phoneNumber": "+15551234567",
  "email": "john.doe@example.com",
  "gender": "Male",
  "birthDate": "1990-01-15",
  "externalId": "your-internal-id-001",
  "updatedAt": "2025-01-15T12:00:00",
  "documents": [],
  "occupation": "Software Engineer",
  "documentId": null,
  "sourceOfFundsId": null,
  "employerName": null,
  "employerAddressId": null
}
```

> ⚠️ **Save the returned `id`** — this is the `senderId` used in all subsequent API calls.

***

### Updating a Sender

**Endpoint:** `PATCH /organizations/{tenant}/people/{personId}`\
**Authentication:** Tenant-level

Only fields included in the request body are updated; omitted fields remain unchanged.

```bash
curl --request PATCH \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/people/$PERSON_ID \
  --header 'Content-Type: application/json' \
  --header "x-api-key: $API_KEY" \
  --data '{
  "occupation": "Consultant",
  "documents": [
    {
      "type": "SSN",
      "document": "987654321",
      "countryCode": "US"
    }
  ]
}'
```

***

### Address Verification

US addresses are automatically validated through USPS APIs. If validation fails, the sender's transactions will be placed **on hold** until the compliance team manually verifies the address.

**Verify an address before submitting:**

```bash
curl --request GET \
  --url "https://api.sandbox.inyoplatform.com/organizations/$TENANT/addresses/check?countryCode=US&stateCode=CA&city=San+Francisco&line1=123+Market+St&line2=&zipCode=94105" \
  --header "x-api-key: $API_KEY"
```

| Response Code | Meaning |
| ------------- | ------- |
| `202` | Address is valid |
| `200` | Address suggestion returned (USPS found a corrected version) |
| `400` | Address validation failed |

***

### Creating Business Senders (KYB)

For B2B use cases, you can create a company as a sender:

**Endpoint:** `POST /organizations/{tenant}/companies`\
**Authentication:** Tenant-level

Companies follow a similar compliance level system but with different required fields (business registration, EIN, etc.). Contact your Inyo account manager for your tenant-specific KYB configuration.

***

### Compliance Levels

| Level | Typical Required Fields | Description |
| ----- | ----------------------- | ----------- |
| Level 0 | (none) | Cannot transact |
| Level 1 | firstName, lastName, address, phoneNumber | Basic KYC |
| Level 2 | SSN, occupation, document ID | Enhanced KYC |
| Level 3 | Proof of source of funds | Full KYC |

> ⚠️ These are customizable per tenant. See [Trust Level Limits](trust-level-limits.md) for details.

***

### Best Practices

* **Progressive onboarding** — Collect only Level 1 fields at signup. Prompt for more data when the user needs higher limits.
* **Pre-validate addresses** — Use the address check endpoint before creating the sender to avoid holds.
* **Sync compliance state** — After profile updates, re-check the compliance level to see if it has upgraded.
* **Handle restricted users** — If a sender is `Restricted`, check `GET /participants/{id}/complianceLevels` to understand why and what action is needed.

***

### Related Pages

* [Trust Level Limits](trust-level-limits.md) — Check and upgrade compliance levels
* [Uploading Documents](uploading-documents.md) — Submit ID and source-of-funds documents
* [Test Data](test-data.md) — Sandbox testing scenarios for compliance flows

### All Endpoints

| Operation | Method | Endpoint |
| --------- | ------ | -------- |
| Create person | `POST` | `/organizations/{tenant}/people` |
| Update person | `PATCH` | `/organizations/{tenant}/people/{personId}` |
| Get person | `GET` | `/organizations/{tenant}/people/{personId}` |
| Get person with details | `GET` | `/organizations/{tenant}/people/{personId}/details` |
| Update address | `PUT` | `/organizations/{tenant}/people/{personId}/address` |
| Update employer address | `PUT` | `/organizations/{tenant}/people/{personId}/employerAddress` |
| Update place of birth | `PUT` | `/organizations/{tenant}/people/{personId}/placeOfBirth` |
| Check address | `GET` | `/organizations/{tenant}/addresses/check` |

[Interactive API Documentation](https://dev-api.inyoglobal.com/sandbox/#tag/account_Person-Resource)
