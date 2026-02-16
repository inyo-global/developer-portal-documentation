---
description: >-
  To ensure secure, reliable, and efficient operations, Inyo gateway leverages
  the following technologies and services
---

# Gateway environment settings

#### 1. **Authentication and Security with IdP**

Inyo gateway uses **IdP** for authentication and security management. The IdP enables:

* Centralized user management.
* Support for various authentication protocols (e.g., OAuth2, OpenID Connect).

#### 2. **Publish/Subscriber Architecture**

Inyo gateway processes transactions using a **publish/subscribe architecture** powered by **Industry Standards**. This mechanism ensures:

* Decoupling between services, allowing for independent scaling.
* Efficient transaction processing by distributing tasks among multiple consumers.
* High reliability through message durability and retry mechanisms.

#### 3. **KMS for Data Encryption**

To protect sensitive data, Inyo gateway uses **KMS (Key Management Service)** to create and manage encryption keys. These keys are used to securely store:

* Card details.
* Provider credentials.
* Other sensitive information that requires strict security compliance.

By integrating these technologies, Inyo gateway achieves a secure and scalable environment for managing financial transactions across diverse providers and payment methods.
