# Trust Level Limits

The Trust Level (Compliance Level) system controls how much a sender is allowed to transact. Higher levels require more KYC data and documents but unlock higher transaction limits across three rolling time windows: **24 hours**, **30 days**, and **180 days**.

***

### Checking Compliance Level

**Endpoint:** `GET /organizations/{tenant}/participants/{participantId}/complianceLevels`\
**Authentication:** Tenant-level (`x-api-key`)

```bash
curl --request GET \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/participants/$SENDER_ID/complianceLevels \
  --header "x-api-key: $API_KEY" \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY"
```

This endpoint returns:
* The participant's **current compliance level** and its limits
* The **next available level** and the fields required to upgrade

***

### Sample Response

A participant at `LEVEL_0` (cannot transact yet) with upgrade path to `LEVEL_1`:

```json
{
  "currentLevel": {
    "level": "LEVEL_0",
    "limits": {
      "oneDayLimit": { "amount": "0.00", "currency": "USD" },
      "thirtyDaysLimit": { "amount": "0.00", "currency": "USD" },
      "oneHundredAndEightyDaysLimit": { "amount": "0.00", "currency": "USD" }
    }
  },
  "nextLevel": {
    "level": "LEVEL_1",
    "limits": {
      "oneDayLimit": { "amount": "2999.00", "currency": "USD" },
      "thirtyDaysLimit": { "amount": "6000.00", "currency": "USD" },
      "oneHundredAndEightyDaysLimit": { "amount": "9999.00", "currency": "USD" }
    },
    "requiredFields": [
      "firstName",
      "lastName",
      "phoneNumber",
      "residentialAddress"
    ]
  }
}
```

***

### Compliance Level Overview

| Level | Typical Required Fields | Description |
| ----- | ----------------------- | ----------- |
| `LEVEL_0` | (none) | Default level. **Cannot transact.** Participant exists but has not provided minimum KYC data. |
| `LEVEL_1` | firstName, lastName, address, phoneNumber | Basic KYC. Enables standard transaction volumes. |
| `LEVEL_2` | SSN/ITIN, occupation, document ID upload | Enhanced KYC. Unlocks higher limits. |
| `LEVEL_3` | Proof of source of funds | Full KYC. Maximum transaction limits. |

> ⚠️ **These levels are customizable per tenant.** Your specific required fields and limits are defined during the Inyo onboarding process and may differ from the defaults shown above.

***

### Checking Transaction Limits & Usage

To see how much of their limit a sender has already used:

**Endpoint:** `GET /organizations/{tenant}/fx/participants/{participantId}/limits`\
**Authentication:** Agent-level (`x-api-key` + `x-agent-id` + `x-agent-api-key`)

```bash
curl --request GET \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/fx/participants/$SENDER_ID/limits \
  --header "x-api-key: $API_KEY" \
  --header "x-agent-id: $AGENT_ID" \
  --header "x-agent-api-key: $AGENT_KEY"
```

**Sample Response:**

```json
{
  "oneDayLimit": {
    "limit": { "amount": "2999.00", "currency": "USD" },
    "used": { "amount": "500.00", "currency": "USD" },
    "available": { "amount": "2499.00", "currency": "USD" }
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
    "requiredFields": ["birthDate", "lastName", "name", "occupation", "phoneNumber", "residentialAddress"],
    "perTransactionFields": []
  }
}
```

**Key fields:**
* `limit` — Maximum amount for this time window
* `used` — Amount already consumed in the rolling window
* `available` — Remaining capacity (`limit - used`)
* `complianceLevel.requiredFields` — Fields needed for the *next* level upgrade

***

### Upgrading a Sender's Level

To move a sender from one level to the next:

1. **Check current level** — `GET /participants/{id}/complianceLevels`
2. **Identify missing fields** — Look at `nextLevel.requiredFields`
3. **Collect the data** — Update the sender's profile with the missing fields:
   * Use `PATCH /people/{personId}` for profile fields (SSN, occupation, etc.)
   * Use `PUT /people/{personId}/address` for address updates
   * Use document upload endpoints for ID documents and source of funds
4. **Verify the upgrade** — Call the compliance levels endpoint again to confirm the level increased

```
          LEVEL_0                 LEVEL_1                 LEVEL_2                 LEVEL_3
     ┌──────────────┐       ┌──────────────┐       ┌──────────────┐       ┌──────────────┐
     │  Cannot      │  Add  │  Standard    │  Add  │  Enhanced    │  Add  │  Maximum     │
     │  transact    │──────▶│  limits      │──────▶│  limits      │──────▶│  limits      │
     │              │ name, │              │ SSN,  │              │ proof │              │
     │  $0 / $0 / $0│ addr, │  $X/$X/$X    │ docs, │  $X/$X/$X    │ of    │  $X/$X/$X    │
     └──────────────┘ phone └──────────────┘ occup └──────────────┘ funds └──────────────┘
```

***

### Use Cases

* **Progressive onboarding** — Start with minimal data (Level 1) and prompt for more only when the user needs higher limits.
* **Limit warnings** — Show users their remaining capacity before they initiate a transaction.
* **Upgrade prompts** — When a transaction would exceed the current limit, show exactly which fields are missing and guide the user to complete them.
* **Compliance dashboard** — Display a visual progress bar showing the user's level and what's needed for the next tier.

***

### Pending Verifications

To check what the compliance team is still reviewing for a participant:

**Endpoint:** `GET /organizations/{tenant}/participants/{participantId}/pendingVerifications`\
**Authentication:** Tenant-level

```bash
curl --request GET \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/participants/$SENDER_ID/pendingVerifications \
  --header "x-api-key: $API_KEY"
```

This is useful for showing users that their documents are under review and their level upgrade is in progress.

[Interactive API Documentation](https://dev-api.inyoglobal.com/sandbox/#tag/account_Participant-Resource)
