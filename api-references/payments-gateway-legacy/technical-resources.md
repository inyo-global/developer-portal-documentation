# Technical Resources

Prior to start integrating and testing, it's necessary to have three components in hand, that needs to be provided by our team.

<table><thead><tr><th width="216">Key</th><th width="285">Sample value</th><th>Used for</th></tr></thead><tbody><tr><td>public key</td><td>a23271e1-c1c0-44d3-4567-5610217d1151</td><td>Tokenizing cards using the javascript library</td></tr><tr><td>client id</td><td>MY_CLIENT_ID</td><td>client id to be used during the oauth authentication</td></tr><tr><td>client secret</td><td>4efa3460-a121-1ca9-abcd-e1345ff5510d</td><td>client secret to be used during the oauth authentication</td></tr></tbody></table>

It's also important to take note of the following resources:

<table><thead><tr><th width="315">Resource</th><th>URL</th></tr></thead><tbody><tr><td>Static javascript component</td><td><a href="https://cdn.simpleps.com/production/inyo.js">https://cdn.simpleps.com/production/inyo.js</a></td></tr><tr><td>OpenAPI specs</td><td><a href="https://sandbox-gw.simpleps.com/q/swagger-ui/#/">https://sandbox-gw.simpleps.com/q/swagger-ui</a></td></tr></tbody></table>

<table><thead><tr><th width="215">Resource</th><th width="303">Sandbox</th><th>Production</th></tr></thead><tbody><tr><td>API endpoint</td><td><a href="https://sandbox-gw.simpleps.com/">https://sandbox-gw.simpleps.com/</a></td><td><a href="https://sandbox-gw.simpleps.com/">https://gw.simpleps.com/</a></td></tr><tr><td>Postman</td><td>Upon request</td><td></td></tr></tbody></table>

Considerations: During the key generation, it will be required by our team the url of the origin server used for development and production. This origin url will ensure that communications in between the browser and our servers are allowed, per [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) rules.
