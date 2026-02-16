---
description: Read on updating recipient details
---

# Update Recipient

You can update the recipient details using this API. You can only update recipients if a transaction has not been forwarded for delivery to a recipient.

**`PUT /v2/senders/{{senderId}}/recipients/{{recipientId}}`**

{% tabs %}
{% tab title="Details" %}
| Parameter            | Updatable | Remarks                                                    |
| -------------------- | --------- | ---------------------------------------------------------- |
| first\_name          | Yes       |                                                            |
| middle\_name         | Yes       |                                                            |
| last\_name           | Yes       |                                                            |
| country              | No        |                                                            |
| address\_line1       | Yes       |                                                            |
| address\_line2       | Yes       |                                                            |
| city                 | Yes       | City is mandatory while updating any Address fields        |
| postal\_code         | Yes       | Postal code is mandatory while updating any Address fields |
| province             | Yes       | Province is mandatory while updating any Address fields    |
| email                | Yes       |                                                            |
| home\_phone          | Yes       |                                                            |
| mobile\_phone        | Yes       |                                                            |
| work\_phone          | Yes       |                                                            |
| date\_of\_birth      | Yes       |                                                            |
| occupation           | Yes       |                                                            |
| sender\_relationship | No        |                                                            |
| enable\_cash\_pickup | Yes       |                                                            |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X PUT {{url}}/v2/senders/{{senderId}}/recipients/{{recipientId}} \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret' \
  -d { \
    "first_name": "string", \
    "middle_name": "string", \
    "last_name": "string", \
    "country": "string", \
    "address_line1": "string", \
    "address_line2": "string", \
    "city": "string", \
    "postal_code": "string", \
    "province": "string", \
    "email": "string", \
    "home_phone": "string", \
    "mobile_phone": "string", \
    "work_phone": "string", \
    "date_of_birth": "string", \
    "occupation": "string", \
    "enable_cash_pickup": true, \
    "sender_relationship": "string"
  }
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
  "id": "UUID",
  "sender_id": "string",
  "first_name": "string",
  "middle_name": "string",
  "last_name": "string",
  "country": "string",
  "address_line1": "string",
  "address_line2": "string",
  "city": "string",
  "postal_code": "string",
  "province": "string",
  "email": "string",
  "home_phone": "string",
  "mobile_phone": "string",
  "work_phone": "string",
  "date_of_birth": "string",
  "occupation": "string",
  "sender_relationship": "string",
  "status": "string",
  "available_payout_methods": [
          "BANK_DEPOSIT",
          "CASH_PICKUP",
          "WALLET",
          "HOME_DELIVERY"
      ]
}
```
{% endtab %}
{% endtabs %}
