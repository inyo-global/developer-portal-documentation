---
description: Read on retrieving available banks
---

# Banks

The bank and branch APIs provide the list of banks and their respective branches that are available in the destination country.

**Bank Object**

{% tabs %}
{% tab title="Details" %}
| Field               | Type    | Description                                                         |
| ------------------- | ------- | ------------------------------------------------------------------- |
| id                  | integer | ID                                                                  |
| name                | string  | Name of the bank                                                    |
| country             | string  | 3-character ISO code of the country                                 |
| receiving\_currency | List    | List of 3-character ISO code of the currency supported by the bank. |
| branches            | List    | List of branch objects.                                             |

**Branch Object**

| Field        | Type    | Description              |
| ------------ | ------- | ------------------------ |
| bank\_id     | integer | Bank ID                  |
| bank\_name   | string  | Name of the bank         |
| branch\_id   | integer | Branch ID                |
| branch\_name | string  | Name of the branch       |
| address      | string  | Branch address           |
| city         | string  | Branch city              |
| state        | string  | Branch state or province |
{% endtab %}
{% endtabs %}

## Get bank list <a href="#get-payer-list" id="get-payer-list"></a>

**`GET /v2/banks`**

{% tabs %}
{% tab title="Request Sample" %}
```
curl -X GET {{url}}/v2/banks \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
  "id": 00,
  "name": "Test Bank",
  "country": "NGA",
  "branches": [
    {
      "id": 00,
      "name": "Test Bank Branch"
    }
  ]
}
```
{% endtab %}
{% endtabs %}

## Get banks by country <a href="#get-payer-list-1" id="get-payer-list-1"></a>

**`GET /v2/banks?countryCode={{countryCode}}`**

{% tabs %}
{% tab title="Parameters" %}
| Field       | Required | Type    | Description           |
| ----------- | -------- | ------- | --------------------- |
| countryId   | No       | integer | Country ID            |
| countryCode | No       | String  | ISO Code of a Country |


{% endtab %}

{% tab title="Request Sample" %}
```
curl -X GET {{url}}/v2/banks?countryCode=KEN \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
  "id": 00,
  "name": "Test Bank",
  "country": "KEN",
  "branches": [
    {
      "id": 00,
      "name": "Test Bank Branch",
    }
  ]
}
```
{% endtab %}
{% endtabs %}

## Get banks by ID <a href="#get-payer-list-2" id="get-payer-list-2"></a>

**`GET /v2/banks/{{bankId}}`**

{% tabs %}
{% tab title="Parameters" %}
| Field  | Required | Type    | Description |
| ------ | -------- | ------- | ----------- |
| bankId | Yes      | integer | Bank ID     |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X GET {{url}}/v2/banks/10 \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
  "id": 10,
  "name": "Test Bank",
  "country": "NGA",
  "receiving_currency" : ["NGN", "USD"],
  "branches": [
    {
      "id": 20,
      "name": "Test Bank Branch"
    }
  ]
}
```
{% endtab %}
{% endtabs %}
