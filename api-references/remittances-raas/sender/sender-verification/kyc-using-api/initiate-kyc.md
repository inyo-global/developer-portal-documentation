---
description: Read on initiating KYC process for senders
---

# Initiate KYC

{% hint style="info" %}
This API is only applicable if you are using API for KYC.
{% endhint %}

The API enables you to initiate the sender's KYC process. It is required to provide user agreement details and all the required sender information before initiating sender's KYC.

**`PUT /v2/senders/{{senderId}}/initiate-kyc`**

{% tabs %}
{% tab title="Details" %}
| Field            | Required    | Type | Description                                                                                                                                                    |
| ---------------- | ----------- | ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| senderId         | Yes, in URL | UUID | id of the sender                                                                                                                                               |
| sender\_id       | Response    | UUID | Id of the sender                                                                                                                                               |
| kyc\_status      | Response    | Enum | Updated KYC status of sender. `UNVERIFIED`,`IN_PROGRESS`, `REVIEW_PENDING`, `RETRY`, `VERIFIED`, `RETRY_REQUESTED`,`DOCUMENT_REQUESTED`,`SUSPENDED`,`DOCUMENT` |
| document\_status | Response    | Enum | Document status of sender. `APPROVED`, `RETRY`, `PENDING`, `FAILED`, `MANUAL_VERIFICATION_REQUIRED`, `NONE`,`MANUALLY_VERIFIED`,`CANCELED`                     |
{% endtab %}

{% tab title="Request Sample" %}
```
 curl --location --request PUT {{url}}/v2/senders/{{senderId}}/initiate-kyc' \
--header 'Content-Type: application/json' \
--header 'X-Client-Id: clientid ' \
--header 'X-Client-Secret: clientSecret ' \
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
      "senderId": "senderId",
      "kycStatus": "IN_PROGRESS",
      "documentStatus": "PENDING"
  }
```

**If the API is called with insufficient information, it results in error response.**

```
{
    "status": 400,
    "code": "BAD_REQUEST",
    "message": "Some fields are missing for [SENDER_GENDER]"
}
```
{% endtab %}
{% endtabs %}

If KYC status is `RETRY`, you should prompt sender to provide additional information. The additional information required for KYC can be retrieved from [Get KYC Fields API](get-kyc-fields.md).

If KYC status is `REVIEW_PENDING`, the admin user can request sender for additional information such as `senderFullSSN`,`senderProofOfAddress`, and `senderSourceOfFund` from dashboard. The additional information requested by admin user can be retrieved from [Get KYC Fields API](get-kyc-fields.md).
