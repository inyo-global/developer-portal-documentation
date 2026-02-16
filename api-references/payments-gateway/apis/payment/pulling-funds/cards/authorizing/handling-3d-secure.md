# Handling 3D secure

There are two types of 3D Secure (3DS) authentication: **data-only and challenge**. The key difference lies in the level of customer interaction. **Data-only** transactions are completed without involving the cardholder — only transaction data is shared with the issuer to assess risk. In contrast, **challenge** transactions require the cardholder to verify their identity through an additional step, such as entering a code or using biometrics. The type of 3DS used can impact both user experience and fraud prevention measures.

It is important to note that in the United States, banks are not mandated to enforce step-up authentication. Instead, the decision to opt for a data-only transaction or to issue a challenge rests solely with the bank, even if you request during authentication. This flexibility allows banks to apply their risk-based approach, adapting authentication processes according to the specifics of each transaction.

_Note: For the AFT markets, 3DS Secure is solely designed for security/fraud control, as there is no liability shift, even if 3DS is fully authenticated._

Our solution allows you to request 3DS in two different ways:

* **Automatically**, via configuration in our backend. All payment transactions will require 3DS authentication, and if the bank accepts and supports it, 3DS will be performed;
* **Manually controlled**, meaning that you can decide when you want to request it, during the tokenization of the card (see threeDSData section of the tokenization library);

Overall flow of a 3DS transaction with challenge:

* Tokenizer is configured to request 3DS. Two URLs are mandatory: success and fail. Those urls will be called by the 3DS provider to indicate the result of a transaction;
* Client types in the card data in the form, a click to pay;
* Tokenizer issues an API call to the backend, passing card holder data, as well as client device data;
* Tokenizer returns a token. Frontend issues a backend call to complete the transaction;
* Backend calls Inyo's authorization API, passing the token and all required data;
* Inyo's API return status 200, along with  status: CHALLENGE and redirectAcsUrl in the data element;
* Upon receiving this, frontend detects and redirects the client to the redirectAcsUrl;
* Client is presented the bank authentication page and proceeds to the authentication; if success, customer will be redirected to the "success url" ; if not, will be redirected to the "failed url";
* Success URL will receive a POST from the 3DS server along with transaction data\[1], and authorization/pre-authorization will be automatically executed (depending on how you have requested during tokenization). It's imperative to check Inyo's backend to confirm payment status; if everything is fine, payment confirmation is presented;
* Failed URL will receive a POST from the 3DS server; customer has to be redirected to the payment flow again;



\[1] payload received in a success url

```json
{
    "paymentId": "",
    "externalPaymentId": "",
    "amount": "",
    "approved": "true"
}
```

\[2] payload received as a result of failed challenge

```json
{
  "paymentId": "",
  "externalId": "",
  "amount": 19.99,
  "approved": false,
  "status": "Rejected by ACS",
  "responseMsg": "Rejected by ACS",
  "responseCode": "99",
  "acquirerMessage": "Rejected by ACS",
  "redirectUrl": "",
  "schemeId": "",
  "providerId": "",
  "cardTokenId": "",
  "gatewayProviderId": "",
  "issuerName": "",
  "issuerCountry": "",
  "needReversedByRules": false,
  "signatureVerification": "N",
  "acsChallengeRequired": true,
  "acsCavv": "",
  "acsParStatus": "N",
  "acsTransactionId": "",
  "acsErrorNumber": 0,
  "acsErrorDesc": "",
  "acsEciFlag": "",
  "acsThreeDSServerTransactionId": "",
  "acsReferenceNumber": ""
}
```
