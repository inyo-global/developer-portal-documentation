---
description: >-
  Inyo Gateway allows for combined pull and push transactions in a single
  operation.
---

# Pull and push in one step

Inyo Gateway allows for combined pull and push transactions in a single operation. This method is designed to streamline workflows where funds need to be collected from one account and subsequently transferred to another account, reducing latency and improving efficiency.

#### Root Object

| Field               | Type   | Required | Description                                         |
| ------------------- | ------ | -------- | --------------------------------------------------- |
| `externalPaymentId` | string | ✅        | External identifier for the payment.                |
| `amount`            | object | ✅        | Amount sent by the sender (in USD).                 |
| `recipientAmount`   | object | ✅        | Amount received by the recipient (in any currency). |
| `exchangeRate`      | number | ✅        | Exchange rate applied between currencies.           |
| `ipAddress`         | string | ✅        | IPv4 or IPv6 address of the request origin.         |
| `paymentType`       | string | ✅        | Must be `"PULLPUSH"`.                               |
| `sender`            | object | ✅        | Contains sender information.                        |
| `recipient`         | object | ✅        | Contains recipient information.                     |

***

#### `amount` object

| Field      | Type   | Required | Validation      | Description                  |
| ---------- | ------ | -------- | --------------- | ---------------------------- |
| `total`    | number | ✅        | Must be ≥ 1     | Amount in USD.               |
| `currency` | string | ✅        | Must be `"USD"` | Currency of the transaction. |

***

#### `recipientAmount` object

| Field      | Type   | Required | Validation        | Description                   |
| ---------- | ------ | -------- | ----------------- | ----------------------------- |
| `total`    | number | ✅        | Must be ≥ 1       | Amount to be received.        |
| `currency` | string | ✅        | 3-letter ISO code | Currency of recipient amount. |

***

#### `sender` object

Same structure as in PULL payments:

**`customer`**

| Field               | Type   | Required | Validation                                  |
| ------------------- | ------ | -------- | ------------------------------------------- |
| `firstName`         | string | ✅        | —                                           |
| `lastName`          | string | ✅        | —                                           |
| `phoneNumber`       | string | ✅        | 7–15 digits                                 |
| `documentNumber`    | string | ✅        | 5–20 digits                                 |
| `documentType`      | string | ✅        | `NATIONAL_ID`, `PASSPORT`, `DRIVER_LICENSE` |
| `email`             | string | ✅        | Must be valid email                         |
| `countryCodeAlpha3` | string | ✅        | Must be 3 uppercase letters                 |

**`customerAddress`**

| Field       | Type   | Required |
| ----------- | ------ | -------- |
| `stateCode` | string | ✅        |
| `city`      | string | ✅        |
| `line1`     | string | ✅        |
| `line2`     | string | ❌        |
| `state`     | string | ✅        |
| `zipCode`   | string | ✅        |

**`source`**

| Field  | Type   | Required | Description                  |
| ------ | ------ | -------- | ---------------------------- |
| `type` | string | ✅        | `"CARD"` or `"BANK_ACCOUNT"` |

**🟢 If `type` = `CARD`**

| Field   | Type   | Required | Validation | Description |
| ------- | ------ | -------- | ---------- | ----------- |
| `token` | string | ✅        | UUID       | Card token  |

**🟢 If `type` = `BANK_ACCOUNT`**

| Field           | Type   | Required |
| --------------- | ------ | -------- |
| `accountType`   | string | ✅        |
| `accountNumber` | string | ✅        |
| `routingNumber` | string | ✅        |
| `accountHolder` | object | ✅        |

**`accountHolder`**

| Field       | Type   | Required | Description                  |
| ----------- | ------ | -------- | ---------------------------- |
| `type`      | string | ✅        | `"personal"` or `"business"` |
| `firstName` | string | ✅        | First name                   |
| `lastName`  | string | ✅        | Last name                    |

***

#### `recipient` object

Same structure as in PUSH payments:

**`customer`, `customerAddress`: same as sender**

**`destination`**

| Field  | Type   | Required | Description           |
| ------ | ------ | -------- | --------------------- |
| `type` | string | ✅        | `"PIX"` or `"WALLET"` |

**🟢 If `type` = `PIX`**

| Field     | Type   | Required | Description                                 |
| --------- | ------ | -------- | ------------------------------------------- |
| `keyType` | string | ✅        | `"EMAIL"`, `"PHONE"`, `"DOCUMENT"`, `"EVP"` |
| `key`     | string | ✅        | PIX key                                     |

**🟢 If `type` = `WALLET`**

| Field      | Type   | Required | Description       |
| ---------- | ------ | -------- | ----------------- |
| `walletId` | string | ✅        | Wallet identifier |

***

#### ✅ Example Payload – PULLPUSH (CARD + WALLET)

```json
{
  "externalPaymentId": "pullpush_001",
  "amount": {
    "total": 100.00,
    "currency": "USD"
  },
  "recipientAmount": {
    "total": 497.50,
    "currency": "BRL"
  },
  "exchangeRate": 4.975,
  "ipAddress": "2001:db8::ff00:42:8329",
  "paymentType": "PULLPUSH",
  "sender": {
    "customer": {
      "firstName": "Lucas",
      "lastName": "Silva",
      "phoneNumber": "11999999999",
      "documentNumber": "12345678900",
      "documentType": "NATIONAL_ID",
      "email": "lucas.silva@example.com",
      "countryCodeAlpha3": "USA"
    },
    "customerAddress": {
      "stateCode": "CA",
      "city": "San Francisco",
      "line1": "123 Mission St",
      "line2": "Suite 900",
      "state": "California",
      "zipCode": "94105"
    },
    "source": {
      "type": "CARD",
      "card": {
        "token": "123e4567-e89b-12d3-a456-426614174000"
      }
    }
  },
  "recipient": {
    "customer": {
      "firstName": "Ana",
      "lastName": "Souza",
      "phoneNumber": "21987654321",
      "documentNumber": "10987654321",
      "documentType": "PASSPORT",
      "email": "ana.souza@example.com",
      "countryCodeAlpha3": "BRA"
    },
    "customerAddress": {
      "stateCode": "RJ",
      "city": "Rio de Janeiro",
      "line1": "Rua da Alfândega, 45",
      "state": "Rio de Janeiro",
      "zipCode": "20010-030"
    },
    "destination": {
      "type": "WALLET",
      "wallet": {
        "walletId": "ana.souza@wallet.com"
      }
    }
  }
}
```
