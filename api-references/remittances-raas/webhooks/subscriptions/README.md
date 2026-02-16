---
description: Read on how to get and manage Webhooks
---

# Subscriptions

Create a webhook subscription to receive POST requests from the platform (called webhooks) when events associated with your application occur. Webhooks are sent to a URL which you provide when creating a webhook subscription. Refer to the events section for the list of events that trigger webhooks.

{% tabs %}
{% tab title="Object Details" %}
| Field        | Type     | Description                                      |
| ------------ | -------- | ------------------------------------------------ |
| id           | UUID     | ID                                               |
| end\_point   | URL      | URL where webhooks will be forwarded.            |
| secret       | string   | A key used to maintain message integrity.        |
| active\_from | datetime | Date/time when the subscription URL was created. |
| expired\_at  | datetime | Date/time when the subscription URL was removed. |
| is\_paused   | boolean  | True if the subscription is paused.              |
| is\_active   | boolean  | True if the subscription is active.              |
{% endtab %}
{% endtabs %}
