# Banks in a country

For some country destinations we have a list of the available banks.

| Parameter              | Type   | Required | Description                                                            |
| ---------------------- | ------ | -------- | ---------------------------------------------------------------------- |
| tenant                 | string | Yes      | The tenant identifier for your organization.                           |
| destinationCountryIso2 | string | Yes      | ISO 3166-1 alpha-2 code of the destination country (e.g., US, BR, MX). |

### Sample Request

{% tabs %}
{% tab title="Request" %}
```
curl --request GET \
  --url {api_endpoint}/organizations/{tenant}/payout/{destinationCountryIso2}/banks \
  --header 'content-type: application/json' \
  --header 'x-agent-api-key: {agentApiKey}' \
  --header 'x-agent-id: {agentId}'
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "total": 47,
  "size": 10,
  "page": 0,
  "numberOfPages": 5,
  "items": [
    {
      "code": "007",
      "name": "BANCO BCSC",
      "countryIso2": "CO"
    },
    {
      "code": "1000",
      "name": "BANCO REPÚBLICA",
      "countryIso2": "CO"
    },
    {
      "code": "1001",
      "name": "BANCO DE BOGOTÁ",
      "countryIso2": "CO"
    }
    ...
  ]
}
```
{% endtab %}
{% endtabs %}

[Documentation](https://dev-api.inyoglobal.com/sandbox/#tag/payout_Payout-Resource/paths/~1organizations~1%7Btenant%7D~1payout~1%7BcountryIso2%7D~1banks/get)

```
GET /organizations/{tenant}/payout/{destinationCountryIso2}/banks
```
