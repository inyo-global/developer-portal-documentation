# Handling AVS / CVC

The Address Verification System (AVS) is a security measure used to prevent credit card fraud during card-not-present transactions, such as online purchases. AVS works by comparing the billing address provided by the customer with the address on file with the credit card issuer. If the addresses match, the transaction is more likely to be legitimate; if not, it raises a fraud alert.

#### Importance of AVS in Fraud Prevention

1. **Mitigates Risk of Fraud:** By confirming the billing address, AVS helps reduce the likelihood of unauthorized use of stolen credit card information.
2. **Reduces Chargebacks:** Accurate address verification can lead to fewer valid transactions being disputed, minimizing chargeback rates for merchants.
3. **Enhances Trust:** Implementing AVS improves customer trust by demonstrating a commitment to secure transactions and protecting personal information.
4. **Cost-Effective:** It is a relatively low-cost solution compared to more advanced fraud detection tools, making it accessible for small to midsize businesses.
5. **Regulatory Compliance:** Use of AVS helps businesses comply with industry standards and regulations regarding data security and fraud prevention.

Incorporating AVS as part of a comprehensive fraud prevention strategy is crucial for businesses looking to protect themselves and their customers in the digital marketplace.

During the onboarding of your merchant account, AVS behavior might be configured directly at Inyo, meaning that we will take care of the authorization results on your behalf.

Other scenario, in which you want to control your risk tolerance, you may be able to take the right decision, based on the information we provide.

During the authorization cycle, by informing the customerAddress, our authorization system will interact with the networks and provide their response directly to you.

<mark style="color:$success;">**Example**</mark> <mark style="color:$success;"></mark><mark style="color:$success;">of a CARD response, after a payment call</mark>

```json
{
  "status": 200,
  "data": {
    "paymentId": "dce568c6-98ec-456c-bb33-4a6809c4fff8",
    "parentPaymentId": "dce568c6-98ec-456c-bb33-4a6809c4fff8",
    "externalPaymentId": "c6bfd9ac-906f-4218-b9ec-aabf0f83fd0c",
    "redirectAcsUrl": "",
    "amount": 10.25,
    "created": "2025-03-31 03:49:05",
    "approved": true,
    "message": "Payment Approved",
    "automaticReversed": false,
    "status": "APPROVED",
    "captured": false,
    "voided": false,
    "responseCode": "00",
    "issuerName": "BANK ISSUER",
    "issuerCountry": "ITALY",
    "cvcResult": "APPROVED",
    "avsResult": "FAILED",
    "avsCardholderNameResult": "N/A",
    "avsTelephoneResult": "N/A"
  }
}
```

#### AVS verification

avsResult is the verification of the address the issuing bank has on a file for a specific customer, taking into consideration house number, street and  zipcode code. AVS doesn't directly influence the authorization flow, but it's a strong indication for a possible fraud.

<table><thead><tr><th width="259.01953125">Status</th><th width="439.64453125">Description</th><th data-hidden></th></tr></thead><tbody><tr><td>APPROVED</td><td>The billing components are satisfied to the rules configured during onboarding of your merchant account. <br><br>Approved might mean that all billing components must be satisfied;<br><br>In some scenarios, only the zipcode might be satisfied.</td><td></td></tr><tr><td>FAILED</td><td>The billing components didn't satisfy the rules set for the authorization.</td><td></td></tr><tr><td>NOT_SENT</td><td>Billing data was not informed.</td><td></td></tr></tbody></table>

#### CVC verification

cvcResult is the verification of the last 3 (or 4 digits) of the card, performed by the issuer bank. Some banks allow the transaction to be authorized, even if the digits are not informed or and wrong. Our gateway will return:

<table><thead><tr><th width="259.01953125">Status</th><th width="439.64453125">Description</th><th data-hidden></th></tr></thead><tbody><tr><td>APPROVED</td><td>The digits match, and were verified by the issuing bank.</td><td></td></tr><tr><td>FAILED</td><td>The digits don't match, and were verified by the issuing bank. In general, if you receive this status and the transaction is approved, you should exercise caution in approving a card payment.</td><td></td></tr><tr><td>NOT_SENT</td><td></td><td></td></tr></tbody></table>

