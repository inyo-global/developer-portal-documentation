# Widget v1.0 Types

**Know Your Customer Widget (Sender Identity Verification Widget)**

This widget can be triggered by passing widget type as `kyc`. Please note that the senders would not be allowed to change any of their details once their KYC has been verified.

**Link/Add Sender Bank Widget**

This widget can be accessed when the sender’s KYC status is not UNVERIFIED or SUSPENDED. This widget can be triggered by passing widget type as `bank`.

**Link/Add Sender Debit Card Widget**

This widget can be accessed when the sender’s KYC status is not UNVERIFIED or SUSPENDED. This widget can be triggered by passing widget type as `card`.

**Change Sender Bank Credentials**

In case the sender bank credentials have been changed, the sender is required to re-login to their account with updated credentials. This event is notified through a webhook with event name `sender_bank_login_required`. In this case, the widget needs to be initialized using the `bankId`.

**Upgrade Tier Widget**

If the sender wants to transfer more funds than the default limits then they will need to upgrade to a higher tier. For this, they will need to provide additional information. This widget can be triggered by passing widget type as “tier”. Please refer to the Compliance section in Machnet White Label Integration Guide for more details.

**Invoice Widget**

The Invoice Widget will be used to generate MSB specific invoice once a transaction has been created. This widget will only be accessible after the transaction has been created on Machnet platform and can be triggered by passing the widget type as `invoice` and the respective `transactionId`.

```
<script>
  var widget = new MachnetWidget({
    elementId     : "widget-root",
    senderId      : "d084fa75-fad4-40fc-a2b8-7996ea56d044",
    width         : "100%",
    height        : "2000px",
    type          : "invoice",
    locale        : "en",
    transactionId : "rektaa75-fad4-40fc-a2b8-7996ea56d044",
    token         : "very.long.token"
 });
 widget.init();
</script>
```

**3DS Widget**

This widget will be accessible after card txn is created on Machnet platform. This widget can be triggered by passing widget type as `3ds`. In this case, the widget needs to be initialized using the `senderId`, `token` and `transactionId`.
