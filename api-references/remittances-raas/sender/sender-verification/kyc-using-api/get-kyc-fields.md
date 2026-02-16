---
description: Read on understanding required KYC information of senders
---

# Get KYC fields

{% hint style="info" %}
This API is only applicable if you are using API for KYC.
{% endhint %}

This provides the current KYC status of sender and the list of KYC fields which represents sender information required for KYC verification in current tier level. You should fetch the KYC fields and update the sender information as required by the tier level.

**`GET /v2/senders/{senderId}/kyc-fields`**

{% tabs %}
{% tab title="Details" %}
| Field                | Description                                                                                                                                                                                                                           |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| senderFullName       | Sender's full name. It includes `First Name`, `Middle Name` and `Last Name`                                                                                                                                                           |
| senderEmail          | Sender's email address                                                                                                                                                                                                                |
| senderGender         | Sender's gender                                                                                                                                                                                                                       |
| senderDOB            | Sender's date of birth. It includes `Year`, `Month` and `Day`                                                                                                                                                                         |
| senderPhone          | Sender's phone number                                                                                                                                                                                                                 |
| senderAddress        | Sender Address. It includes `AddressLine1`, `AddressLine2`, `City`, `State` and `Zipcode`                                                                                                                                             |
| senderFourDigitSSN   | Sender four digit SSN. Sender four digit SSN should be provided via widget v2.0                                                                                                                                                       |
| senderFullSSN        | Sender full nine digit SSN. Sender full SSN should be provided via widget v2.0                                                                                                                                                        |
| senderIdInfo         | Sender identification document. Sender identification document should be provided via widget v2.0                                                                                                                                     |
| senderCompanyDetails | Sender company details. It includes `Company Name`, `Company Address`, `Company Phone Number`                                                                                                                                         |
| senderOccupation     | Sender occupation details.                                                                                                                                                                                                            |
| senderProofOfAddress | Document that specifies sender current address. Types of sender proof of address document : `Utility Bill`, `Bank or Credit Card Statement`, `Driver License`, `Council Tax Bill`,`Vehicle Registration or Tax` and `Lease Agreement` |
| senderSourceOfFund   | Document that specifies sender source of fund. Types of sender source of fund document : `Bank Statement` and `Pay Slip`                                                                                                              |
| senderRemitAgreement | Sender remit agreement needs to be displayed to and agreed by the sender. The sender remit agreement value should be always true for KYC process to be completed.                                                                     |


{% endtab %}

{% tab title="Request Sample" %}
```
curl -X GET {{url}}/v2/senders/{{senderId}}/kyc-fields \
   -H 'Accept: application/json' \
   -H 'X-Client-Id: clientid' \
   -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
       "sender_id" : "sender_UUID",
       "kyc_status" : "KYC_STATUS",
       "metadata" : [
           {
               "level" : "LEVEL1",
               "fields" : [
                   "senderDOB",
                   "senderFourDigitSSN",
                   "senderPhone",
                   "senderEmail",
                   "senderAddress",
                   "senderRemitAgreement",
                   "senderFullName",
                   "senderGender"
               ]
           }
       ]
   }
```
{% endtab %}
{% endtabs %}

