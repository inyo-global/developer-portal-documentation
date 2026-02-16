---
hidden: true
---

# Remittance

Integration recommendations for remittance use case

## Overview <a href="#overview" id="overview"></a>

This document will guide you through the API endpoints for the remittance use case. Remittance transactions include processing and delivery of person-to-person transactions. Based on our client’s requirements and agreement with Inyo, the transactions can be delivered either using Inyo's payout network or Client’s own payout network. Regardless, these are external transactions to external receivers who may be residing in a country different from the sender. Please note that these are recommendations for standard flow and can be customized as per your needs.

## 1. Sender Registration <a href="#id-1.-sender-registration" id="id-1.-sender-registration"></a>

The first step in the user flow requires you to register a Sender in our system. For more details on the required information, you can refer to the Register Sender API. Please note that senders can only be registered from the United States of America and the respective States which have been enabled for you as per your agreement with Inyo. The list of available states can be retrieved from the States API.

## 2. Sender KYC and Transaction Limits <a href="#id-2.-sender-kyc-and-transaction-limits" id="id-2.-sender-kyc-and-transaction-limits"></a>

Once a sender has been registered, you will have to provide additional sender information so that necessary KYC checks can be conducted on the sender. In order to run KYC, you can use two different methods:

### **KYC using Widget**

For the Widget Based KYC, you need to conduct the following steps:

1. Generate a widget token for the Sender. The details can be found here.
2. Load the KYC Widget v1.0 with type: kyc. The details for loading the widget can be found [here](https://about/api-references/widget/widget-v1.0#1.-include-the-widget-script).
3. Once the widget is loaded, the user will be able to submit all the necessary KYC info in the widget. Please note that in the widget the required fields are auto rendered so you don’t have to take additional action.
4. CSS can be applied to the widget to match the theme of your website/mobile APP

### **KYC using API**

For API Based KYC, you need to conduct the following steps:

1. Use the Get KYC Field API to fetch the current KYC status of sender and the list of information required to complete the basic user verification.
2. Use the Update KYC API to patch the information requested in the GET KYC Field API. You can provide all the required sender information at once or update the sender information partially.
3. Once all the requested information is submitted, use the Initiate KYC API to run the verification on the user. Please note that KYC will not be conducted unless you use this Endpoint.
4. If the user needs to submit sensitive information such as SSN or ID, it needs to be submitted via. Widget v2.0. This widget version is only applicable if you use an API based KYC solution. Details for widget version 2.0 can be found here.

Clients will be notified of the KYC status of the users using [events](https://about/api-references/webhooks/events#sender-events).

### **Increase User limits**

In order to increase the transaction limit of the user, additional information is required to be submitted. The requirement varies depending on the profile of the client and is outlined in their spec sheet. Currently, there are a total of 3 Tier levels that a user can access. KYC verification provides access to tier 1 and users will need to upgrade their tier for additional limits. To increase user limit, you need to follow the mentioned steps:

1. Ensure that the KYC status of the user is Verified.
2. Load the widget v1.0 with type: tier and ask the user to submit additional information. The process to load the Tier widget is the same as the KYC widget.
3. Once the user submits their information, the upgrade request will be approved by Inyo admin.
4. Clients will be notified of the status of tier upgrade requests through [webhooks](../api-references/remittances-raas/webhooks/).

## 3. Add Funding Account <a href="#id-3.-add-funding-account" id="id-3.-add-funding-account"></a>

Once a sender has submitted their information for verification, the user will then have to link a funding source for the transaction. Users can link either their bank accounts and/or debit cards for funding their transactions. They need to be added using either the CARD or BANK widget to ensure security. The process of loading the CARD and BANK widget is the same as the KYC widget, however, you will need to specify the appropriate type for the widget. Once a funding account has been linked, further information can be retrieved from the Funding Source API.

The sender can add a funding account and proceed to create a transaction as long as their KYC status is not UNVERIFIED or SUSPENDED.

## 4. Add Receiver <a href="#id-4.-add-receiver" id="id-4.-add-receiver"></a>

Once the KYC details for the sender have been submitted and verification has been initiated, you can proceed to add a recipient. You will need to register at least one recipient to be able to initiate a transfer. Based on the payout method and corridor of the transaction, different details of the receiver may need to be collected. The details of required information of the recipient are specified on your spec sheet. You can refer the recipient section for more details.

## **5. Add a Receiver Account** <a href="#id-5.-add-a-receiver-account" id="id-5.-add-a-receiver-account"></a>

The sender can send money using any payout method available in the country of the recipient. If the payout method is bank deposit or wallet transactions, then you will also need to add receiver's account details for Bank Deposit and Wallet transactions. The available banks and their details can be obtained using the Banks API. while the available wallets and their details can be obtained using the Payers API.

For Cash Pickup transactions, you do not need to add a receiver account but instead will need to select a payer location during transaction creation. The available payer details can be obtained using the Payers API. The associated ID for the selected location will have to be passed during transaction creation.

## 6. Create Transaction <a href="#id-6.-create-transaction" id="id-6.-create-transaction"></a>

Once senders have completed the KYC details of the sender, linked a funding account and added a receiver/receiver account, senders are now able to create a transaction.

Please note that senders can only create transactions within the limits set in their spec sheet. For higher transaction levels, users will need to submit additional KYC information to have their tier upgraded. The next transaction limit available to a user can be fetched using the transaction limit API.

Once you have ensured that the user has sufficient limits, you can proceed to create a transaction.

## 7. Webhooks <a href="#id-7.-webhooks" id="id-7.-webhooks"></a>

When the state of a resource changes, Inyo generates a new event resource to record the change. When an Event is created, a Webhook will be created to deliver the Event to any URLs specified by your active Webhook Subscriptions. Details on webhooks can be found here.

## 8. Transaction Delivery <a href="#id-8.-transaction-delivery" id="id-8.-transaction-delivery"></a>

Even though a transaction has been created, a transaction will not be forwarded for payout to the beneficiary unless you send a delivery request. You can choose to send a delivery request once the funds have been debited from the sender (transaction status: PROCESSED) to contain risks or beforehand to ensure faster payments. To forward a transaction for payout, you need to use the Delivery request API, details can be found here.

If you are delivering the transaction using your own payout network and NOT using our network, you will need to update the delivery status on our system as well.
