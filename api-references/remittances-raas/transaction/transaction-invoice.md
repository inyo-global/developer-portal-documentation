---
description: Details on how to obtain transaction invoice
---

# Transaction Invoice

This API is used to retrieve or download Invoice once a transaction has been successfully created in Machnet system.

**`GET {{url}}/senders/{{senderId}}/transactions/{{transactionId}}/invoice?type={{type}}`**

{% hint style="info" %}
You can either request for a FILE or LINK by specifying `type` in the URL. The file and link can be used to download the invoice. Please note that the link will only be valid for 60 mins in the sandbox/production environment after which the API needs to be called once again for the new link.
{% endhint %}

{% tabs %}
{% tab title="Request Sample" %}
```
curl -X GET {{url}}/senders/{{senderId}}/transactions/{{transactionId}}/invoice?type=FILE \
--header 'X-Client-Id: client_id' \
--header 'X-Client-Secret: client_secret' \
--header 'Content-Type: application/json' \
--data '{
}'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
  "id": UUID,
  "file_name": "invoice.pdf",
  "type": "INVOICE"
}
```
{% endtab %}
{% endtabs %}

API can be used to either directly download the file or generate a link to the file. The downloaded file will be of `Content-Type: application/octet-stream`.
