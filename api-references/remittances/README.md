# Remittances

The Remittances API provides a complete, end-to-end solution for building cross-border payment applications. It combines Inyo's **Core Services** (regulatory infrastructure and money transmission licensing), **Compliance Platform** (KYC, KYB, AML, and anti-fraud), and **Payment Gateway** (fund collection and disbursement) into a single, unified integration.

***

### How It Works

A remittance transaction follows a well-defined lifecycle that ensures regulatory compliance at every step:

```
┌─────────────┐    ┌─────────────┐    ┌──────────────┐    ┌─────────────┐
│   Sender     │    │  Compliance  │    │    Quote     │    │  Recipient   │
│  Onboarding  │───▶│ Verification │───▶│   (FX Rate)  │───▶│    Setup     │
│   (KYC)      │    │              │    │              │    │              │
└─────────────┘    └─────────────┘    └──────────────┘    └─────────────┘
                                                                 │
┌─────────────┐    ┌─────────────┐    ┌──────────────┐          │
│   Receipt    │    │  Transaction │    │   Funding    │◀─────────┘
│  (Regulated) │◀───│  Execution   │◀───│   Source     │
│              │    │              │    │  (Card/ACH)  │
└─────────────┘    └─────────────┘    └──────────────┘
```

***

### Integration Steps

| Step | Action | Endpoint | Description |
| ---- | ------ | -------- | ----------- |
| 1 | [Sender Onboarding](sender/) | `POST /people` | Create the sender and trigger KYC/AML screening |
| 2 | [Compliance Check](sender/trust-level-limits.md) | `GET /participants/{id}/complianceLevels` | Verify the sender has reached at least Level 1 |
| 3 | [Destinations](data-population/payout-countries-enabled.md) | `GET /payout/{country}/destinations` | Fetch available payout corridors |
| 4 | [Get a Quote](quotes-fx.md) | `POST /payout/quotes` | Lock in FX rate and fees |
| 5 | [Recipient Setup](recipient.md) | `POST /people` | Create the beneficiary using country-specific schemas |
| 6 | [Recipient Account](recipient-account.md) | `POST /payout/participants/{id}/recipientAccounts/gateway` | Link the recipient's bank account |
| 7 | [Funding Source](sender-funding-account.md) | `POST /payout/participants/{id}/fundingAccounts` | Register the sender's card or ACH account |
| 8 | [Limits Check](sender/trust-level-limits.md) | `GET /fx/participants/{id}/limits` | Verify the sender hasn't exceeded transaction limits |
| 9 | [Execute Transaction](transaction.md) | `POST /fx/transactions` | Submit the transaction with all linked IDs |
| 10 | [Webhooks](webhooks.md) | `POST /webhooks` | Register for real-time status notifications |

***

### Architecture Pattern

Inyo requires a **Backend-for-Frontend (BFF)** architecture. Your API credentials (`x-api-key`, `x-agent-id`, `x-agent-api-key`) must **never** be exposed to frontend or mobile clients.

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Frontend   │────▶│  Your Server │────▶│   Inyo API   │
│ (React, etc) │     │  (BFF Layer) │     │  (Sandbox /  │
│              │◀────│              │◀────│  Production) │
└──────────────┘     └──────────────┘     └──────────────┘
                      Holds API keys       KYC, FX, Payout
                      Orchestrates calls   Compliance engine
```

***

### Caching Guidelines

The Inyo API enforces rate limits on data-population endpoints. Implement caching as follows:

| Data | Cache Duration | Reason |
| ---- | -------------- | ------ |
| Destinations & banks | 24 hours | Rarely change |
| Compliance limits | 24 hours (or until a transaction occurs) | Semi-static |
| Recipient/account schemas | 24 hours | Rarely change |
| Quotes | **Never cache** | Expire within ~2 minutes |

***

### Transaction Types

The platform supports three transaction types:

| Type | Description | Use Case |
| ---- | ----------- | -------- |
| `FX` | Cross-border foreign exchange transaction | Remittances, international payouts |
| `TOP_UP` | Airtime or mobile top-up | Telecom recharges |
| `BILLPAY` | Bill payment | Utility and service payments |

***

### Need Help?

* **Reference implementation**: [Remittance Sample Project](https://github.com/inyo-global/remittance-sample) — a working Node.js + React app demonstrating the full integration
* **OpenAPI specification**: [Interactive API docs](https://dev-api.inyoglobal.com/sandbox/)
* **Support**: Get in touch with our sales team
