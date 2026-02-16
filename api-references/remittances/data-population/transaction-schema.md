# Transaction schema

Each country destination requires a set of different information for the additional data in transaction, and that schema is described by

| Parameter              | Type   | Required | Description                                                            |
| ---------------------- | ------ | -------- | ---------------------------------------------------------------------- |
| tenant                 | string | Yes      | The tenant identifier for your organization.                           |
| destinationCountryIso2 | string | Yes      | ISO 3166-1 alpha-2 code of the destination country (e.g., US, BR, MX). |

### Sample Request

{% tabs %}
{% tab title="Request" %}
```
curl --request GET \
  --url '{api_endpoint}/organizations/{tenant}/fx/transactions/schema?countryCode={destinationCountryIso2}' \
  --header 'content-type: application/json' \
  --header 'x-agent-api-key: {agentApiKey}' \
  --header 'x-agent-id: {agentId}'
```
{% endtab %}

{% tab title="Response" %}
```
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Inyo Global Payout Additional Data - Morocco",
  "version": "1.0.0",
  "type": "object",
  "properties": {
    "additionalData": {
      "type": "object",
      "properties": {
        "statementNarrative": {
          "type": "string",
          "description": "Optional narrative text providing transaction details."
        }
      }
    }
  },
  "required": [
    "additionalData"
  ]
}
```
{% endtab %}
{% endtabs %}

[Documentation](https://dev-api.inyoglobal.com/sandbox/#tag/fx_Transaction-Resource/paths/~1organizations~1%7Btenant%7D~1fx~1transactions~1schema/get)

```
GET /organizations/{tenant}/fx/transactions/schema?countryCode={destinationCountryIso2}
```
