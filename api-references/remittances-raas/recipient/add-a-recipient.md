---
description: Read on how to add a receive user
---

# Add a Recipient

This API allows you to add a Recipient for given Sender. You will need to provide recipient’s basic information via this API. The response contains a unique Recipient ID that you can store on your end.

The field `available_payout_methods` shows the list of payout method that are available from the recipient while creating a transaction. For a recipient to be eligible for `BANK_DEPOSIT` or`CASH_PICKUP` or `WALLET` or `HOME_DELIVERY`, `available_payout_methods` must contain the payout method.

1. `BANK_DEPOSIT` is enabled by default for all receivers and they can receive the amount if their bank information is added.
2. For a recipient to be eligible for `CASH_PICKUP` the field `enable_cash_pickup` must be set to `true` while creating or updating the recipient. If the `enable_cash_pickup` is set to `true`, additional information about the receiver which were optional may be required based on the receiver's country.
3. If you have been approved for Mobile Wallet,`WALLET` is enabled for all the applicable receivers.
4. If you have been approved for Home Delivery, `HOME_DELIVERY` is enabled for all the receivers.

**`POST /v2/senders/{{senderId}}/recipients`**

{% tabs %}
{% tab title="Parameters" %}
| Parameter            | Required    | Type    | Description                                                                                                                                                                                                                                                                                                                               |
| -------------------- | ----------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| senderId             | Yes, in URL | UUID    | ID of the sender under which the recipient will be added                                                                                                                                                                                                                                                                                  |
| first\_name          | Yes         | String  | First name of the recipient                                                                                                                                                                                                                                                                                                               |
| middle\_name         | No          | String  | Middle name of the recipient                                                                                                                                                                                                                                                                                                              |
| last\_name           | Yes         | String  | Last name of the recipient                                                                                                                                                                                                                                                                                                                |
| country              | Yes         | String  | ISO code of the recipient's country                                                                                                                                                                                                                                                                                                       |
| address\_line1       | Yes         | String  | Street address of the recipient                                                                                                                                                                                                                                                                                                           |
| address\_line2       | No          | String  | Second line of the street address                                                                                                                                                                                                                                                                                                         |
| city                 | Yes         | String  | City of the recipient                                                                                                                                                                                                                                                                                                                     |
| postal\_code         | Yes         | String  | Zip code of the recipient                                                                                                                                                                                                                                                                                                                 |
| province             | Yes         | String  | State or province of the recipient                                                                                                                                                                                                                                                                                                        |
| email                | No          | Email   | The email address of the recipient                                                                                                                                                                                                                                                                                                        |
| home\_phone          | No          | String  | Home phone number of the recipient                                                                                                                                                                                                                                                                                                        |
| mobile\_phone        | No          | String  | Mobile phone number of the recipient                                                                                                                                                                                                                                                                                                      |
| work\_phone          | No          | String  | Work phone number of the recipient                                                                                                                                                                                                                                                                                                        |
| date\_of\_birth      | No          | Date    | Date of birth of the recipient (yyyy-mm-dd)                                                                                                                                                                                                                                                                                               |
| occupation           | No          | String  | Occupation of the recipient                                                                                                                                                                                                                                                                                                               |
| sender\_relationship | Yes         | String  | Enumerated value: `Aunt`, `Brother`, `Brother in Law`, `Cousin`,  `Employee`, `Daughter`, `Father`, `Father in Law`, `Friend`, `Grandfather`, `Grandchild`, `Grandmother`, `Mother`, `Mother in law`, `Nephew`, `Niece`, `Self`, `Sister`, `Sister in Law`, `Son`,   `Spouse`, `Service provider`, `Business associate`, `Uncle`, `Other` |
| enable\_cash\_pickup | No          | boolean | Set the flag to true for enabling `CASH_PICKUP`                                                                                                                                                                                                                                                                                           |


{% endtab %}

{% tab title="Request Sample" %}
```
curl -X POST {{url}}/v2/senders/{{senderId}}/recipients \
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
    "document": "string", \
    "home_phone": "string", \
    "mobile_phone": "string", \
    "work_phone": "string", \
    "date_of_birth": "string", \
    "occupation": "string", \
    "sender_relationship": "string" \
    "enable_cash_pickup": true \
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
  "document": "string",
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
