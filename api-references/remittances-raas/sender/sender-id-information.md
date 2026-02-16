# Sender ID Information

Clients need to collect the sender’s ID information to successfully process and deliver a transaction in certain corridors. **This may be required regardless of the current tier of the sender and is only required if not collected as per the tier requirement of the sender.**

If ID information is required for a specific corridor but is not provided, the transaction’s delivery will be placed on HOLD. In order to avoid such cases, it is recommended to either collect the ID information of senders during the KYC process or before they create a transaction to the specific corridors.

You can use the below API to collect the ID information of registered senders. ID information includes the listed fields. However, the exact requirements for your service will be outlined in your spec sheet.

**`PUT {{url}}/v2/senders/{{senderId}}/documents`**

{% tabs %}
{% tab title="Details" %}
| **Field**              | **Required** | **Type** | **Description**                                                                          |
| ---------------------- | ------------ | -------- | ---------------------------------------------------------------------------------------- |
| senderId               | Yes, in URL  | UUID     | Unique ID of the sender                                                                  |
| identification\_number | Yes          | String   | Identification number for the ID document                                                |
| id\_issuing\_authority | Yes          | String   | 2 char state code of the ID issuing authority’s state                                    |
| document\_type         | Yes          | Enum     | <p>Type of ID document.</p><p>Enumerated values: DRIVING_LICENCE, PASSPORT, STATE_ID</p> |
| issue\_date            | No           | Date     | ID issued date (yyyy-mm-dd)                                                              |
| expiry\_date           | Yes          | Date     | ID expiry date (yyyy-mm-dd)                                                              |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X PUT {{url}}/v2/senders/{{senderId}}/documents \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret' \
  -d { \
  "identification_number": "123456789",
  "id_issuing_authority": "AL",
  "document_type": "PASSPORT",
  "expiry_date": "2022-12-07",
  "issue_date": "2022-01-01"
}
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
    "id": "{{document_unique_id}}",
    "document_type": "PASSPORT",
    "identification_number": "123456789",
    "id_issuing_authority": "AL",
    "issue_date": "2022-01-01",
    "expiry_date": "2022-12-07"
}
```
{% endtab %}
{% endtabs %}
