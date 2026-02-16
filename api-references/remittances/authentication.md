# Authentication

Inyo uses API keys to securely control access to its APIs. Our servers are protected by strict firewall rules and can only be accessed using valid credentials.

Depending on the product you’re integrating with, Inyo will provide a specific set of keys. In a typical setup, you will receive:

* x-api-key: Primary API key for accessing general endpoints
* tenant: Identifier for your Inyo installation
* x-agent-id: UUID of the agent used to create quotes, process transactions, and more
* x-agent-api-key: API key associated with the agent above

All keys must be included in the HTTP request headers. Additionally, be sure to set the following headers:

```http
Content-Type: application/json
Accept: application/json
```

Our endpoints:

<table><thead><tr><th width="235.46875">Resource</th><th>URL</th></tr></thead><tbody><tr><td>Core API - sandbox</td><td>api.sandbox.hubcrossborder.com</td></tr></tbody></table>
