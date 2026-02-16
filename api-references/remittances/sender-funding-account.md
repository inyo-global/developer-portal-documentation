# Sender funding account

To support seamless transaction initiation, each Sender can be linked to a Funding Account, which represents the source of funds used for sending payments. The funding account works in conjunction with Inyo’s modern payment gateway to manage the lifecycle of inbound and outbound transactions using a secure, ledger-based architecture.

***

#### Supported Payment Methods

The platform supports multiple funding mechanisms to accommodate a variety of use cases and regulatory environments:

* ACH (Automated Clearing House): Bank transfers via the U.S. financial network
* Cards: debit card payments (AFT) through tokenized and PCI-compliant processing
* Wallets / P2P Settlement: Peer-to-peer balances and internal wallets for same-day or instant transfers

Each funding method is abstracted through a unified ledger system, which records every debit and credit transaction for compliance, reconciliation, and settlement tracking.

***

#### How It Works

1. Sender is created and approved via KYC
2. A funding account is linked to the sender, specifying the type (e.g., ACH, Card, Wallet)
3. When a transaction is initiated:
   * Funds are debit-locked or collected from the funding source
   * The transaction is recorded in the ledger
   * On approval, the funds are settled to the receiver via the configured payout method

This flow ensures real-time visibility, atomic balance updates, and traceability across all movement of funds.

[Documentation](https://dev-api.inyoglobal.com/sandbox/#tag/payout_Funding-Account-Resource/paths/~1organizations~1%7Btenant%7D~1payout~1participants~1%7BparticipantId%7D~1fundingAccounts/post)\
Endpoint: /organizations/{tenant}/payout/participants/{participantId}/fundingAccounts

