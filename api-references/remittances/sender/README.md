# Sender

#### Initial Compliance Setup

At the beginning of your integration with Inyo, our compliance team works closely with your organization to define a custom compliance framework tailored to your product and customer profile. This framework determines:

* Compliance levels: tiers that define how much a customer is allowed to send within set timeframes (e.g., 24h, 30d, 180d).
* Validation rules: the required data and documents for each level (e.g., name, SSN, proof of income).
* Risk controls: thresholds that trigger enhanced due diligence.

This configuration is unique per tenant and directly impacts how participants are verified and which operations they are allowed to perform within the system.

***

#### Sender Setup

To initiate a transaction on the Inyo platform, you must first create a Sender — the participant who is initiating the payment. Every sender must pass through the KYC (Know Your Customer) process, and their eligibility is determined based on the compliance level achieved.

***

#### Creating Participants

Each transaction must include at least one participant, which can be either a Person or a Company (for companies, it's based on use case and proper risk assessment ). You can create them using the following endpoints:

* POST /person – to create an individual
* POST /companies – to create a business entity

A single participant may act as both sender and receiver in a transaction if needed. In this case, you can reuse the same participantId for both senderId and receiverId.

***

#### Sender Eligibility & Compliance Levels

To use a participant as a Sender, it must reach at least Compliance Level 1. Compliance levels are dynamically calculated based on:

* The fields and documents submitted
* The participant type (person or company)
* Your tenant’s configured compliance framework

Different tenants may have different thresholds, field requirements, and document validation rules.

***

#### Example Compliance Level

| Level   | Required Fields                           | 24h Limit | 30d Limit | 180d Limit |
| ------- | ----------------------------------------- | --------- | --------- | ---------- |
| Level 1 | firstName, lastName, address, phoneNumber | $X        | $X        | $X         |
| Level 2 | ssn, occupation, documentId               | $X        | $X        | $X         |
| Level 3 | proofOfSourceOfFunds                      | $X        | $X        | $X         |

> ⚠️ These values and requirements are customizable and will be defined during the onboarding process.

#### Best Practices



* Build your onboarding flow to collect only the fields necessary for the user’s target compliance level.
* If a participant attempts to transact above their limit, prompt them to complete additional KYC fields.
* Sync compliance state frequently if your platform allows user profile updates.
