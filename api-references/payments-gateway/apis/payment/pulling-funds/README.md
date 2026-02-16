---
description: >-
  Inyo Gateway facilitates pull transactions, a type of financial operation
  where the payment gateway initiates a request to withdraw funds from a
  customer's account.
---

# Pulling funds

Supports multiple payment methods, including:

* **Credit/Debit Cards**
* **ACH Transfers**

**Fields Definition**

Root Object

| Field               | Type    | Required | Description                                 |
| ------------------- | ------- | -------- | ------------------------------------------- |
| `externalPaymentId` | String  | ✅        | Client external identifier for the payment. |
| `capture`           | Boolean | ✅        | true or false                               |
| `amount`            | Object  | ✅        | Contains the payment amount information.    |
| `ipAddress`         | String  | ✅        | IPv4 address of the request origin.         |
| `paymentType`       | String  | ✅        | Must be `"PULL"`.                           |
| `sender`            | Object  | ✅        | Sender information.                         |

***

#### `amount` Object

| Field      | Type   | Required | Constraints     | Description                 |
| ---------- | ------ | -------- | --------------- | --------------------------- |
| `total`    | Number | ✅        | >= 1            | Payment amount              |
| `currency` | String | ✅        | Must be `"USD"` | Currency of the transaction |

***

#### `sender` Object

&#x20;**`customer`**

| Field               | Type   | Required | Description                                         |
| ------------------- | ------ | -------- | --------------------------------------------------- |
| `firstName`         | String | ✅        | Customer's first name.                              |
| `lastName`          | String | ✅        | Customer's last name.                               |
| `phoneNumber`       | String | ✅        | Digits only, 7–15 characters.                       |
| `documentNumber`    | String | ✅        | Digits only, 5–20 characters.                       |
| `documentType`      | String | ✅        | One of: `NATIONAL_ID`, `PASSPORT`, `DRIVER_LICENSE` |
| `email`             | String | ✅        | Must be a valid email address.                      |
| `countryCodeAlpha3` | String | ✅        | 3-letter uppercase country code (e.g., `USA`).      |

**`customerAddress`**

| Field       | Type   | Required | Description                              |
| ----------- | ------ | -------- | ---------------------------------------- |
| `stateCode` | String | ✅        | US state abbreviation (e.g., `MA`).      |
| `city`      | String | ✅        | City name.                               |
| `line1`     | String | ✅        | Street address line 1.                   |
| `line2`     | String | ❌        | Street address line 2 (optional).        |
| `state`     | String | ✅        | Full state name (e.g., `Massachusetts`). |
| `zipCode`   | String | ✅        | Postal code.                             |

***

#### `source` Object

| Field  | Type   | Required | Description                       |
| ------ | ------ | -------- | --------------------------------- |
| `type` | String | ✅        | Must be `CARD` or `BANK_ACCOUNT`. |

**🟢 If `type` = `CARD`**

| Field   | Type   | Required | Description           |
| ------- | ------ | -------- | --------------------- |
| `token` | String | ✅        | Must be a valid UUID. |

**🟢 If `type` = `BANK_ACCOUNT`**

| Field           | Type   | Required | Description                                                                    |
| --------------- | ------ | -------- | ------------------------------------------------------------------------------ |
| `accountType`   | String | ✅        | One of: `savings`, `checking`, `loan`, `business_checking`, `business_savings` |
| `accountNumber` | String | ✅        | 6–20 digits.                                                                   |
| `routingNumber` | String | ✅        | Must be 9 digits.                                                              |
| `accountHolder` | Object | ✅        | Information about the account holder.                                          |

**`accountHolder` Object**

| Field       | Type   | Required | Description                       |
| ----------- | ------ | -------- | --------------------------------- |
| `type`      | String | ✅        | Either `personal` or `business`.  |
| `firstName` | String | ✅        | First name of the account holder. |
| `lastName`  | String | ✅        | Last name of the account holder.  |

***

####
