---
description: Add a document for a recipient
---

# Recipient Document

Recipient document may be required based on your spec sheet.

{% tabs %}
{% tab title="Details" %}
| **Field**        | **Type** | **Description**                 |
| ---------------- | -------- | ------------------------------- |
| documentType     | String   | Type of the recipient's documen |
| uploadedFile     | String   | File in png/ pdf/ jpeg/ jpg     |
| documentIdNumber | String   | Document identification number  |
{% endtab %}
{% endtabs %}

## Add a Recipient Document <a href="#add-a-recipient-document" id="add-a-recipient-document"></a>

You can use this API to add a recipient document.

**`POST /v2/senders/{{senderId}}/recipients/{{recipientId}}/documents`**

{% tabs %}
{% tab title="Details" %}
| Field            | Required | Type   | Description                      |
| ---------------- | -------- | ------ | -------------------------------- |
| documentType     | No       | String | Type of the recipient's document |
| uploadedFile     | Yes      | String | File in png/ pdf/ jpeg/ jpg      |
| documentIdNumber | No       | String | Document identification number   |
{% endtab %}

{% tab title="Request Sample" %}
```
curl --location --request POST '{{URL}}/v2/senders/{{senderId}}/recipients/{{recipientId}}/documents' \
--header 'X-Client-Id: clientId \
--header 'X-Client-Secret: clientsecret\
--form 'documentType=receiver passport' \
--form 'uploadedFile=@/home/Pictures/test.png' \
--form 'documentIdNumber=123456'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
   "test_field": "test_value",
   "id": "26a8aede-fbdb-41c6-a332-ab8b2822348a",
   "documentType": "receiver pasport",
   "documentIdNumber": "123456",
   "uploadedDate": "2023-05-02"
}
```
{% endtab %}
{% endtabs %}

#### Get Recipient Document <a href="#get-recipient-document" id="get-recipient-document"></a>

You can view the recent document submitted for the recipient using this document.

**`GET /v2/senders/{{senderId}}/recipients/{{recipientId}}/documents`**

{% tabs %}
{% tab title="Request Sample" %}
```
curl --location --request GET '{{URL}}/v2/senders/{{senderId}}/recipients/{{recipientId}}/documents' \
--header 'X-Client-Id: clientId' \
--header 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
   "test_field": "test_value",
   "results": [
       {
           "id": "26a8aede-fbdb-41c6-a332-ab8b2822348a",
           "fileName": "RECEIVER_IDENTITY_2023-05-02T02%3A46%3A43.475.png",
           "frontImageLink": LINK,
           "documentType": "receiver passport",
           "documentIdNumber": "1221424242",
           "uploadedDate": "2023-05-02"
       }
   ],
   "paging": {
       "total_count": 1
   }
}
```
{% endtab %}
{% endtabs %}
