# Getting started

#### **Access to API Credentials**

Please note that our API credentials, including the public key, consumer API key, and consumer API secret, are not publicly accessible. To ensure the security and integrity of our services, access to these credentials is granted through a controlled process.

If you are interested in integrating with our API and require these credentials, please contact our commercial team. They will guide you through the necessary steps to verify your application and provide you with the specific credentials needed for your use case.

#### **Core Features of the API**

1. **Authentication Management**
   * **Token Generation**: Supports OAuth standards for secure authentication. The API allows the generation of access tokens and refresh tokens through a dedicated endpoint, enabling clients to authenticate and subsequently authorize their API requests securely.<br>
2. **Card Tokenization**
   * **Token Creation**: Facilitates the secure handling of card information by converting sensitive card details into a token. This process enhances security by minimizing the exposure of actual card details during transactions.<br>
3.  **Payment Processing**

    * **Pull payments**: Clients can initiate various types of payment transactions, including credit, debit, and ACH transfers. This endpoint supports comprehensive payment details including amounts, currency, and payer information.
      * For cards only
        * **Payment Capture**: Allows for the capture of pre-authorized payments, facilitating delayed charging scenarios commonly used in hospitality and retail industries.
        * **Payment Void**: Offers the ability to void transactions, typically used to cancel a payment before it is fully processed and settled.
      * For all payments
        * **Payment Refund**: Supports the refunding of transactions, which is essential for handling returns and customer service adjustments.
    * **Push payment**: Clients can initiate various types of push payment transactions, domestic or internationally, leveraging our robust network.&#x20;
    * **Payment Retrieval**: The API provides functionality to retrieve details of specific payments using an external identifier, allowing for easy tracking and management of transactions.



