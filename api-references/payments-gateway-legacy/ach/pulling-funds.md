# Pulling funds

#### Pulling funds from a Personal Account

#### JSON Structure

```json
{
  "capture": false,
  "externalPaymentId": "10123090",
  "paymentType": "REGULAR",
  "amount": {
    "total": 18.59,
    "currency": "USD"
  },
  "ipAddress": "2607:fb90:3c10:1270:1cac:5aba:ca1:906c",
  "customer": {
    "firstName": "TEST",
    "lastName": "USER",
    "gender": "MALE",
    "phoneNumber": "00000000",
    "email": "test@hotmail.com"
  },
  "billingAddress": {
    "countryCode": "US",
    "city": "Somerville",
    "line1": "2342 Boston St",
    "state": "MA",
    "zipCode": "00000"
  },
  "source": {
    "type": "BANK_ACCOUNT",
    "bankAccount": {
      "accountType": "checking",
      "accountNumber": "123123123123",
      "routingNumber": "1231",
      "accountHolder": {
        "type": "personal",
        "firstName": "TEST",
        "lastName": "USER"
      }
    }
  }
}

if the ACH account being pulled belong to a company, the accountHolder must be:

    "accountHolder": {
        "type": "business",
        "companyLegalName": "INYO"
      }
```

#### Fields Description

* **capture**:
  * Description: Indicates whether the transaction should be captured immediately.
  * Type: Boolean
  * Example: `false`
* **externalPaymentId**:
  * Description: External payment identifier provided by the merchant.
  * Type: String
  * Example: `"10123090"`
* **paymentType**:
  * Description: The type of payment being processed.
  * Type: String
  * Values:
    * `REGULAR`: A standard payment transaction.
  * Example: `"REGULAR"`
* **amount**:
  * **total**:
    * Description: The total amount of the transaction.
    * Type: Number
    * Example: `18.59`
  * **currency**:
    * Description: The currency of the transaction.
    * Type: String
    * Example: `"USD"`
* **ipAddress**:
  * Description: The IP address of the customer initiating the transaction.
  * Type: String
  * Example: `"2607:fb90:3c10:1270:1cac:5aba:ca1:906c"`
* **customer**:
  * **firstName**:
    * Description: The first name of the customer.
    * Type: String
    * Example: `"TEST_USER"`
  * **lastName**:
    * Description: The last name of the customer.
    * Type: String
    * Example: `"TEST_USER"`
  * **phoneNumber**:
    * Description: The phone number of the customer.
    * Type: String
    * Example: `"0052151506"`
  * **email**:
    * Description: The email address of the customer.
    * Type: String
    * Example: `"samirfilho@hotmail.com"`
* **billingAddress**:
  * **countryCode**:
    * Description: The country code for the billing address.
    * Type: String
    * Example: `"US"`
  * **stateCode**:
    * Description: The state code for the billing address.
    * Type: String
    * Example: `"MA"`
  * **city**:
    * Description: The city of the billing address.
    * Type: String
    * Example: `"Somerville"`
  * **line1**:
    * Description: The first line of the billing address.
    * Type: String
    * Example: `"51 Boston St"`
  * **state**:
    * Description: The state of the billing address.
    * Type: String
    * Example: `"MA"`
  * **zipCode**:
    * Description: The ZIP code of the billing address.
    * Type: String
    * Example: `"02143"`
* **source**:
  * **type**:
    * Description: The type of payment source used.
    * Type: String
    * Values:
      * `BANK_ACCOUNT`: A payment made using a bank account (ACH).
    * Example: `"BANK_ACCOUNT"`
  * **bankAccount**:
    * **accountType**:
      * Description: The type of bank account.
      * Type: String
      * Values:
        * `savings`: Savings account.
        * `checking`: Checking account.
      * Example: `"checking"`
    * **accountNumber**:
      * Description: The account number to be debited.
      * Type: String
      * Example: `"123123123123"`
    * **routingNumber**:
      * **Description**: The routing number of the bank where the debit will be performed.
      * **Type**: String
      * **Example**: `"1231"`
    * **accountHolder**:
      * **type**:
        * Description: The type of account holder.
        * Type: String
        * Values:
          * `personal`: Indicates the account holder is an individual.
          * `business`: Indicates the account holder is a business entity.
        * Example: `"personal"`
      * **firstName**:
        * Description: The first name of the account holder if the type is `PERSONAL`.
        * Type: String
        * Example: `"TEST"`
      * **lastName**:
        * Description: The last name of the account holder if the type is `PERSONAL`.
        * Type: String
        * Example: `"USER"`
