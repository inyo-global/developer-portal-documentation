# Payout countries enabled

The Payout Destinations endpoint returns the list of countries available to receive payouts from a specified source country.

This is typically used to determine which destination countries are supported for a payout corridor (that are active for your installation) before proceeding with quote or transaction creation.



| Parameter         | Type   | Required | Description                                                       |
| ----------------- | ------ | -------- | ----------------------------------------------------------------- |
| tenant            | string | Yes      | The tenant identifier for your organization.                      |
| sourceCountryIso2 | string | Yes      | ISO 3166-1 alpha-2 code of the source country (e.g., US, BR, MX). |

### Sample Request

{% tabs %}
{% tab title="Request" %}
```
curl --request GET \
  --url {api_endpoint}/organizations/{tenant}/payout/{sourceCountryIso2}/destinations \
  --header 'content-type: application/json' \
  --header 'x-agent-api-key: {agentApiKey}' \
  --header 'x-agent-id: {agentId}'
```
{% endtab %}

{% tab title="Response" %}
```
{
   "countryDestinations": [
      {
          "country": "BR",
          "countryName": "Brazil",
          "currency": "BRL",
          "currencyName": "Brazilian real",
          "currencySymbol": "R$",
          "emoji": ""
       },
      {
          "country": "DO",
          "countryName": "Dominican Republic",
          "currency": "DOP",
          "currencyName": "Dominican peso",
          "currencySymbol": "RD$",
          "emoji": ""
      }
    ]
}
```
{% endtab %}
{% endtabs %}

### Response Details

* country — ISO 3166-1 alpha-2 code of the destination country.
* countryName — Full name of the destination country.
* currency — ISO 4217 code of the destination currency.
* currencyName — Full name of the currency.
* currencySymbol — Symbol for the destination currency (if available).
* emoji — (Optional) Flag emoji representation.

***

### Usage Notes

* Use this endpoint to populate dropdown lists or validate corridors when the user selects a source country.
* Once a valid source–destination pair is chosen, you can proceed to request quotes and create payouts.



[Documentation](https://dev-api.inyoglobal.com/sandbox/#tag/payout_Payout-Resource/paths/~1organizations~1%7Btenant%7D~1payout~1%7BsourceCountryIso2%7D~1destinations/get)

```
GET /organizations/{tenant}/payout/{sourceCountryIso2}/destinations
```

