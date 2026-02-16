---
description: Read on retrieving enabled states
---

# State

The state API provides a list of all states in the origination country which is enabled for you.

{% tabs %}
{% tab title="Details" %}
| **Field**   | **Type** | **Description**                   |
| ----------- | -------- | --------------------------------- |
| id          | Integer  | ID of the state                   |
| name        | String   | Full name of the state            |
| code        | String   | 2-character ISO code of the state |
| country\_id | String   | ID of the country                 |


{% endtab %}
{% endtabs %}

## **Get a list of all available states** <a href="#get-a-list-of-all-available-states" id="get-a-list-of-all-available-states"></a>

**`GET /v2/states`**

{% tabs %}
{% tab title="Request Sample" %}
```
curl -X GET '{{url}}/v2/states' \
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
      "name": "California",
      "code": "CA",
      "country_id": 23,
    }
  ]
}
```
{% endtab %}
{% endtabs %}

## **Get state by ID** <a href="#get-state-by-id" id="get-state-by-id"></a>

**`GET /v2/states/{{stateId}}`**

{% tabs %}
{% tab title="Parameters" %}
| **Field** | **Type** | **Description** |
| --------- | -------- | --------------- |
| stateId   | Integer  | ID of the state |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X GET '{{url}}/v2/countries/states/{{stateId}} \
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
      "name": "California",
      "code": "CA",
      "country_id": 23,
    }
  ]
}
```
{% endtab %}
{% endtabs %}
