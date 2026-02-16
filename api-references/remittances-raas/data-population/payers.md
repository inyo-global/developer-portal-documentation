---
description: Read on retrieving available payers
---

# Payers

Payers are the entities that deliver the transaction to the recipient in recipient's country. A payer is associated with the destination country and supports one of the following delivery methods:

* CASH\_PICKUP
* HOME\_DELIVERY
* WALLET

The payer id is required for creating transactions with delivery\_method as `CASH_PICKUP` and `HOME_DELIVERY`. Here, the recipient can collect the transferred amount from any of the payer's location in the country when the delivery\_method is `CASH_PICKUP` or `HOME_DELIVERY`.

The payer id is also required when adding recipient's wallet account. The recipient's wallet account needs to be associated with the wallet payers.

{% tabs %}
{% tab title="Details" %}
| Field            | Type   | Description                         |
| ---------------- | ------ | ----------------------------------- |
| id               | long   | ID of the payer                     |
| name             | string | Name of the Payer                   |
| country          | string | 3-character ISO code of the country |
| code             | string | Payer Code                          |
| address          | string | Address of the Payer.               |
| phone\_number    | string | Contact number of the Payer         |
| website          | string | Website of the Payer                |
| network\_limit   | string | Transaction limit of the Payer      |
| delivery\_method | string | Delivery method of the Payer        |
{% endtab %}
{% endtabs %}

## Get All Payers <a href="#get-all-payers" id="get-all-payers"></a>

This API provides list of payers available for you.

**`GET /v2/payers`**

{% tabs %}
{% tab title="Request Sample" %}
```
curl -X GET {{url}}/v2/payers \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
    "results": [
        {
            "id": 1,
            "name": "Acme - REPUBLIC OF ARMENIA",
            "country": "ARM",
            "website": "",
            "delivery_method": "CASH_PICKUP"
        },
        {
            "id": 2,
            "name": "Acme ",
            "country": "ARM",
            "website": "",
            "delivery_method": "HOME_DELIVERY"
        },
        {
            "id": 3,
            "name": "Acme",
            "country": "KEN",
            "website": "",
            "delivery_method": "WALLET"
        }
    ],
    "paging": {
        "total_count": 3
    }
}
```
{% endtab %}
{% endtabs %}

## Get Payers by Country <a href="#get-all-payers-1" id="get-all-payers-1"></a>

This API provides specific payers in a country

**`GET /v2/payers?country={{country}}`**

{% tabs %}
{% tab title="Parameters" %}
| Field   | Required | Type   | Description              |
| ------- | -------- | ------ | ------------------------ |
| country | No       | string | 3 character country code |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X GET {{url}}/v2/payers?country=KEN \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
            "id": 45,
            "name": "Thunes",
            "country": "KEN",
            "delivery_method": "WALLET"
}
```
{% endtab %}
{% endtabs %}

## Get Payers by ID <a href="#get-all-payers-2" id="get-all-payers-2"></a>

This API provides specific payer with the provided ID

**`GET /v2/payers/{{payerId}}`**

{% tabs %}
{% tab title="Parameters" %}
| Field   | Required | Type | Description |
| ------- | -------- | ---- | ----------- |
| payerId | Yes      | long | Payer ID    |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X GET {{url}}/v2/payers/{{payerId}} \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
    "id": 1,
    "name": "Acme - REPUBLIC OF ARMENIA",
    "receiving_currency": "USD",
    "code": "ARM0001",
    "country": "ARM",
    "address": "Payer Address",
    "phone_number": "+123 456789",
    "website": "",
    "network_limit": [],
    "delivery_method": "CASH_PICKUP"
}
```
{% endtab %}
{% endtabs %}
