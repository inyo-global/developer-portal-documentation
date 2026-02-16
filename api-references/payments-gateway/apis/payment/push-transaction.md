---
description: >-
  Inyo Gateway also supports push transactions, a type of financial operation
  where the payment gateway facilitates the transfer of funds initiated by the
  sender to a recipient’s account, globally.
---

# Push Transaction

Supports multiple payment methods, including:

* **Credit/Debit Cards (domestic and cross-border)**
* **ACH Transfers (domestic)**
* **Accounts (cross-border)**

#### Root Object

| Field               | Type   | Required | Description                           |
| ------------------- | ------ | -------- | ------------------------------------- |
| `externalPaymentId` | string | ✅        | External identifier for the payment.  |
| `amount`            | object | ✅        | Payment source amount details.        |
| `recipientAmount`   | string | ✅        | Payment recipient amount details.     |
| `paymentType`       | string | ✅        | Must be `"PUSH"`.                     |
| `ipAddress`         | object | ✅        | Must be a valid IPv4 or IPv6 address. |
| `sender`            | object | ✅        | Contains sender details.              |
| `recipient`         | object | ✅        | Contains recipient details.           |

***

`amount` object

| Field      | Type   | Required | Validation      | Description                   |
| ---------- | ------ | -------- | --------------- | ----------------------------- |
| `total`    | number | ✅        | Must be >= 1    | Amount to be sent.            |
| `currency` | string | ✅        | Must be `"USD"` | Currency used in the payment. |

***

#### `recipientAmount` object

| Field      | Type   | Required | Validation        | Description                   |
| ---------- | ------ | -------- | ----------------- | ----------------------------- |
| `total`    | number | ✅        | Must be ≥ 1       | Amount to be received.        |
| `currency` | string | ✅        | 3-letter ISO code | Currency of recipient amount. |

**`sender` Object**

**`customer`**

|                     |        |   |                                                     |
| ------------------- | ------ | - | --------------------------------------------------- |
| `firstName`         | String | ✅ | Customer's first name.                              |
| `lastName`          | String | ✅ | Customer's last name.                               |
| `phoneNumber`       | String | ✅ | Digits only, 7–15 characters.                       |
| `documentNumber`    | String | ✅ | Digits only, 5–20 characters.                       |
| `documentType`      | String | ✅ | One of: `NATIONAL_ID`, `PASSPORT`, `DRIVER_LICENSE` |
| `email`             | String | ✅ | Must be a valid email address.                      |
| `countryCodeAlpha3` | String | ✅ | 3-letter uppercase country code (e.g., `USA`).      |

**`customerAddress`**

|             |        |   |                                          |
| ----------- | ------ | - | ---------------------------------------- |
| `stateCode` | String | ✅ | US state abbreviation (e.g., `MA`).      |
| `city`      | String | ✅ | City name.                               |
| `line1`     | String | ✅ | Street address line 1.                   |
| `line2`     | String | ❌ | Street address line 2 (optional).        |
| `state`     | String | ✅ | Full state name (e.g., `Massachusetts`). |
| `zipCode`   | String | ✅ | Postal code.                             |

#### `recipient` object

**`customer`**

<table><thead><tr><th width="192.515625">Field</th><th>Type</th><th>Required</th><th>Validation</th><th>Description</th></tr></thead><tbody><tr><td><code>firstName</code></td><td>string</td><td>✅</td><td>—</td><td>Recipient's first name.</td></tr><tr><td><code>lastName</code></td><td>string</td><td>✅</td><td>—</td><td>Recipient's last name.</td></tr><tr><td><code>phoneNumber</code></td><td>string</td><td>✅</td><td>7–15 digits (only numbers)</td><td>Recipient's phone number.</td></tr><tr><td><code>documentNumber</code></td><td>string</td><td>✅</td><td>5–20 digits</td><td>Recipient's document number.</td></tr><tr><td><code>documentType</code></td><td>string</td><td>✅</td><td><code>NATIONAL_ID</code>, <code>PASSPORT</code>, <code>DRIVER_LICENSE</code></td><td>Type of document.</td></tr><tr><td><code>email</code></td><td>string</td><td>✅</td><td>Must be a valid email</td><td>Recipient's email address.</td></tr><tr><td><code>countryCodeAlpha3</code></td><td>string</td><td>✅</td><td>3-letter uppercase (e.g., <code>USA</code>)</td><td>ISO Alpha-3 country code.</td></tr></tbody></table>

**`customerAddress`**

| Field       | Type   | Required | Description                          |
| ----------- | ------ | -------- | ------------------------------------ |
| `stateCode` | string | ✅        | State code (e.g., `SP`).             |
| `city`      | string | ✅        | City name.                           |
| `line1`     | string | ✅        | Street address (line 1).             |
| `line2`     | string | ❌        | Street address (line 2 - optional).  |
| `state`     | string | ✅        | Full state name (e.g., `São Paulo`). |
| `zipCode`   | string | ✅        | ZIP/postal code.                     |

***

#### `destination` object

| Field  | Type   | Required | Description                   |
| ------ | ------ | -------- | ----------------------------- |
| `type` | string | ✅        | Must be `"PIX"` or `"WALLET"` |

**🟢 If `type` = `PIX`**

| Field     | Type   | Required | Validation                          | Description     |
| --------- | ------ | -------- | ----------------------------------- | --------------- |
| `keyType` | string | ✅        | `EMAIL`, `PHONE`, `DOCUMENT`, `EVP` | Type of PIX key |
| `key`     | string | ✅        | —                                   | PIX key value   |

**🟢 If `type` = `WALLET`**

| Field      | Type   | Required | Description                     |
| ---------- | ------ | -------- | ------------------------------- |
| `walletId` | string | ✅        | Wallet identifier (email or ID) |

***

#### ✅ Example Payload – PIX

```json
{
    "externalPaymentId": "push_456",
    "amount": {
        "total": 55.00,
        "currency": "USD"
    },
    "recipientAmount": {
        "total": 57.9026948,
        "currency": "BRL"
    },
    "ipAddress": "2804:14d:8c80:9b21::1",
    "paymentType": "PUSH",
    "sender": {
        "customer": {
            "firstName": "Bob Danilo",
            "lastName": "Bob Danilo",
            "phoneNumber": "+1123456435",
            "documentNumber": "050482156",
            "documentType": "NATIONAL_ID",
            "email": "Danilo@gmail.com",
            "countryCodeAlpha3": "USA"
        },
        "customerAddress": {
            "stateCode": "CA",
            "city": "LAKEWOOD",
            "line1": "4429 CANDLEWOOD ST",
            "line2": "Some line 2 address",
            "zipCode": "90712",
            "countryCode": "US"
        }
    },
    "recipient": {
        "customer": {
            "firstName": "Carlos",
            "lastName": "Silva",
            "phoneNumber": "1122334455",
            "documentNumber": "12345678900",
            "documentType": "PASSPORT",
            "email": "carlos.silva@example.com",
            "countryCodeAlpha3": "BRA"
        },
        "customerAddress": {
            "stateCode": "RJ",
            "city": "Rio de Janeiro",
            "line1": "Rua das Laranjeiras 321",
            "state": "Rio de Janeiro",
            "zipCode": "22240-005"
        },
        "destination": {
            "type": "PIX",
            "pix": {
                "keyType": "CPF",
                "key": "01034861788"
            }
        }
    }
}
```

***

#### ✅ Example Payload – WALLET

```json
{
    "externalPaymentId": "push_456",
    "amount": {
        "total": 55.00,
        "currency": "USD"
    },
    "recipientAmount": {
        "total": 57.9026948,
        "currency": "BRL"
    },
    "ipAddress": "2804:14d:8c80:9b21::1",
    "paymentType": "PUSH",
    "sender": {
        "customer": {
            "firstName": "Bob Danilo",
            "lastName": "Bob Danilo",
            "phoneNumber": "+1123456435",
            "documentNumber": "050482156",
            "documentType": "NATIONAL_ID",
            "email": "Danilo@gmail.com",
            "countryCodeAlpha3": "USA"
        },
        "customerAddress": {
            "stateCode": "CA",
            "city": "LAKEWOOD",
            "line1": "4429 CANDLEWOOD ST",
            "line2": "Some line 2 address",
            "zipCode": "90712",
            "countryCode": "US"
        }
    },
    "recipient": {
        "customer": {
            "firstName": "Carlos",
            "lastName": "Silva",
            "phoneNumber": "1122334455",
            "documentNumber": "12345678900",
            "documentType": "PASSPORT",
            "email": "carlos.silva@example.com",
            "countryCodeAlpha3": "BRA"
        },
        "customerAddress": {
            "stateCode": "RJ",
            "city": "Rio de Janeiro",
            "line1": "Rua das Laranjeiras 321",
            "state": "Rio de Janeiro",
            "zipCode": "22240-005"
        },
        "destination": {
            "type": "WALLET",
            "wallet": {
                "walletId": "carlos.wallet@example.com"
            }
        }
    }
}
```
