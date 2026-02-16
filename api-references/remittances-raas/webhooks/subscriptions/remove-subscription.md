---
description: Read on completely removing subscriptions
---

# Remove Subscription

**`DELETE /v2/subscriptions/{{subscriptionId}}`**

{% tabs %}
{% tab title="Parameters" %}
| Field          | Required    | Type | Description      |
| -------------- | ----------- | ---- | ---------------- |
| subscriptionId | Yes, in URL | UUID | Subscription ID. |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X DELETE {{url}}/v2/subscriptions/{{subscriptionId}} \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
2XX
```
{% endtab %}
{% endtabs %}
