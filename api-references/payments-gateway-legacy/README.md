---
hidden: true
---

# Payments Gateway (legacy)

Inyo’s payment gateway merges advanced technology with versatile payment options, enabling the acceptance of cash, ACH transfers, and credit/debit cards. As we extend our platform's capabilities, we are excited to offer enhanced integration opportunities, particularly for card processing.

**Sandbox Environment**: \
Developers can access our dedicated sandbox at [https://sandbox-gw.simpleps.com](https://sandbox-gw.simpleps.com/) to conduct thorough testing of their payment processing workflows in a risk-free, fully functional replica of our live environment. This feature allows for debugging and enhancement of integrations without impacting actual payment operations.

**OAuth Standards and API Access**: \
In line with industry best practices, our API follows OAuth protocols, facilitating secure, token-based authentication and authorization. Comprehensive details, including how to implement OAuth, are available in our OpenAPI specification at https://sandbox-gw.simpleps.com/q/openapi. This specification provides a clear, executable API description, enabling developers to generate clients, servers, and documentation efficiently.

**Security and Compliance**: \
As a PCI Level 2 certified provider, we maintain stringent security measures mandated by this certification. This ensures that our partners and their customers can trust the integrity and security of our payment processing solutions.

**Integration Guidelines**:

* **PCI Certified Partners**: Entities with PCI certification, once cleared by our compliance team, are authorized to handle cardholder data directly. These partners can integrate deeply with our gateway, utilizing bespoke or advanced features.
* **Non-PCI Certified Customers**: Customers without PCI certification must use our SDKs for secure card tokenization. These SDKs abstract the complexities of direct card data handling, ensuring compliance and security without direct interaction with sensitive payment details.

**Documentation Examples**:

1. **Quick Start Guide**: A step-by-step introduction to getting set up with our API, from obtaining your API keys to making your first API call using our SDKs.
2. **Code Samples**: Practical examples in multiple programming languages, illustrating how to integrate various payment methods.
3. **API Reference**: Detailed descriptions of all API endpoints, parameters, and expected responses, supplemented by examples of request and response bodies.
4. **Error Handling**: Guidelines on how to interpret and resolve common errors encountered during API integration.

By providing a robust technical framework, detailed documentation, and secure, compliant integration options, Inyo ensures that all users, from tech-savvy developers to businesses needing straightforward solutions, can efficiently and safely integrate with our payment gateway.
