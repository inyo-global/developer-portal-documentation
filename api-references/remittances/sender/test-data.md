# Test data

The following data can be used to test different compliance and transaction validation scenarios:



| Parameter            | Value            | Result                                                   | Compliance Status | Payout Status |
| -------------------- | ---------------- | -------------------------------------------------------- | ----------------- | ------------- |
| address.zipcode      | 99999            | Transaction will be REJECTED                             | Rejected          | Cancelled     |
| phoneNumber          | +14155557777     | Transaction will be REJECTED                             | Rejected          | Cancelled     |
| firstName + lastName | BLOCK LIST MATCH | Transaction will be REJECTED                             | Rejected          | Cancelled     |
| firstName + lastName | OFAC MATCH       | Transaction will be put ON HOLD for further verification | Hold              | Pending       |



