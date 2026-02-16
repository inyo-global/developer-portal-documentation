# Transaction

## Test value for Transaction Status Change <a href="#test-value-for-transaction-status-change" id="test-value-for-transaction-status-change"></a>

### **For Bank Deposit**

| Transaction Amount                | Status    |
| --------------------------------- | --------- |
| All values except below mentioned | Processed |
| 99 (Including Fee)                | Failed    |
| 55 (Including Fee)                | Returned  |

### **For Debit Card transaction**

| Transaction Amount | Status    |
| ------------------ | --------- |
| 10 (Including Fee) | Canceled  |
| Other              | Processed |

## Test value for Transaction Risk Score Check <a href="#test-value-for-transaction-risk-score-check" id="test-value-for-transaction-risk-score-check"></a>

The risk score of every transaction created in Inyo Platform is evaluated and necessary action is taken, which can result in a transaction being canceled or kept on hold for further checks. The risk score can lead to the following scenarios:

1. If a transaction has a Risk Score between 1-7, the transaction will pass the Risk Check
2. If a transaction has a Risk Score between 7-70, the transaction will go On Hold and the transaction must be approved or canceled from the compliance team.
3. If a transaction has a Risk Score higher than 70, the transaction will be automatically canceled.

To simulate the various risk-score of a transaction, you can use the following test values.

| Sender Email                      | Min Risk Score | Max Risk Score | Remarks                                                                                             |
| --------------------------------- | -------------- | -------------- | --------------------------------------------------------------------------------------------------- |
| All values except below mentioned | 1              | 7              |                                                                                                     |
| <_>suspicious<_>@example.com      | 7              | 70             | You need to include the word `suspicious` in the sender email to get a risk score between 7 and 70. |
| <_>risky<_>@example.com           | 70             | 100            | You need to include the word `risky` in the sender email to get a risk score greater than 70.       |
