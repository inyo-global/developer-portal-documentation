# Recipient account schema

For a specific payout method, the schema can vary and the amount of information too.

| Parameter              | Type   | Required | Description                                                            |
| ---------------------- | ------ | -------- | ---------------------------------------------------------------------- |
| tenant                 | string | Yes      | The tenant identifier for your organization.                           |
| destinationCountryIso2 | string | Yes      | ISO 3166-1 alpha-2 code of the destination country (e.g., US, BR, MX). |

### Sample Request

{% tabs %}
{% tab title="Request" %}
```
curl --request GET \
  --url {api_endpoint}/organizations/{tenant}/payout/recipientAccounts/schema/{destinationCountryIso2} \
  --header 'content-type: application/json' \
  --header 'x-agent-api-key: {agentApiKey}' \
  --header 'x-agent-id: {agentId}'
```
{% endtab %}

{% tab title="Response" %}
```
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Inyo Global Recipient Account Data - Colombia",
  "description": "Schema for a COL account.",
  "version": "1.0.0",
  "type": "object",
  "required": [
    "asset",
    "payoutMethod"
  ],
  "properties": {
    "externalId": {
      "type": "string",
      "description": "An unique identifier from your system."
    },
    "asset": {
      "type": "string",
      "description": "The currency code that identifies the recipient account's currency in the 'ISO 4217' format.",
      "enum": [
        "COP"
      ]
    },
    "payoutMethod": {
      "type": "object",
      "description": "Details of the payout method.",
      "required": [
        "countryCode",
        "bankCode",
        "routingNumber",
        "accountNumber",
        "accountType"
      ],
      "properties": {
        "type": {
          "type": "string",
          "description": "The type of payout method.",
          "enum": [
            "BANK_DEPOSIT"
          ]
        },
        "countryCode": {
          "type": "string",
          "description": "The country code of the bank account.",
          "enum": [
            "CO"
          ]
        },
        "bankCode": {
          "type": "string",
          "description": "The code of the bank.",
          "minLength": 1
        },
        "routingNumber": {
          "type": "string",
          "description": "The routing number for the bank (e.g., ABA for USA, SWIFT/BIC for international).",
          "minLength": 3
        },
        "accountNumber": {
          "type": "string",
          "description": "The bank account number.",
          "minLength": 1
        },
        "accountType": {
          "type": "string",
          "description": "The type of bank account.",
          "enum": [
            "CHECKING",
            "SAVINGS"
          ]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

***

[Documentation](https://dev-api.inyoglobal.com/sandbox/#tag/payout_Payout-Resource/paths/~1organizations~1%7Btenant%7D~1payout~1recipientAccounts~1schema~1%7BcountryIso2%7D/get)

```
GET /organizations/{tenant}/payout/recipientAccounts/schema/{destinationCountryIso2}
```

