# Webhooks

Webhooks allow you to receive real-time notifications when important events occur in the Inyo platform — such as a transaction changing status from `Pending` to `Paid`, or a document verification completing. Instead of polling the API, you register a callback URL and Inyo pushes events to you.

***

### Supported Events

| Event                                | Description                                     | When It Fires                                                  |
| ------------------------------------ | ----------------------------------------------- | -------------------------------------------------------------- |
| `TransactionStatusChanged`           | A transaction's payout status has changed       | Status transitions (e.g., Pending → Paid, Pending → Cancelled) |
| `documentUpdatedEvents`              | A document verification status has been updated | After compliance team reviews an uploaded document             |
| `agentStatusChangedEvent`            | An agent's status or configuration has changed  | Agent approval, suspension, or configuration updates           |
| `TransactionComplianceStatusChanged` | A transaction's compliance status has changed   | Status transitions (e.g., Hold → Approved, Hold → Rejected)    |



***

### Registering a Webhook

**Endpoint:** `POST /organizations/{tenant}/webhooks`\
**Authentication:** Tenant-level (`x-api-key`)

```bash
curl --request POST \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/webhooks \
  --header 'Content-Type: application/json' \
  --header "x-api-key: $API_KEY" \
  --data '{
  "events": [
    "transactionStatusChanged",
    "documentUpdatedEvents",
    "agentUpdatedEvents"
  ],
  "url": "https://your-server.com/api/webhooks/inyo"
}'
```

> 💡 You can register for all events or only the ones relevant to your integration.

***

### Listing Registered Webhooks

**Endpoint:** `GET /organizations/{tenant}/webhooks`\
**Authentication:** Tenant-level (`x-api-key`)

```bash
curl --request GET \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/webhooks \
  --header "x-api-key: $API_KEY"
```

***

### Deleting a Webhook

**Endpoint:** `DELETE /organizations/{tenant}/webhooks/{webhookId}`\
**Authentication:** Tenant-level (`x-api-key`)

```bash
curl --request DELETE \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/webhooks/$WEBHOOK_ID \
  --header "x-api-key: $API_KEY"
```

***

### Webhook Payload

When an event occurs, Inyo sends an HTTP `POST` request to your registered URL with a JSON payload. Your endpoint should:

1. Accept `POST` requests with `Content-Type: application/json`
2. Return an HTTP `2xx` response to acknowledge receipt
3. Process the event asynchronously (don't block the response)

#### Example: Transaction Status Changed

```json
{
  "event": "transactionStatusChanged",
  "transactionId": "9035ee18-052b-4176-a940-ccfe599a1829",
  "tenantId": "your_tenant",
  "complianceStatus": "Approved",
  "payoutStatus": "Paid",
  "externalId": "GMT000648512199",
  "timestamp": "2025-11-21T14:48:05.891186Z",
  "messages": [
    {
      "message": "STANDARD - PAYMENT_DELIVERED",
      "createdBy": "system",
      "type": "SystemUpdate"
    }
  ]
}
```

***

### Best Practices

#### Idempotency

Your webhook handler should be **idempotent** — the same event may be delivered more than once. Use the `transactionId` and event data to deduplicate.

#### Respond Quickly

Return a `200 OK` as soon as you receive the webhook. Perform heavy processing (database updates, notifications, etc.) asynchronously. If your endpoint takes too long to respond, the delivery may be retried.

#### Secure Your Endpoint

* **Use HTTPS** — always register an `https://` URL for your webhook endpoint.
* **Validate the source** — consider IP allowlisting or implementing shared-secret verification if supported by your integration agreement.
* **Don't trust the payload blindly** — after receiving a `transactionStatusChanged` event, fetch the transaction via `GET /fx/transactions/{id}` to confirm the current status before taking action.

#### Handle Failures Gracefully

If your endpoint returns a non-2xx response or is unreachable, Inyo will retry delivery. Design your system to handle:

* Duplicate deliveries (idempotency)
* Out-of-order deliveries (check timestamps)
* Delayed deliveries (don't assume real-time)

***

### Integration Example

Here's a minimal Node.js webhook handler:

```javascript
const express = require('express');
const app = express();
app.use(express.json());

app.post('/api/webhooks/inyo', async (req, res) => {
  // Acknowledge immediately
  res.status(200).send('OK');

  const { event, transactionId, complianceStatus, payoutStatus } = req.body;

  console.log(`[Webhook] ${event} — Transaction: ${transactionId}`);
  console.log(`  Compliance: ${complianceStatus}, Payout: ${payoutStatus}`);

  // Process asynchronously
  if (event === 'transactionStatusChanged') {
    if (payoutStatus === 'Paid') {
      // Notify sender, update your database, generate receipt
      await markTransactionComplete(transactionId);
    } else if (complianceStatus === 'Rejected') {
      // Handle rejection — notify sender, initiate refund if needed
      await handleRejection(transactionId);
    }
  }
});

app.listen(3001, () => console.log('Webhook listener on :3001'));
```

***

### All Endpoints

| Operation        | Method   | Endpoint                                       |
| ---------------- | -------- | ---------------------------------------------- |
| Register webhook | `POST`   | `/organizations/{tenant}/webhooks`             |
| List webhooks    | `GET`    | `/organizations/{tenant}/webhooks`             |
| Delete webhook   | `DELETE` | `/organizations/{tenant}/webhooks/{webhookId}` |

[Interactive API Documentation](https://dev-api.inyoglobal.com/sandbox/#tag/webhook_Webhooks-Resource)
