# KYC Using API

You can also use our API to provide sender KYC information. In order to use our API, you will first need to register the sender with basic information. Once the sender is register,

1. Know what KYC information is required for the sender using our [Get KYC fields API](get-kyc-fields.md).
2. Collect and send the required information to Inyo. [All KYC information can be sent through API](update-kyc-information.md) _except_ for sensitive information such as sender SSN and sender identification document. You will need to provide sensitive information through [widget v2.0](../../../widget/widget-v2.0.md).
3. [Initiate the sender's KYC process](initiate-kyc.md). If all required information for KYC (Tier 1) as specified in your specification sheet is not provided, you will not be able to initiate KYC. Once KYC is initiated, you will not be able to update the sender information.
4. You will be notified of KYC status changes through [webhooks](../../../webhooks/events.md). However, you can also [get KYC details of a sender using this API](get-kyc-information.md).
