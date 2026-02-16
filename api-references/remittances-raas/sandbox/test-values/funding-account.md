# Funding Account

## Test Value for linking a Bank account <a href="#test-value-for-linking-a-bank-account" id="test-value-for-linking-a-bank-account"></a>

| Username     | Password    | MFA  | Case                                           |
| ------------ | ----------- | ---- | ---------------------------------------------- |
| user\_good   | pass\_good  | -    | Every account linked will have same details.   |
| user\_good   | mfa\_device | 1234 | MFA Required                                   |
| user\_custom | {}          | -    | Every account linked will have random details. |

{% hint style="info" %}


For OAuth Banks, use Platypus OAuth Bank in bank widget. The dummy institution does not direct you to a real bank's site, but allows you to grant, deny, or simulate errors from the placeholder OAuth page instead.

Ensure that you are able to replicate the following scenarios once you support the suggested OAuth support/

1. Users should be able to add Non-OAuth Bank.
2. Users should be able to add OAuth Bank.
3. Users should be able to update credentials for Non-OAuth banks.
4. Users should be able to update credentials for OAuth banks.
{% endhint %}

## Test value for Balance Check <a href="#test-value-for-balance-check" id="test-value-for-balance-check"></a>

If a user’s bank account does not have sufficient funds at the time of transaction creation, the transaction will be automatically cancelled.

When adding a funding source an account with balance can be linked. Enter amount lower than the one available in the linked funding source for Balance check Pass. Enter amount higher than the one available in the linked funding source for Balance check Fail.

## Test value for Debit Card <a href="#test-value-for-debit-card" id="test-value-for-debit-card"></a>

| Card Number      | Network          | Type        | Status                         |
| ---------------- | ---------------- | ----------- | ------------------------------ |
| 4000056655665556 | Visa             | Exempted    | Card Successfully Added        |
| 4005519200000004 | Visa             | Regulated   | Card Successfully Added        |
| 4000000760000002 | Visa             | Regulated   | Card Successfully Added        |
| 4500600000000061 | Visa             | Exempted    | Card Successfully Added        |
| 4217651111111119 | Visa             | Debit Card  | Pull Transaction Not Supported |
| 2223000048400011 | MasterCard       | Exempted    | Card Successfully Added        |
| 5200828282828210 | MasterCard       | Regulated   | Card Successfully Added        |
| 2223003122003222 | MoneySend        | Debit Card  | Pull Transaction Not Supported |
| 5555555555554444 | MoneySend        | Debit Card  | Pull Transaction Not Supported |
| 371449635398431  | American Express | Debit Card  | Pull Transaction Not Supported |
| 6011111111111117 | Discover         | Debit Card  | Pull Transaction Not Supported |
| 4111111111111111 | Visa             | Credit Card | Card Successfully Added.       |
| 5403879999999997 | MasterCard       | Credit Card | Card Successfully Added.       |
| 378282246310005  | American Express | Credit Card | Card Successfully Added.       |

{% hint style="info" %}
If Credit Card is not enabled for you, you will receive the message "Sorry, we only support Debit cards." in the widget. Please get in touch with our customer support to enable credit cards.
{% endhint %}

{% hint style="info" %}
Adding the following card multiple times in sandbox will trigger a duplicate card check. The details of how our duplicate card check works is outlined here.

card number: 4005519200000004

expiry date: Any future date
{% endhint %}

| Security (CVV) Code | 123                       |
| ------------------- | ------------------------- |
| Expiry Date         | Any future date will work |

{% hint style="info" %}
Cards are removed when they expire from the Machnet system. To test the case in sandbox, please use expiry date as 06/32 while adding a card. All such cards will expire in an hour. You will receive `expiring_soon` webhook in 15 minutes and `card_removed` webhook in an hour.
{% endhint %}

## Test Values for 3DS using Card <a href="#test-values-for-3ds-using-card" id="test-values-for-3ds-using-card"></a>

| Card Number      | Network    | 3DS flow     | Results                                                                                                                      |
| ---------------- | ---------- | ------------ | ---------------------------------------------------------------------------------------------------------------------------- |
| 4000056655665556 | Visa       | Frictionless | 3DS verification status will be FAILED and transaction will be Canceled.                                                     |
| 2223000048400011 | MasterCard | Frictionless | 3DS verification status will be FAILED and transaction will be Canceled.                                                     |
| 4005519200000004 | Visa       | Frictionless | 3DS verification status will be VERIFIED and transaction will be further processed.                                          |
| 5200828282828210 | MasterCard | Frictionless | 3DS verification status will be VERIFIED and transaction will be further processed.                                          |
| 4000000000002446 | Visa       | Frictionless | 3DS verification status will be FAILED and transaction will be Canceled.                                                     |
| 4000000000002354 | Visa       | Frictionless | 3DS verification status will be FAILED and transaction will be Canceled.                                                     |
| 4500600000000061 | Visa       | Frictionless | 3DS verification status will be FAILED and transaction will be Canceled.                                                     |
| 4000000760000002 | Visa       | Challenge    | 3DS verification status will be VERIFIED if correct authentication code is passed and transaction will be further processed. |

PreviousSender VerificationNextTransaction

Last updated 3 months ago
