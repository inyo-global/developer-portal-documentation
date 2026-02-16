# Uploading Documents

To advance a sender to higher compliance levels (Level 2 and above), you may need to upload identity documents and proof of source of funds. These documents are reviewed by the Inyo compliance team.

***

### Document Types

#### Identity Documents (Document ID)

Government-issued identification uploaded for KYC verification.

**Endpoint:** `POST /organizations/{tenant}/people/{personId}/documents/documentId/{subtype}/upload`\
**Authentication:** Tenant-level (`x-api-key`)

**Supported Subtypes:**

| Subtype | Description |
| ------- | ----------- |
| `FRONT` | Front side of the document |
| `BACK` | Back side of the document |

**Accepted document types** (configured per tenant):
* Driver's License
* Passport
* National ID Card
* State ID

#### Source of Funds

Proof of the sender's source of funds, required for higher compliance tiers.

**Endpoint:** `POST /organizations/{tenant}/people/{personId}/documents/sourceOfFunds/{subtype}/upload`\
**Authentication:** Tenant-level (`x-api-key`)

**Supported Subtypes:**

| Subtype | Description |
| ------- | ----------- |
| `FRONT` | Primary document (e.g., bank statement, pay stub) |

***

### Upload Request

Documents are uploaded as **multipart/form-data** with the file attached.

```bash
# Upload front of driver's license
curl --request POST \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/people/$PERSON_ID/documents/documentId/FRONT/upload \
  --header "x-api-key: $API_KEY" \
  --form "file=@/path/to/drivers-license-front.jpg"
```

```bash
# Upload back of driver's license
curl --request POST \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/people/$PERSON_ID/documents/documentId/BACK/upload \
  --header "x-api-key: $API_KEY" \
  --form "file=@/path/to/drivers-license-back.jpg"
```

```bash
# Upload source of funds
curl --request POST \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/people/$PERSON_ID/documents/sourceOfFunds/FRONT/upload \
  --header "x-api-key: $API_KEY" \
  --form "file=@/path/to/bank-statement.pdf"
```

***

### Document Verification Status

After uploading, documents are queued for review by the compliance team. You can track the verification status:

#### Get Current Verification Status

**Endpoint:** `GET /organizations/{tenant}/documents/{documentId}/status`\
**Authentication:** Tenant-level

```bash
curl --request GET \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/documents/$DOCUMENT_ID/status \
  --header "x-api-key: $API_KEY"
```

#### Get Verification Status History

**Endpoint:** `GET /organizations/{tenant}/documents/{documentId}/status/history`\
**Authentication:** Tenant-level

```bash
curl --request GET \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/documents/$DOCUMENT_ID/status/history \
  --header "x-api-key: $API_KEY"
```

#### List Person Documents by Status

**Endpoint:** `GET /organizations/{tenant}/people/{personId}/documents`\
**Authentication:** Tenant-level

```bash
curl --request GET \
  --url https://api.sandbox.inyoplatform.com/organizations/$TENANT/people/$PERSON_ID/documents \
  --header "x-api-key: $API_KEY"
```

***

### Verification Statuses

| Status | Description |
| ------ | ----------- |
| `Pending` | Document uploaded, awaiting review |
| `Verified` | Document accepted — compliance level may upgrade |
| `NotVerified` | Document rejected — see reason and re-upload |
| `NotReadable` | Document image is unclear — request a better scan |
| `TemplateIdentified` | Document type recognized but verification in progress |

***

### Webhook Notifications

Register for the `documentUpdatedEvents` webhook to receive real-time notifications when a document's verification status changes. See [Webhooks](../webhooks.md) for setup instructions.

***

### Best Practices

* **Upload high-quality images** — blurry or cropped images will be rejected as `NotReadable`.
* **Upload both sides** when required — for documents like driver's licenses, upload both `FRONT` and `BACK`.
* **Check compliance level after verification** — once a document is `Verified`, call `GET /participants/{id}/complianceLevels` to see if the sender has been upgraded.
* **Handle rejections gracefully** — prompt the user to re-upload with clear instructions about what went wrong.
* **Use webhooks** instead of polling — register for `documentUpdatedEvents` to be notified immediately when a document is reviewed.

***

### Test Data

For sandbox testing, use the test data provided in [Test Data](test-data.md). Certain name/address combinations trigger specific compliance behaviors (approval, hold, rejection).

[Interactive API Documentation — Document ID](https://dev-api.inyoglobal.com/sandbox/#tag/account_Person-Documents-Resource/paths/~1organizations~1%7Btenant%7D~1people~1%7BpersonId%7D~1documents~1documentId~1%7Bsubtype%7D~1upload/post)\
[Interactive API Documentation — Source of Funds](https://dev-api.inyoglobal.com/sandbox/#tag/account_Person-Documents-Resource/paths/~1organizations~1%7Btenant%7D~1people~1%7BpersonId%7D~1documents~1sourceOfFunds~1%7Bsubtype%7D~1upload/post)
