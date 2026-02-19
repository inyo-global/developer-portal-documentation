# Authentication

Inyo uses API keys to securely control access to its APIs. All requests must be authenticated — unauthenticated requests are rejected by the firewall.

***

### Credentials

During onboarding, Inyo provides you with the following credentials:

| Credential | Header | Description |
| ---------- | ------ | ----------- |
| Tenant ID | Path parameter (`{tenant}`) | Your organization identifier. Used in all endpoint URLs. |
| Tenant API Key | `x-api-key` | Primary key for account management, compliance queries, and listing operations. |
| Agent ID | `x-agent-id` | UUID identifying the agent that executes transactional operations. |
| Agent API Key | `x-agent-api-key` | Secret key paired with the agent, used for quotes, transactions, and payouts. |

***

### Authentication Layers

The API uses a **dual-layer authentication model**. Different endpoints require different credential combinations:

#### Tenant-Level Authentication

Used for account management, compliance, and read operations.

```http
GET /organizations/{tenant}/people/{personId}
Content-Type: application/json
Accept: application/json
x-api-key: {your_tenant_api_key}
```

**Endpoints using Tenant auth:**
* Person/Company CRUD (`POST /people`, `PATCH /people/{id}`, etc.)
* Compliance levels (`GET /participants/{id}/complianceLevels`)
* Transaction listing and status (`GET /fx/transactions`, `GET /fx/transactions/{id}/status`)
* Document uploads (`POST /people/{id}/documents/...`)
* Webhook management (`POST /webhooks`, `GET /webhooks`)
* Transaction cancel/refund operations
* Address verification

#### Agent-Level Authentication

Used for transactional operations that move money. Requires **both** the Tenant API key and Agent credentials.

```http
POST /organizations/{tenant}/fx/transactions
Content-Type: application/json
Accept: application/json
x-api-key: {your_tenant_api_key}
x-agent-id: {your_agent_id}
x-agent-api-key: {your_agent_api_key}
```

**Endpoints using Agent auth:**
* Quotes (`POST /payout/quotes`)
* Transactions (`POST /fx/transactions`)
* Funding accounts (`POST /payout/participants/{id}/fundingAccounts`)
* Recipient accounts (`POST /payout/participants/{id}/recipientAccounts/gateway`)
* Wallet operations

> ⚠️ **Agent Approval Required**: Newly created agents must be approved by the Inyo compliance team before they can execute transactions. Before approval, agent-authenticated requests will return `403 Forbidden`.

***

### Required Headers

Include the following headers on **every** request:

```http
Content-Type: application/json
Accept: application/json
x-api-key: {your_tenant_api_key}
```

For agent-authenticated endpoints, also include:

```http
x-agent-id: {your_agent_id}
x-agent-api-key: {your_agent_api_key}
```

***

### Environments

| Environment | Base URL |
| ----------- | -------- |
| Sandbox | `https://api.sandbox.inyoplatform.com` |
| Production | Provided during go-live (contact your Inyo account manager) |

> 💡 **Sandbox and production use separate credentials.** Never use sandbox keys in production or vice versa.

***

### Security Best Practices

* **Never expose API keys in frontend code.** Use a Backend-for-Frontend (BFF) pattern — your server holds the secrets and proxies requests to Inyo.
* **Rotate keys** if you suspect compromise. Contact our sales team for key rotation.
* **Restrict network access** — use IP allowlisting in production if supported by your plan.
* **Use HTTPS only** — all Inyo endpoints enforce TLS. Plain HTTP requests are rejected.

***

### Error Responses

| HTTP Status | Meaning |
| ----------- | ------- |
| `401 Unauthorized` | Missing or invalid `x-api-key`. |
| `403 Forbidden` | Valid key but insufficient permissions — agent not approved, or endpoint requires agent auth. |
| `429 Too Many Requests` | Rate limit exceeded. Implement caching and backoff. |
