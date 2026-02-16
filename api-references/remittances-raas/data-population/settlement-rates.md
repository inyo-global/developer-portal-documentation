---
description: Details on obtaining current indicative settlement rates
---

# Settlement Rates

Details on obtaining current indicative settlement rates

You can receive settlement rates applied during payout using this API. Please note that these rates are indicative and actual settlement rates may vary based on settlement time. Based on these rates, you can set you own exchange rate and provide them per transaction during transaction creation. The exchange rate provided during transaction creation is the customer exchange rate and can vary from the settlement rate.

**`GET /v2/settlement-rates/`**

{% hint style="info" %}
You can use the following filter parameters to filter out responses are per your requirement.

1. GET \{{url\}}/settlement-rates/?payout\_method=\{{payout\_method\}}
2. GET \{{url\}}/settlement-rates/?destination\_currency=\{{destination\_currency\}}
3. GET \{{url\}}/settlement-rates/?destination\_country\_code=\{{destination\_country.code\}}
4. GET \{{url\}}/settlement-rates/?payout\_method=\{{payout\_method\}}\&destination\_country\_code=\{{destination\_country.code\}}
{% endhint %}

{% tabs %}
{% tab title="Parameters" %}
| Field                     | Type   | Description                                                 |
| ------------------------- | ------ | ----------------------------------------------------------- |
| source\_country           | Object | Source country                                              |
| source\_country.name      | String | Name of source country                                      |
| source\_country.code      | String | 2-character ISO code of the source country                  |
| source\_currency          | String | 3-character ISO code of the country                         |
| destination\_country      | String | Destination country                                         |
| destination\_country.name | String | Name of destination country                                 |
| destination\_country.code | String | 2-character ISO code of the destination country             |
| destination\_currency     | String | 3-character ISO code of the country                         |
| payout\_method            | String | Delivery method for which the settlement rate is applicable |
| settlement\_rate          | Number | Indicative settlement rate in destination currency          |
{% endtab %}

{% tab title="Request Sample" %}
```
curl --location GET '{{url}}/v2/settlement-rates/' \
--header 'X-Client-Id: clientid’\
--header 'X-Client-Secret: clientsecret’\
--header 'Content-Type: application/json'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
    "rates": [
{
            "source_currency": "USD",
            "destination_currency": "USD",
            "payout_method": "BANK_DEPOSIT",
            "settlement_rate": 1.0000,
            "source_country": {
                "name": "United States",
                "code": "US"
            },
            "destination_country": {
                "name": "Nigeria",
                "code": "NG"
            }
        },
       {
            "source_currency": "USD",
            "destination_currency": "GTQ",
            "payout_method": "WALLET",
            "settlement_rate": 7.8060,
            "source_country": {
                "name": "United States",
                "code": "US"
            },
            "destination_country": {
                "name": "Guatemala",
                "code": "GT"
            }
        },
        {
            "source_currency": "USD",
            "destination_currency": "GTQ",
            "payout_method": "BANK_DEPOSIT",
            "settlement_rate": 7.7291,
            "source_country": {
                "name": "United States",
                "code": "US"
            },
            "destination_country": {
                "name": "Guatemala",
                "code": "GT"
            }
        }      
           ]
}
```
{% endtab %}
{% endtabs %}
