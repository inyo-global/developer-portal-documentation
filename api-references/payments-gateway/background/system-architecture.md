# System architecture

#### 1. **Microservices Architecture**

Inyo gateway is built on a microservices architecture, where each functionality is encapsulated in a standalone service, compiled and tested and deployed automatically. This approach ensures:

* Modular development and deployment.
* High scalability to handle varying workloads.
* Fault isolation, where issues in one service do not affect others.

Each microservice is containerized, enabling consistent environments and seamless deployment across different platforms

#### 2. **Payment Router Mechanism**

A central component of the architecture is the **Payment Router**, which dynamically selects the optimal payment route based on:

* **Transaction Type**: Determines whether the operation is a **pull** or **push**.
* **Payment Method**: Identifies the payment type (e.g., **ACH**, **Card**, or **different payouts formats**).
* **Currency**: Considers the currencies of the origin and destination countries to ensure compatibility and compliance with local regulations.

The router ensures that every transaction is processed efficiently and routed to the appropriate provider, optimizing performance and reliability.

#### 3. **Provider-Specific Microservices**

To cater to the diverse requirements of payment providers, each provider has its own dedicated microservice. These microservices:

* Implement the specific rules and standards mandated by their respective providers.
* Manage provider-specific APIs, ensuring seamless integration.
* Are independently deployable, allowing for quick updates and scaling.

#### 4. **Integration with Payment Router**

The provider microservices are tightly integrated with the **Payment Router**, making them readily available for the gateway to:

* Offer a wide range of payment integrations to client banks.
* Dynamically scale as new providers are added, enhancing the system’s flexibility.

This tightly coupled design ensures that the gateway remains extensible and capable of supporting future integrations without disrupting existing services.

#### 5. **Scalability and Security**

The architecture leverages modern design principles to ensure:

* **Horizontal Scalability**: Services can scale independently based on load.
* **Enhanced Security**: Each microservice operates in an isolated environment, reducing vulnerabilities.

This architecture makes Inyo gateway a powerful tool for businesses, enabling seamless financial transactions while maintaining the highest standards of performance and reliability.
