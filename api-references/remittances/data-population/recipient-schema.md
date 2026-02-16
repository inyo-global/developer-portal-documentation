# Recipient schema

Each country requires a set of different information for the recipient, and that schema is described by&#x20;

| Parameter              | Type   | Required | Description                                                            |
| ---------------------- | ------ | -------- | ---------------------------------------------------------------------- |
| tenant                 | string | Yes      | The tenant identifier for your organization.                           |
| destinationCountryIso2 | string | Yes      | ISO 3166-1 alpha-2 code of the destination country (e.g., US, BR, MX). |

### Sample Request

{% tabs %}
{% tab title="Request" %}
```
curl --request GET \
  --url {api_endpoint}/organizations/{tenant}/payout/recipients/schema/{destinationCountryIso2} \
  --header 'content-type: application/json' \
  --header 'x-agent-api-key: {agentApiKey}' \
  --header 'x-agent-id: {agentId}'
```
{% endtab %}

{% tab title="Response" %}
```
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Inyo Global Recipient Data - Colombia",
  "description": "Schema for a COL person.",
  "version": "1.0.0",
  "type": "object",
  "required": [
    "firstName",
    "lastName",
    "address",
    "documents"
  ],
  "properties": {
    "firstName": {
      "type": "string",
      "description": "The first name of the person.",
      "minLength": 1
    },
    "lastName": {
      "type": "string",
      "description": "The last name of the person.",
      "minLength": 1
    },
    "birthDate": {
      "type": "string",
      "description": "The birth date of the person. Formatted as yyyy-MM-dd",
      "pattern": "^\\d{4}-\\d{2}-\\d{2}$"
    },
    "phoneNumber": {
      "type": "string",
      "description": "The contact phone number of the person.",
      "minLength": 1
    },
    "documents": {
      "type": "array",
      "description": "A list of identification documents for the person.",
      "minItems": 1,
      "items": {
        "type": "object",
        "required": [
          "document",
          "type"
        ],
        "properties": {
          "document": {
            "type": "string",
            "description": "The document number."
          },
          "type": {
            "type": "string",
            "description": "The type of document.",
            "enum": [
              "CC"
            ]
          },
          "countryCode": {
            "type": "string",
            "description": "The country code of document.",
            "enum": [
              "CO"
            ]
          }
        },
        "if": {
          "properties": {
            "type": {
              "const": "CC"
            }
          }
        },
        "then": {
          "properties": {
            "document": {
              "description": "Cédula de Ciudadania",
              "pattern": "^(\\d){8,10}"
            }
          }
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

***

[Documentation](https://dev-api.inyoglobal.com/sandbox/#tag/payout_Payout-Resource/paths/~1organizations~1%7Btenant%7D~1payout~1recipients~1schema~1%7BcountryIso2%7D/get)

```
GET /organizations/{tenant}/payout/recipients/schema/{destinationCountryIso2}
```
