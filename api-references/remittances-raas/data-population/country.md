# Country

The country API provides a list of all source and destination countries enabled for you.

{% tabs %}
{% tab title="Details" %}
<table data-header-hidden><thead><tr><th></th><th width="131"></th><th></th></tr></thead><tbody><tr><td><strong>Field</strong></td><td><strong>Type</strong></td><td><strong>Description</strong></td></tr><tr><td>id</td><td>Integer</td><td>ID of the Country in Machnet's platform</td></tr><tr><td>name</td><td>String</td><td>Full name of the country</td></tr><tr><td>three_char_code</td><td>String</td><td>The 3-character ISO code of the country</td></tr><tr><td>two_char_code</td><td>String</td><td>The 2-character ISO code of the country</td></tr><tr><td>phone_code</td><td>Integer</td><td>The county code used for international dialing</td></tr><tr><td>currency</td><td>Object</td><td>Details on the currencies supported in the country</td></tr><tr><td>currency.code</td><td>String</td><td>ISO code of the currency</td></tr><tr><td>currency.name</td><td>String</td><td>Name of the currency</td></tr><tr><td>currency.symbol</td><td>String</td><td>Symbol of the currency</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

## **Get a list of all available countries**

**`GET /v2/countries`**

{% tabs %}
{% tab title="Request Sample" %}
```
curl -X GET '{{url}}/v2/countries' \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
  "result": [
    {
      "id": 1,
      "name": "Mexico",
      "three_char_code": "MEX",
      "two_char_code": "MX",
      "phone_code": 123,
      "currency": {
        "code": "MXN",
        "name": "Mexican peso",
        "symbol": "MXN"
          }
    },
    {
      "id": 2,
      "name": "India",
      "three_char_code": "IND",
      "two_char_code": "IN",
      "phone_code": 91,
      "currency": {
        "code": "INR",
        "name": "Indian Rupee",
        "symbol": "I"
          }
    }
  ]
}
```
{% endtab %}
{% endtabs %}

## **Get country by code**

**`GET /v2/countries/?countryCode={{countryCode}}`**

{% tabs %}
{% tab title="Parameters" %}
| **Field**   | **Type** | **Description**       |
| ----------- | -------- | --------------------- |
| countryCode | String   | ISO Code of a Country |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X GET '{{url}}/v2/countries/?countryCode=IND' \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
  "result": [
    {
      "id": 2,
      "name": "India",
      "three_char_code": "IND",
      "two_char_code": "IN",
      "phone_code": 91,
      "currency": {
        "code": "INR",
        "name": "Indian Rupee",
        "symbol": "I"
      }
    }
  ]
}
```
{% endtab %}
{% endtabs %}

## **Get country by country Id** <a href="#get-country-by-country-id" id="get-country-by-country-id"></a>

**`GET /v2/countries/{{countryId}}`**

{% tabs %}
{% tab title="Parameters" %}
| **Field** | **Type** | **Description** |
| --------- | -------- | --------------- |
| countryId | Integer  | Country ID      |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X GET '{{url}}/v2/countries/{{countryId}} \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
  "id": 1,
  "name": "Mexico",
  "three_char_code": "MEX",
  "two_char_code": "MX",
  "phone_code": 123,
  "currency": {
      "code": "MXN",
      "name": "Mexican peso",
      "symbol": "MXN"
      }
}
```
{% endtab %}
{% endtabs %}
