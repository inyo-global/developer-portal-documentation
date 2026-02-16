# Trust level limits

[Documentation](https://app.gitbook.com/o/RgJnijNmfIAmDAwzEkvD/s/O44q8hR5fecwbHnmU0Nz/)

Endpoint: /participants/{participantId}/complianceLevels

The Check Trust Level endpoint allows you to retrieve the current compliance level (trust level) of a participant and view their associated transaction limits and required KYC fields. It also provides visibility into the next available compliance level, including the fields that must be collected to upgrade.

This endpoint is designed to help you dynamically guide users through the onboarding process, ensuring they meet the necessary compliance requirements to send higher transaction volumes. It also allows your application to monitor participants’ eligibility to act as senders in financial transactions.

***

Use Cases

* Display a participant’s daily, monthly, and semi-annual sending limits.
* Determine if a participant can be used as a sender in a transaction.
* Identify which fields must be collected to unlock the next trust level.
* Proactively prompt users to submit missing KYC data.
* Tailor user onboarding experiences based on compliance readiness.

***

#### Sample Response (Trust Level Overview)

The following table represents a typical response returned by the endpoint for a participant currently at LEVEL\_0 with the potential to upgrade to LEVEL\_1:

**Current Compliance Level: LEVEL\_0**

| Period   | Limit | Currency |
| -------- | ----- | -------- |
| 1 Day    | 0.00  | USD      |
| 30 Days  | 0.00  | USD      |
| 180 Days | 0.00  | USD      |

**Next Compliance Level: LEVEL\_1**

| Period   | Limit    | Currency |
| -------- | -------- | -------- |
| 1 Day    | 2,999.00 | USD      |
| 30 Days  | 6,000.00 | USD      |
| 180 Days | 9,999.00 | USD      |

