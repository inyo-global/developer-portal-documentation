---
description: >-
  The Inyo Gateway tracks the status of payments throughout their lifecycle
  using well-defined statuses.
---

# Payment Status

These statuses provide clarity on the current state of a payment and help clients manage their payment workflows effectively.

#### Payment Status Enum

The following statuses are available:

* **CHALLENGE**: The payment requires a challenge by the customer, 3DS flow.
* **AUTHORIZED**: The payment has been authorized but not yet captured.
* **VOIDED**: The payment authorization has been voided.
* **CAPTURED**: The payment has been successfully captured.
* **PARTIALLY\_CAPTURED**: A portion of the authorized amount has been captured.
* **PARTIALLY\_REFUNDED**: A portion of the captured amount has been refunded.
* **REFUNDED**: The full captured amount has been refunded.
* **DECLINED**: The payment was declined by the issuer or provider.

