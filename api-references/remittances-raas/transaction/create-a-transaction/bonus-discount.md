---
description: Read on building bonus and discount features for your remittance service
---

# Bonus/Discount

## Overview <a href="#overview" id="overview"></a>

During transaction creation, you can apply a bonus or discount to a particular transaction. Our clients have used this feature to improve their customer acquisition and retention strategies. Based on how you build your application, you can support multiple different use cases for bonus and discount offers. In general, bonus features can be implemented by providing a value for the “bonus\_amount” field in the [Create Transaction API](./).

On the Machnet system, the criteria required for transaction creation remains the same for transactions with bonus similar transactions without bonus. However, Clients can build different criteria on their system to provide bonuses based on their use case. The most common implementations are listed below with details on how to build them.

## Implementation 1: Bonus provided to the Receive **User** <a href="#implementation-1-bonus-provided-to-the-receive-user" id="implementation-1-bonus-provided-to-the-receive-user"></a>

This implementation allows a transaction to be created with a bonus amount where the bonus amount is provided to the receive user. You can choose to build logics on when to provide this bonus. It could be a first time user or a loyal user who has created 10 transactions already. When your bonus criteria is met, just be sure to include the bonus amount in the “from\_amount” field and provide the bonus amount in the “bonus\_amount” field as well in the Create Transaction API. Detailed examples are below.

{% tabs %}
{% tab title="Example 1" %}
**Bonus amount in sending currency**

Amount entered by sender: USD 100

Bonus provided by the client USD 5

| **Field**                                   | **Amount** | **Description**                                     |
| ------------------------------------------- | ---------- | --------------------------------------------------- |
| from\_amount                                | USD 105    | Amount entered by sender ($100) + Bonus amount ($5) |
| fee\_amount                                 | USD 2      | Fee to be added, if applicable                      |
| bonus\_amount                               | USD 5      | Bonus provided to the receiver                      |
| Total amount deducted from sender’s account | USD 102    | from\_amount + fee\_amount - bonus\_amount          |
| exchange\_rate                              | 14         | Exchange rate (USD 1=GHS 14)                        |
| Total amount received by the receiver       | GHS 1470   | from\_amount\*exchange\_rate                        |

**Sample Request**

```
curl --location --request POST '{{url}}/users/{{user_id}}/transactions' \
--header 'X-Client-Id: client_id' \
--header 'X-Client-Secret: client_secret' \
--header 'Content-Type: application/json' \
--data-raw '{
  "sender_amount":100,
  "exchange_rate":14,
  "fee_amount": 2,
  "bonus_amount":5,
  "sender_funding_account_id": "{{accountId}}",
  "recipient_id": "{{recipientId}}",
  "recipient_account_id":"{{recipientBankId}}",
  "recipient_currency": "GHS",
  "remittance_purpose": "House Purchase",
  "ip_address": "10.10.10.5",
  "funding_source" : "BANK",
  "payout_method": "BANK_DEPOSIT"
}'
```
{% endtab %}

{% tab title="Example 2" %}
**Bonus amount in receiving currency**

Amount entered by sender: USD 100

Discount provided by the client: GHS 140

| **Field**                                     | **Amount** | **Description**                                                                                              |
| --------------------------------------------- | ---------- | ------------------------------------------------------------------------------------------------------------ |
| from\_amount                                  | USD 110    | Amount entered by sender (100) + Bonus amount (10)                                                           |
| fee\_amount                                   | USD 2      | Fee to be added, if applicable                                                                               |
| bonus\_amount                                 | USD 10     | <p>Discount to be provided to the sender:</p><p>Discount provided by Client (GHS 140)/Exchange rate (14)</p> |
| Total amount deducted from sender’s account\* | USD 102    | from\_amount + fee\_amount - bonus\_amount                                                                   |
| exchange\_rate                                | 14         | Exchange rate (USD 1=GHS 14)                                                                                 |
| Total amount received by the receiver         | GHS 1540   | from\_amount\*exchange\_rate                                                                                 |

**Sample Request**

```
curl --location --request POST '{{url}}/users/{{user_id}}/transactions' \
--header 'X-Client-Id: client_id' \
--header 'X-Client-Secret: client_secret' \
--header 'Content-Type: application/json' \
--data-raw '{
  "sender_amount":100,
  "exchange_rate":14,
  "fee_amount": 2,
  "bonus_amount":10,
  "sender_funding_account_id": "{{accountId}}",
  "recipient_id": "{{recipientId}}",
    "recipient_account_id":"{{recipientBankId}}",
  "recipient_currency": "GHS",
  "remittance_purpose": "House Purchase",
  "ip_address": "10.10.10.5",
  "funding_source" : "BANK",
  "payout_method": "BANK_DEPOSIT"
}' 
```
{% endtab %}
{% endtabs %}

## Implementation 2: Discount provided to the Send user <a href="#implementation-2-discount-provided-to-the-send-user" id="implementation-2-discount-provided-to-the-send-user"></a>

This implementation allows a transaction to be created where a discount equal to the bonus amount is provided to the send user. You can choose to build logics on when to provide this discount to your user. It could be a first time user or a loyal user who has created 10 transactions already. When a criteria is met, just be sure to send us the amount you would like to discount in the “bonus\_amount” field in the Create Transaction API.

{% tabs %}
{% tab title="Example" %}
**Discount provided to the send user**

Amount entered by sender: USD 100

Discount provided by the client: USD 5

| **Field**                                     | **Amount** | **Description**                            |
| --------------------------------------------- | ---------- | ------------------------------------------ |
| from\_amount                                  | USD 100    | Amount entered by sender                   |
| fee\_amount                                   | USD 2      | Fee to be added, if applicable             |
| bonus\_amount                                 | USD 5      | Discount to be provided to the sender      |
| Total amount deducted from sender’s account\* | USD 97     | from\_amount + fee\_amount - bonus\_amount |
| exchange\_rate                                | 14         | Exchange rate (USD 1=GHS 14)               |
| Total amount received by the receiver         | GHS 1400   | from\_amount\*exchange\_rate               |

**Sample Request**

```
curl --location --request POST '{{url}}/users/{{user_id}}/transactions' \
--header 'X-Client-Id: client_id' \
--header 'X-Client-Secret: client_secret' \
--header 'Content-Type: application/json' \
--data-raw '{
  "sender_amount":100,
  "exchange_rate":14,
  "fee_amount": 2,
  "bonus_amount":5,
  "sender_funding_account_id": "{{accountId}}",
  "recipient_id": "{{recipientId}}",
  "recipient_account_id":"{{recipientBankId}}",
  "recipient_currency": "GHS",
  "remittance_purpose": "House Purchase",
  "ip_address": "10.10.10.5",
  "funding_source" : "BANK",
  "payout_method": "BANK_DEPOSIT"
  }
'
```
{% endtab %}
{% endtabs %}

## Implementation 3: Bonus FX rate <a href="#implementation-3-bonus-fx-rate" id="implementation-3-bonus-fx-rate"></a>

You can provide a bonus FX rate to your users to remain competitive in the market. When you create a transaction, you will have to provide us the FX rate applicable to the specific transaction which should include the bonus FX.

{% tabs %}
{% tab title="Example" %}
**Provide higher FX to the user**

Amount entered by sender: USD USD 100

Exchange rate: USD 1 = GHS 14

Bonus FX provided by the client for the user: USD 1=GHS 20

| **Field**                                     | **Amount** | **Description**                                 |
| --------------------------------------------- | ---------- | ----------------------------------------------- |
| from\_amount                                  | USD 100    | Amount entered by sender                        |
| fee\_amount                                   | USD 2      | Fee to be added, if applicable                  |
| bonus\_amount                                 | -          | Bonus provided to the receiver                  |
| Total amount deducted from sender’s account\* | USD 102    | from\_amount + fee\_amount - bonus\_amount)     |
| exchange\_rate                                | 20         | Exchange rate including bonus FX (USD 1=GHS 20) |
| Total amount received by the receiver         | GHS 2000   | from\_amount\*exchange\_rate                    |

**Sample Request**

```
curl --location --request POST '{{url}}/users/{{user_id}}/transactions' \
--header 'X-Client-Id: client_id' \
--header 'X-Client-Secret: client_secret' \
--header 'Content-Type: application/json' \
--data-raw '{
  "sender_amount":100,
  "exchange_rate":20,
  "fee_amount": 2,
  "bonus_amount":5,
  "sender_funding_account_id": "{{accountId}}",
  "recipient_id": "{{recipientId}}",
    "recipient_account_id":"{{recipientBankId}}",
  "recipient_currency": "GHS",
  "remittance_purpose": "House Purchase",
  "ip_address": "10.10.10.5",
  "funding_source" : "BANK",
  "payout_method": "BANK_DEPOSIT"
  }
'
```
{% endtab %}
{% endtabs %}

## Implementation 4: Waive fees of a transaction <a href="#implementation-4-waive-fees-of-a-transaction" id="implementation-4-waive-fees-of-a-transaction"></a>

As a marketing offer, you can also waive fees fully or partially for a user’s transaction. In order to do so, you will have to provide "bonus\_amount" equal to the total fees or equal to the partial discount you are providing on the fees.

{% tabs %}
{% tab title="Example 1" %}
**Waive partial fees for a transaction**

Amount entered by sender: USD 100

Fees: USD 5

Discount on fees provided by the Client to the user: USD 2

| Field                                         | Amount ($) | Description                                          |
| --------------------------------------------- | ---------- | ---------------------------------------------------- |
| from\_amount                                  | USD 100    | Amount entered by sender                             |
| fee\_amount                                   | USD 3      | Fee to be added after discount on fees (USD 5-USD 2) |
| bonus\_amount                                 | 0          | Bonus provided to the receiver                       |
| Total amount deducted from sender’s account\* | USD 103    | from\_amount + fee\_amount - bonus\_amount           |
| exchange\_rate                                | 14         | Exchange rate including bonus FX (USD 1=GHS 14)      |
| Total amount received by the receiver         | GHS 1400   | from\_amount\*exchange\_rate                         |

**Sample Request**

```
curl --location --request POST '{{url}}/users/{{user_id}}/transactions' \
--header 'X-Client-Id: client_id' \
--header 'X-Client-Secret: client_secret' \
--header 'Content-Type: application/json' \
--data-raw '{
  "sender_amount":100,
  "exchange_rate":14,
  "fee_amount": 3,
  "sender_funding_account_id": "{{accountId}}",
  "recipient_id": "{{recipientId}}",
  "recipient_account_id":"{{recipientBankId}}",
  "recipient_currency": "GHS",
  "remittance_purpose": "House Purchase",
  "ip_address": "10.10.10.5",
  "funding_source" : "BANK",
  "payout_method": "BANK_DEPOSIT"
  }'
```
{% endtab %}

{% tab title="Example 2" %}
**Waive full fees for a transaction**

Amount entered by sender: USD 100

Fees: USD 5

Discount on fees provided by the Client to the user: USD 5

| **Field**                                     | **Amount** | **Description**                                      |
| --------------------------------------------- | ---------- | ---------------------------------------------------- |
| from\_amount                                  | USD 100    | Amount entered by sender                             |
| fee\_amount                                   | USD 0      | Fee to be added after discount on fees (USD 5-USD 5) |
| bonus\_amount                                 | 0          | Bonus provided to the receiver                       |
| Total amount deducted from sender’s account\* | USD 100    | from\_amount + fee\_amount - bonus\_amount           |
| exchange\_rate                                | 14         | Exchange rate including bonus FX (USD 1=GHS 14)      |
| Total amount received by the receiver         | GHS 1400   | from\_amount\*exchange\_rate                         |

**Sample Request**

Copy

```
curl --location --request POST '{{url}}/users/{{user_id}}/transactions' \
--header 'X-Client-Id: client_id' \
--header 'X-Client-Secret: client_secret' \
--header 'Content-Type: application/json' \
--data-raw '{
  "sender_amount":100,
  "exchange_rate":14,
  "fee_amount": 0,
  "sender_funding_account_id": "{{accountId}}",
  "recipient_id": "{{recipientId}}",
  "recipient_account_id":"{{recipientBankId}}",
  "recipient_currency": "GHS",
  "remittance_purpose": "House Purchase",
  "ip_address": "10.10.10.5",
  "funding_source" : "BANK",
  "payout_method": "BANK_DEPOSIT"
  }
'
```
{% endtab %}
{% endtabs %}
