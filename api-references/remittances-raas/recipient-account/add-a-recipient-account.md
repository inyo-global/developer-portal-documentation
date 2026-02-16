---
description: Read on adding a recipient
---

# Add a Recipient Account

This API can be used to add recipient’s bank or wallet account. Recipient account information details needs to be added prior to initiating a transfer.

**`POST /v2/senders/{{senderId}}/recipients/{{recipientId}}/accounts`**

{% tabs %}
{% tab title="Parameters" %}
| Parameter                             | Required       | Type    | Description                                                                                                                                                                                                                                                                        |
| ------------------------------------- | -------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| senderId                              | Yes, in URL    | UUID    | ID of the sender                                                                                                                                                                                                                                                                   |
| recipientId                           | Yes, in URL    | UUID    | ID of the recipient                                                                                                                                                                                                                                                                |
| account\_number                       | Yes, if bank   | String  | Bank account number.                                                                                                                                                                                                                                                               |
| account\_type                         | Yes, if bank   | String  | Account type. Enumerated value - `checking`, `savings, debit_card_deposit`                                                                                                                                                                                                         |
| bank\_id                              | Yes, if bank   | String  | Bank ID. Details can be obtained from this [API](https://raas.docs.machnetinc.com/api-references/data-population/banks).                                                                                                                                                           |
| branch\_id                            | Yes, if bank   | String  | Branch ID. Details can be obtained from this [API](https://raas.docs.machnetinc.com/api-references/data-population/banks).                                                                                                                                                         |
| branch\_location                      | No             | String  | Branch location. Details can be obtained from this [API](https://raas.docs.machnetinc.com/api-references/data-population/banks).                                                                                                                                                   |
| payout\_method                        | Yes            | String  | Payout method type. Enumerated value - `BANK_DEPOSIT`, `WALLET`                                                                                                                                                                                                                    |
| identification\_value                 | Yes, if wallet | String  | Recipient's mobile wallet ID. Mobile Wallet ID is recipient phone number along with country code and area code.                                                                                                                                                                    |
| identification\_type                  | No             | String  | Wallet type. Enumerated value - `TEL`. The default value for identification\_type is `TEL`                                                                                                                                                                                         |
| payer\_id                             | Yes, if wallet | Integer | ID of payer associated with receiver's wallet. Details can be obtained from this [API](https://raas.docs.machnetinc.com/api-references/data-population/payers).                                                                                                                    |
| account\_holder\_name.first\_name     | No             | String  | The account holder’s first name. When applicable, the provided account holder name is matched with the actual account holder’s name received from the bank/wallet provider for verification purposes. If this field is not provided, the first name of the recipient will be used. |
| account\_holder\_name.middle\_name    | No             | String  | The account holder’s middle name. If this field is not provided, the middle name of the recipient will be used.                                                                                                                                                                    |
| account\_holder\_name.last\_name      | No             | String  | The account holder’s last name. If this field is not provided, the last name of the recipient will be used.                                                                                                                                                                        |
| additional\_info.routing\_code        | Conditional    | String  | This field may be required for recipient bank accounts in the US. Please refer to your spec sheet.                                                                                                                                                                                 |
| additional\_info.ifs\_code            | Conditional    | String  | This field may be required for recipient bank accounts in the India. Please refer to your spec sheet.                                                                                                                                                                              |
| additional\_info.swift\_bic\_code     | Conditional    | String  | This field may be required for recipient bank accounts in the UK. Please refer to your spec sheet.                                                                                                                                                                                 |
| additional\_info.branch\_number       | Conditional    | String  | Branch number of bank. This field may be required for certain recipient bank accounts.                                                                                                                                                                                             |
| additional\_info.bsb\_number          | Conditional    | String  | Bank-State-Branch is a six-digit number that identifies banks and branches. This field may be required for certain recipient bank accounts.                                                                                                                                        |
| additional\_info.bik\_code            | Conditional    | String  | Identification code of the bank. This field may be required for certain recipient bank accounts.                                                                                                                                                                                   |
| additional\_info.cbu\_alias           | Conditional    | String  | Alphanumeric code for each bank account. This field may be required for certain recipient bank accounts.                                                                                                                                                                           |
| additional\_info.sort\_code           | Conditional    | String  | Six digit number that identifies recipient bank. This field may be required for certain recipient bank accounts.                                                                                                                                                                   |
| additional\_info.aba\_routing\_number | Conditional    | String  | Nine numeric characters used by banks to identify specific financial institutions. This field may be required for certain recipient bank accounts.                                                                                                                                 |


{% endtab %}

{% tab title="Request Sample" %}
**Bank Account**

```
curl -X POST {{url}}/v2/senders/{{senderId}}/recipients/{{recipientId}}/accounts \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret' \
  -d { \
    "account_number": "string", \
    "account_type": "checking", \
    "bank_id": 0, \
    "branch_id": 0, \
    "branch_location": "string",
    "payout_method": "BANK_DEPOSIT",\
    "account_holder_name": {
        "first_name": "string",
        "middle_name": "string",
        "last_name": "string"
   },
    "additional_info": {
      "ifs_code": "",
      "swift_bic_code": "",
      "routing_code": "074000078"
     }
  }
```

**Wallet**

```
curl --location --request {{url}}/v2/senders/{{senderId}}/recipients/{{recipientId}}/accounts \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret' \
  -d { \
  "identification_value":"254723993187",
  "identification_type": "TEL",
  "payout_method": "WALLET",
  "payer_id": 3,
  "account_holder_name": {
      "first_name": "string",
      "middle_name": "string",
      "last_name": "string"
  }
}
```
{% endtab %}

{% tab title="Response Sample" %}
**Bank Account**

```
{
  "id": "string",
  "recipient_id": "string",
  "account_number": "string",
  "account_type": "checking",
  "bank_id": 0,
  "branch_id": 0,
  "branch_location": "string",
  "flagged": false,
  "payout_method": "BANK_DEPOSIT",
  "status": "UNVERIFIED",
  "account_holder_name": {
      "first_name": "string",
      "middle_name": "string",
      "last_name": "string"
  },
    "additional_info": {
      "ifs_code": "",
      "swift_bic_code": "",
      "routing_code": "074000078"
  }
}
```

**Wallet**

```
{
  "id": "string",
  "recipient_id": "string",
  "identification_value": "254723993187",
  "identification_type": "TEL",
  "status": "VERIFIED",
  "flagged": false,
  "payout_method": "WALLET",
  "account_holder_name": {
      "first_name": "string",
      "middle_name": "string",
      "last_name": "string"
  }
}
```
{% endtab %}
{% endtabs %}
