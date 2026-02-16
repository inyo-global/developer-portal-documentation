---
description: Read for details on removing a funding account linked to a user
---

# Delete Funding Account

You can remove a funding account associated with a user by using this API. Once you remove the account, it will also not be provided when you retrieve the list of funding accounts for the user.

**`DELETE /v2/senders/{{senderId}}/funding-sources/{{fundingAccountId}}`**

{% tabs %}
{% tab title="Parameters" %}
| Field            | Required | Type | Description         |
| ---------------- | -------- | ---- | ------------------- |
| senderId         | Yes      | UUID | Sender ID.          |
| fundingAccountId | Yes      | UUID | Funding Account ID. |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X DELETE {{url}}/v2/senders/{{senderId}}/funding-sources/{{fundingAccountId}} \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}

{% endtab %}
{% endtabs %}
