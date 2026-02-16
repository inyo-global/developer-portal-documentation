---
description: Read on updating sender's information
---

# Update KYC Information

{% hint style="info" %}
This API is only applicable if you are using API for KYC.
{% endhint %}

The API allows you to update the sender information. You can provide all the required sender information at once. You can also update the sender information partially by using this API multiple times as required.

You can update sender when the user is in the following KYC status:

* UNVERIFIED
* REVIEW\_PENDING
* RETRY
* RETRY\_REQUESTED
* DOCUMENT\_REQUESTED
* VERIFIED \[Can only can update Nationality and Occupation fields if they are null]

**`PATCH /v2/senders/{{senderId}}/kyc-info`**

{% tabs %}
{% tab title="Details" %}
| Field                            | Type    | Description                                                                                                                                            |
| -------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| id                               | UUID    | Id of Sender                                                                                                                                           |
| first\_name                      | String  | First name of the sender                                                                                                                               |
| middle\_name                     | String  | Middle name of the sender                                                                                                                              |
| last\_name                       | String  | Last name of the sender                                                                                                                                |
| gender                           | String  | Gender of the sender                                                                                                                                   |
| email                            | String  | Email address of the sender                                                                                                                            |
| mobile\_phone                    | String  | 10-15 digit mobile phone number of the sender                                                                                                          |
| date\_of\_birth                  | String  | Sender's date of birth (yyyy-mm-dd)                                                                                                                    |
| address\_line1                   | String  | Sender's address line 1                                                                                                                                |
| address\_line2                   | String  | Sender's address line 2                                                                                                                                |
| state                            | String  | 2-letter ISO code of the sender's state                                                                                                                |
| city                             | String  | Sender's city                                                                                                                                          |
| zipcode                          | String  | Sender's zipcode                                                                                                                                       |
| nationality                      | String  | 3-letter ISO code of sender's nationality                                                                                                              |
| sender\_company                  | Object  | Sender's company details                                                                                                                               |
| sender\_company.occupation       | String  | Sender's occupation                                                                                                                                    |
| sender\_company.company\_name    | String  | Sender's company name                                                                                                                                  |
| sender\_company.company\_address | String  | Address of sender's company                                                                                                                            |
| sender\_company.company\_phone   | String  | Phone number of sender's company                                                                                                                       |
| kyc\_status                      | Enum    | KYC status of sender. `UNVERIFIED`,`IN_PROGRESS`, `REVIEW_PENDING`, `RETRY`, `VERIFIED`, `RETRY_REQUESTED`,`DOCUMENT_REQUESTED`,`SUSPENDED`,`DOCUMENT` |
| user\_agreement                  | boolean | True if sender remit agreement has been signed                                                                                                         |
{% endtab %}

{% tab title="Request Sample" %}
```
curl --location --request PATCH {{url}}/v2/senders/{{senderId}}/kyc-info' \
--header 'Content-Type: application/json' \
--header 'X-Client-Id: clientid ' \
--header 'X-Client-Secret: clientSecret ' \
--data-raw '{
             "first_name": "John",
             "middle_name" : "",
             "last_name": "Doe",
             "mobile_phone": "1234567890",
             "email": "user@sandbox.com",
             "gender": "male",
             "date_of_birth": "1974-06-17",
             "address_line1": "123 California Avenue",
             "address_line2": "",
             "city": "Santa Monica",
             "state" : "CA",
             "zipcode": "90403",
             "nationality": "USA",
             "user_agreement" : true,
             "sender_company": {
                     "occupation": "Engineer"
                 }
             }'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
      "id": "senderId",
      "first_name": "John",
      "middle_name" : "Mid",
      "last_name": "Doe",
      "mobile_phone": "1234567890",
      "email": "user@sandbox.com",
      "gender": "male",
      "date_of_birth": "1974-06-17",
      "address_line1": "123 California Avenue",
      "address_line2": "",
      "city": "Santa Monica",
      "state" : "CA",
      "zipcode": "90403",
      "nationality": "USA",
      "kyc_status": "UNVERIFIED",
      "user_agreement": true
   }
```
{% endtab %}
{% endtabs %}
