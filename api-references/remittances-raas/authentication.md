# Authentication

Inyo uses API keys to allow access to our APIs. Our servers are restricted by firewall rules and can only be accessed once credentials are generated.

Depending on the type of product being used, Inyo's team will provide a set of different keys: CLIENT\_ID and CLIENT\_SECRET.

For the **Payout product**, you must use X-Client-Id and X-Client-Secret in every API request header.

{% hint style="info" %}
CLIENT\_ID and CLIENT\_SECRET values should never be used or shared publicly and should be stored properly in an encrypted format.
{% endhint %}

X-Client-Id: CLIENT\_ID\
X-Client-Secret: CLIENT\_SECRET

You will have to use the following URL to access our sandbox: https://api.sandbox.raas.hubcrossborder.com/v2

Use our Postman collection to quickly run through our API endpoint

[![Run In Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/14929752-e700efb0-e365-4cb9-a9c7-6fc394ce80be?action=collection%2Ffork\&source=rip_markdown\&collection-url=entityId%3D14929752-e700efb0-e365-4cb9-a9c7-6fc394ce80be%26entityType%3Dcollection%26workspaceId%3D7f3582e5-242e-45c1-9b7c-dcfc5772223e#?env%5BRaaS%20Sandbox%20Environment%5D=W3sia2V5IjoidXJsIiwidmFsdWUiOiJodHRwczovL3NhbmRib3guYXBpLm1hY2hwYXkuY29tL3YyIiwiZW5hYmxlZCI6dHJ1ZSwic2Vzc2lvblZhbHVlIjoiaHR0cHM6Ly9zYW5kYm94LmFwaS5tYWNocGF5LmNvbS92MiIsInNlc3Npb25JbmRleCI6MH0seyJrZXkiOiJjbGllbnRfaWQiLCJ2YWx1ZSI6IiIsImVuYWJsZWQiOnRydWUsInR5cGUiOiJkZWZhdWx0Iiwic2Vzc2lvblZhbHVlIjoiIiwic2Vzc2lvbkluZGV4IjoxfSx7ImtleSI6ImNsaWVudF9zZWNyZXQiLCJ2YWx1ZSI6IiIsImVuYWJsZWQiOnRydWUsInR5cGUiOiJkZWZhdWx0Iiwic2Vzc2lvblZhbHVlIjoiIiwic2Vzc2lvbkluZGV4IjoyfSx7ImtleSI6InNlbmRlcklkIiwidmFsdWUiOiIiLCJlbmFibGVkIjp0cnVlLCJ0eXBlIjoiZGVmYXVsdCIsInNlc3Npb25WYWx1ZSI6IiIsInNlc3Npb25JbmRleCI6M30seyJrZXkiOiJ0cmFuc2FjdGlvbklkIiwidmFsdWUiOiIiLCJlbmFibGVkIjp0cnVlLCJ0eXBlIjoiZGVmYXVsdCIsInNlc3Npb25WYWx1ZSI6IiIsInNlc3Npb25JbmRleCI6NH0seyJrZXkiOiJyZWNpcGllbnRJZCIsInZhbHVlIjoiIiwiZW5hYmxlZCI6dHJ1ZSwidHlwZSI6ImRlZmF1bHQiLCJzZXNzaW9uVmFsdWUiOiIiLCJzZXNzaW9uSW5kZXgiOjV9LHsia2V5IjoidHlwZSIsInZhbHVlIjoiTElOSyIsImVuYWJsZWQiOnRydWUsInR5cGUiOiJkZWZhdWx0Iiwic2Vzc2lvblZhbHVlIjoiTElOSyIsInNlc3Npb25JbmRleCI6Nn1d)<br>
