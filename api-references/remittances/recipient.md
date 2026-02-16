# Recipient

A **Recipient** is the final beneficiary of a transaction—either an individual or a company who receives the funds. Recipients are created using the same participant creation flows as Senders, leveraging the shared Person or Company schema.

***

#### Creating a Recipient

To register a recipient, use the following endpoints:

* POST /person – for individual recipients
* POST /company – for business recipients

There is no separate API for recipients. Instead, the system distinguishes roles (Sender or Receiver) based on how the participantId is used in a transaction request (i.e., as receiverId).

***

#### Required Fields

The required information for a recipient is determined by the schema defined in the Person or Company model. This schema may vary depending on:

* The participant type (Person or Company)
* The recipient’s country or payout network
* Your tenant’s compliance configuratio

For example:

* A domestic payout via bank transfer may only require a name and account number.
* An international payout to Mexico may require full address, document ID, bank information and CLABE.

Your integration should always consult the schema definition to ensure all required fields are collected before attempting to create a transaction.

***

#### Reusing Participants

Since participants are reusable entities, a single participant can act as both a Sender and Recipient in different transactions—or even in the same transaction—by providing the same participantId for both senderId and receiverId.

***

#### Best Practices

* Validate required fields based on the payout destination and method before transaction submission.
* Use field validation tools or schema metadata to dynamically render forms in your UI.
* Always collect only the fields needed for the use case—unnecessary data may trigger enhanced compliance checks.
