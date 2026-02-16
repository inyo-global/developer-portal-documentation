# Data Population

Before building transaction flows, you need to populate your application with reference data from the Inyo API. This data includes available payout countries, required schemas for recipients and accounts, and bank lists.

***

### Overview

| Endpoint | Purpose | Cache Duration |
| -------- | ------- | -------------- |
| [Payout Countries](payout-countries-enabled.md) | Available destination countries for a source corridor | 24 hours |
| [Recipient Schema](recipient-schema.md) | Required fields to create a recipient in a given country | 24 hours |
| [Recipient Account Schema](recipient-account-schema.md) | Required fields to link a bank account in a given country | 24 hours |
| [Transaction Schema](transaction-schema.md) | Country-specific `additionalData` fields for transactions | 24 hours |
| [Banks in a Country](banks-in-a-country.md) | List of banks available for a destination country | 24 hours |

***

### Rate Limits

All data population endpoints are subject to strict rate limits. These endpoints serve reference data that changes infrequently — you are expected to **cache responses locally** and refresh periodically (recommended: every 24 hours).

**Do not call these endpoints on every transaction.** Excessive calls will result in `429 Too Many Requests` responses.

***

### Recommended Caching Strategy

```javascript
// Example: Cache with 24-hour TTL
const NodeCache = require('node-cache');
const cache = new NodeCache({ stdTTL: 86400 }); // 24 hours

async function getDestinations(sourceCountry) {
  const cacheKey = `destinations:${sourceCountry}`;
  let data = cache.get(cacheKey);
  
  if (!data) {
    data = await inyoApi.get(`/payout/${sourceCountry}/destinations`);
    cache.set(cacheKey, data);
  }
  
  return data;
}
```

***

### Schema-Driven Integration

A key design principle of the Inyo API is that **data requirements vary by country**. Instead of hardcoding field requirements, your integration should:

1. **Fetch the schema** for the destination country
2. **Dynamically render forms** based on required/optional fields
3. **Validate user input** against the schema before submitting

This approach ensures your application automatically adapts when Inyo adds new countries or changes requirements — without code changes on your side.

```
User selects destination country
        │
        ▼
Fetch recipient schema ──────────▶ Build person form
Fetch account schema ────────────▶ Build bank account form
Fetch bank list (if needed) ─────▶ Populate bank dropdown
Fetch transaction schema ────────▶ Collect additionalData fields
```
