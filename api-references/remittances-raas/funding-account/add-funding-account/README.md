---
description: Read on adding funding accounts in safe and secure way
---

# Add Funding Account

The sender can add either bank account or link debit cards through [Widget v1.0,](../../widget/widget-v1.0/) type `Bank` or `Card`. Follow the instructions on how to generate widget token and initialize widget. Once the widget has bee initialized,

1. For banks, users will have to select their bank. There are two types of flows that users can be directed to based on the bank they select.

* Non-OAuth: Users authenticate and permission data directly from the widget to allow us access to their financial accounts.
* [OAuth](oauth-integration.md): OAuth provides a more secure connection for your users as credentials are handled entirely by the OAuth provider (bank) and exchanged for a token that we can use. OAuth connections are predefined based on the bank's policies and our integrations. To connect accounts via OAuth, users will be directed to the bank's website for authentication and authorization. Once the user grants permission, the user will have to be redirected to our widget to complete the flow.

2. For debit cards, users will have to add their card details including card number, expiry date, and CVV code. Please note that:

* Cards are removed when they expire and you will be notified when a card is expiring through [webhooks](../../webhooks/events.md).
* If the same card is added by multiple senders within the same Affliate or across the Inyo Platform, the card will be tagged as [Duplicate](../funding-account-object.md). If the senders are within the same Affiliate, you will also receive details on the senders that have added the same card when you fetch the funding account details of the duplicate card. The duplicate flag information you will receive are outlined in the[ funding account object details](../funding-account-object.md). Please note that if a new card is added which is duplicate with a card added earlier, both cards will be flagged as duplicate. Since the old card resource has been updated, you will also receive a [webhook](../../webhooks/events.md) to indicate the change.
