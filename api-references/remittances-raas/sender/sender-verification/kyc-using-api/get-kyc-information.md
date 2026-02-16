---
description: Read on obtaining verification information of senders
---

# Get KYC Information

{% hint style="info" %}
This API is only applicable if you are using API for KYC.
{% endhint %}

The API provides the sender's KYC information.

**`GET /v2/senders/{{senderId}}/kyc-info`**

{% tabs %}
{% tab title="Details" %}
| Name                             | Required | Type    | Description                                                                                                                                                                                                                                                                   |
| -------------------------------- | -------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| senderId                         | Yes      | UUID    | Sender ID                                                                                                                                                                                                                                                                     |
| first\_name                      | Response | String  | First name of the sender                                                                                                                                                                                                                                                      |
| middle\_name                     | Response | String  | Middle name of the sender                                                                                                                                                                                                                                                     |
| last\_name                       | Response | String  | Last name of the sender                                                                                                                                                                                                                                                       |
| gender                           | Response | String  | Gender of the sender                                                                                                                                                                                                                                                          |
| email                            | Response | String  | Email address of the sender.                                                                                                                                                                                                                                                  |
| mobile\_phone                    | Response | String  | 10-15 digit mobile phone number of sender                                                                                                                                                                                                                                     |
| date\_of\_birth                  | Response | String  | Sender's date of birth (yyyy-mm-dd)                                                                                                                                                                                                                                           |
| address\_line1                   | Response | String  | Sender's address line 1                                                                                                                                                                                                                                                       |
| address\_line2                   | Response | String  | Sender's address line 2                                                                                                                                                                                                                                                       |
| state                            | Response | String  | 2-letter ISO code of the sender's state                                                                                                                                                                                                                                       |
| city                             | Response | String  | Sender's city                                                                                                                                                                                                                                                                 |
| zipcode                          | Response | String  | Sender's postal code                                                                                                                                                                                                                                                          |
| kyc\_status                      | Response | Enum    | KYC status of sender. `UNVERIFIED`,`IN_PROGRESS`, `REVIEW_PENDING`, `RETRY`, `VERIFIED`, `RETRY_REQUESTED`,`DOCUMENT_REQUESTED`,`SUSPENDED`,`DOCUMENT` Details on statuses are [here](https://raas.docs.machnetinc.com/api-references/sender/sender-verification/kyc-status). |
| document\_status                 | Response | Enum    | <p>[Will be deprecated soon]</p><p>Document status of sender. <code>APPROVED</code>, <code>RETRY</code>, <code>PENDING</code>, <code>FAILED</code>, <code>MANUAL_VERIFICATION_REQUIRED</code>, <code>NONE</code>,<code>MANUALLY_VERIFIED</code>,<code>CANCELED</code></p>     |
| document                         | Response | Object  | Details of the most recently uploaded ID document of the sender                                                                                                                                                                                                               |
| document.expiry\_date            | Response | String  | ID document expiry date (yyyy-mm-dd)                                                                                                                                                                                                                                          |
| document.status                  | Response | Enum    | Verification status of the sender's ID document: `APPROVED`, `RETRY`, `PENDING`, `FAILED`, `MANUAL_VERIFICATION_REQUIRED`, `NONE`,`MANUALLY_VERIFIED`,`CANCELED`                                                                                                              |
| document.document\_type          | Response | Enum    | Sender's ID document type: `PASSPORT`,`DRIVING_LICENCE`,`STATE_ID`                                                                                                                                                                                                            |
| document.expiry\_status          | Response | Enum    | Expiry status of sender's ID document. `UNEXPIRED`,`EXPIRING_SOON`, `EXPIRED`                                                                                                                                                                                                 |
| sender\_company                  | Response | Object  | Details of sender's company/employer                                                                                                                                                                                                                                          |
| sender\_company.occupation       | Response | String  | Sender's occupation                                                                                                                                                                                                                                                           |
| sender\_company.company\_name    | Response | String  | Sender's company name                                                                                                                                                                                                                                                         |
| sender\_company.company\_address | Response | String  | Address of sender's company                                                                                                                                                                                                                                                   |
| sender\_company.company\_phone   | Response | String  | Phone number of sender's company                                                                                                                                                                                                                                              |
| user\_agreement                  | Response | Boolean | True if sender has signed sender remit agreement                                                                                                                                                                                                                              |


{% endtab %}

{% tab title="Request Sample" %}
```
curl --location --request GET {{url}}/v2/senders/{{senderId}}/kyc-info' \
--header 'X-Client-Id: clientId' \
--header 'X-Client-Secret: clinetSecret' \
--header 'Content-Type: application/json' \
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
    "id": "senderId",
    "first_name": "John",
    "middle_name": "",
    "last_name": "Doe",
    "email": "user@sandbox.com",
    "gender": "female",
    "mobile_phone": "1234567890",
    "date_of_birth": "1974-06-17",
    "address_line1": "123 California Avenue",
    "city": "Santa Monica",
    "state": "CA",
    "country": "USA",
    "zipcode": "90403",
    "nationality": "ARM",
    "sender_company": {
        "occupation": "Engineer"
    },
    "document": {
      "expiry_date": "2022-06-17",
      "status": "RETRY",
      "expiry_status": "EXPIRED",
      "document_type": "PASSPORT"
    },
    "kyc_status": "DOCUMENT_REQUESTED",
    "document_status": "NONE",
    "user_agreement": true
}
```
{% endtab %}
{% endtabs %}
