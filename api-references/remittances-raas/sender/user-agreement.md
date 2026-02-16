---
description: >-
  Read on collecting user's details during the time the user agrees to the user
  agreement terms
---

# User Agreement

Users need to agree to the respective user agreement in order to be able to use the services provided by the platform. When they agree to the user agreement, we need to capture the user’s details in order to maintain comprehensive records of the agreement. This API allows Clients to capture the required details and provide them to Inyo.

{% hint style="info" %}
User's verification process cannot be initiated until they agree to the user agreement.

If you are conducting KYC using API, you will have to use this API to provide user agreement details before initiating user KYC. The agreement that the user will need to agree to will be shared by the Inyo team.

If you are using KYC using widget, we will collect user agreement details through the widget during the KYC process. However, we are working to remove this feature from the widget after which all user agreement details will have to be submitted through this API. Please contact Inyo team for additional details.
{% endhint %}

**`PATCH /v2/senders/{{senderId}}/user-agreement`**

{% tabs %}
{% tab title="Details" %}
| **Field**             | **Required** | **Type**      | **Description**                                                                                                                                                                                                                                                                                                                                                    |
| --------------------- | ------------ | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| senderId              | Yes          | UUID          | Id of the sender. This is required in the URL.                                                                                                                                                                                                                                                                                                                     |
| user\_agreement       | Yes          | boolean       | TRUE if the user agrees to the user agreement and FALSE if the user does not agree to the user agreement. Details cannot be updated once the user has agreed to the user agreement.                                                                                                                                                                                |
| ip\_address           | Yes          | String        | IP address of the device used by the user.                                                                                                                                                                                                                                                                                                                         |
| user\_agent           | Yes          | String        | <p>Device fingerprint of the device used by the user. e.g</p><p>Web: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36 Mobile App:</p><p>Deviceid: 4BE42863-77A5-4EE6-B08D-CEC25A27F59E,</p><p>appVersion: '1.2.8' ,</p><p>platfom: 'ios',</p><p>buildID: '23B81',</p><p>bundleiD: 'com.abc.pay'</p> |
| agreement\_date\_time | Yes          | LocalDateTime | <p>The timestamp when the user has agreed to the agreement. The format of the timestamp must be</p><p>YYYY-MM-DD HH:MM:SS</p>                                                                                                                                                                                                                                      |
{% endtab %}

{% tab title="Request Sample" %}
```
curl --location --request PATCH {{url}}/v2/senders/{{senderId}}/user-agreement' \
--header 'Content-Type: application/json' \
--header 'X-Client-Id: clientid ' \
--header 'X-Client-Secret: clientSecret ' \
--data-raw '{
"user_agreement": true,
   "ip_address": "String",
   "user_agent": "String",
   "agreement_date_time": "2023-07-06 01:15:45"
}
```
{% endtab %}

{% tab title="Response Sample" %}
```
204
```

If the `user_agreement` has been passed as TRUE earlier, and a request is made to PATCH it as FALSE, the following error occurs.

```
{
"status": 400,
"code": "BAD_REQUEST",
"message": "User has already agreed to the user agreement. It cannot be updated."
}
```
{% endtab %}
{% endtabs %}
