---
description: Read on setting up widget
---

# Widget v1.0

To initialize the widget, you will need to first generate a Widget Token, which will only be valid for a certain time period. You will need to pass this token along with the Sender ID to widget initialization snippets.

**`GET /v2/senders/{senderId}/widget-token`**

ParametersRequest SampleResponse Sample

{% tabs %}
{% tab title="Parameters" %}
| **Field**       | **Required** | **Type** | **Description**     |
| --------------- | ------------ | -------- | ------------------- |
| token           | Response     | String   | Token Details       |
| senderId        | Yes          | UUID     | User ID             |
| expiry\_minutes | Response     | Numeric  | Token validity time |
{% endtab %}

{% tab title="Request Sample" %}
```
curl -X GET {{url}}/v2/senders/{senderId}/widget-token \
  -H 'Accept: application/json' \
  -H 'X-Client-Id: clientid' \
  -H 'X-Client-Secret: clientsecret'
```
{% endtab %}

{% tab title="Response Sample" %}
```
{
  "token": "string",
  "sender_id": "UUID",
  "expiry_minutes": 60
}
```
{% endtab %}
{% endtabs %}

Once you have the token, you will need to follow the steps mentioned below to setup the widget v1.0.

## 1. **Include the Widget Script** <a href="#id-1.-include-the-widget-script" id="id-1.-include-the-widget-script"></a>

```
<script src="https://api.sandbox.raas.hubcrossborder.com/v2/widget/widget.js" charset="utf-8"></script>
```

## 2. **Create a div where widget needs to be placed** <a href="#id-2.-create-a-div-where-widget-needs-to-be-placed" id="id-2.-create-a-div-where-widget-needs-to-be-placed"></a>

```
<div id="widget-root"></div>
```

## 3. **Initialize the Widget** <a href="#id-3.-initialize-the-widget" id="id-3.-initialize-the-widget"></a>

The properties required while initializing the widget depends on the type of widget. Details are listed below along with a sample.

{% tabs %}
{% tab title="KYC" %}
| **Name**   | **Required** | **Type**                                        | **Description**                                                                                                                                                |
| ---------- | ------------ | ----------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| elementId  | Yes          | string                                          | Target element where widget will be rendered.                                                                                                                  |
| senderId   | Yes          | string                                          | senderId for whom the widget should be rendered.                                                                                                               |
| token      | Yes          | string                                          | Unique Token that needs to be generated using Generate Widget Token API.                                                                                       |
| type       | Yes          | string                                          | kyc                                                                                                                                                            |
| width      | Yes          | Standard Size Dimension eg. 100px, 100%, 100em  | Width of widget                                                                                                                                                |
| height     | Yes          | Standard Size Dimension e.g. 100px, 100%, 100em | Height of widget                                                                                                                                               |
| locale     | No           | String                                          | By default, this is set to English, For Korean pass ko and for English pass en. If left blank or invalid locale is sent it will fall back to default language. |
| stylesheet | No           | URL                                             | This stylesheet will override the default design of the widget.                                                                                                |
| multiStep  | No           | boolean(true or false)                          | Configuration to display the widget in a single or multiple form                                                                                               |
{% endtab %}

{% tab title="Bank" %}
| **Name**   | **Required** | **Type**                                        | **Description**                                                                                                        |
| ---------- | ------------ | ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| elementId  | Yes          | string                                          | Target element where widget will be rendered.                                                                          |
| senderId   | Yes          | string                                          | senderId for whom the widget should be rendered.                                                                       |
| token      | Yes          | string                                          | Unique Token that needs to be generated using Generate Widget Token API.                                               |
| type       | Yes          | string                                          | bank                                                                                                                   |
| width      | Yes          | Standard Size Dimension eg. 100px, 100%, 100em  | Width of widget                                                                                                        |
| height     | Yes          | Standard Size Dimension e.g. 100px, 100%, 100em | Height of widget                                                                                                       |
| bankId     | Conditional  | string                                          | <p>Required when user login credentials needs to be updated.</p><p>bankId for which credential needs to be updated</p> |
| stylesheet | No           | URL                                             | This stylesheet will override the default design of the widget.                                                        |
{% endtab %}

{% tab title="Card" %}
| **Name**   | **Required** | **Type**                                        | **Description**                                                          |
| ---------- | ------------ | ----------------------------------------------- | ------------------------------------------------------------------------ |
| elementId  | Yes          | string                                          | Target element where widget will be rendered.                            |
| senderId   | Yes          | string                                          | senderId for whom the widget should be rendered.                         |
| token      | Yes          | string                                          | Unique Token that needs to be generated using Generate Widget Token API. |
| type       | Yes          | string                                          | card                                                                     |
| width      | Yes          | Standard Size Dimension eg. 100px, 100%, 100em  | Width of widget                                                          |
| height     | Yes          | Standard Size Dimension e.g. 100px, 100%, 100em | Height of widget                                                         |
| stylesheet | No           | URL                                             | This stylesheet will override the default design of the widget.          |
{% endtab %}

{% tab title="Tier" %}
| **Name**   | **Required** | **Type**                                        | **Description**                                                          |
| ---------- | ------------ | ----------------------------------------------- | ------------------------------------------------------------------------ |
| elementId  | Yes          | string                                          | Target element where widget will be rendered.                            |
| senderId   | Yes          | string                                          | senderId for whom the widget should be rendered.                         |
| token      | Yes          | string                                          | Unique Token that needs to be generated using Generate Widget Token API. |
| type       | Yes          | string                                          | tier                                                                     |
| width      | Yes          | Standard Size Dimension eg. 100px, 100%, 100em  | Width of widget                                                          |
| height     | Yes          | Standard Size Dimension e.g. 100px, 100%, 100em | Height of widget                                                         |
| stylesheet | No           | URL                                             | This stylesheet will override the default design of the widget.          |
{% endtab %}

{% tab title="3ds" %}
| **Name**      | **Required** | **Type**                                        | **Description**                                                          |
| ------------- | ------------ | ----------------------------------------------- | ------------------------------------------------------------------------ |
| elementId     | Yes          | string                                          | Target element where widget will be rendered.                            |
| senderId      | Yes          | string                                          | senderId for whom the widget should be rendered.                         |
| token         | Yes          | string                                          | Unique Token that needs to be generated using Generate Widget Token API. |
| type          | Yes          | string                                          | 3ds                                                                      |
| width         | Yes          | Standard Size Dimension eg. 100px, 100%, 100em  | Width of widget                                                          |
| height        | Yes          | Standard Size Dimension e.g. 100px, 100%, 100em | Height of widget                                                         |
| transactionId | Yes          | String                                          | Required for 3DS verification                                            |
| stylesheet    | No           | URL                                             | This stylesheet will override the default design of the widget.          |
{% endtab %}

{% tab title="Invoice" %}
| **Name**      | **Required** | **Type**                                        | **Description**                                                          |
| ------------- | ------------ | ----------------------------------------------- | ------------------------------------------------------------------------ |
| elementId     | Yes          | string                                          | Target element where widget will be rendered.                            |
| senderId      | Yes          | string                                          | senderId for whom the widget should be rendered.                         |
| token         | Yes          | string                                          | Unique Token that needs to be generated using Generate Widget Token API. |
| type          | Yes          | string                                          | invoice                                                                  |
| width         | Yes          | Standard Size Dimension eg. 100px, 100%, 100em  | Width of widget                                                          |
| height        | Yes          | Standard Size Dimension e.g. 100px, 100%, 100em | Height of widget                                                         |
| transactionId | Yes          | string                                          | transactionId when initializing the Invoice widget                       |
| stylesheet    | No           | URL                                             | This stylesheet will override the default design of the widget.          |
{% endtab %}
{% endtabs %}

```
<script>
  var widget = new MachnetWidget({
    elementId: "widget-root",
    senderId: "62018cb0-e183-4a72-a691-844c213a693a",
    width: "100%",
    height: "200px",
    type: "kyc",
    locale: "ko",
    multiStep: true,
    stylesheet: "https://example.com/mystyle.css",
    token: "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzZW5kZXJJZCI6Ik1qaGtZbVU1WkRBdFpURXlNQzAwTkRreExXSTRNREl0WVRCak16ZGlaV1EzWTJKbFxyXG4iLCJleHAiOjE1NjE2NDExNTcsIm10b0lkIjoiT1FcclxuIn0.f0yKZUt35VC_Mo2D8vk0Cbc_D-c2zAv9MAtmQi2zah8",
  });
  widget.init();
</script>
```

{% hint style="info" %}
**Applying Custom CSS to Widget**

You need to create a CSS file based on the DOM structure and then create a stylesheet based on the required changes. If you load the stylesheet during the widget initialization, the changes will be reflected accordingly.
{% endhint %}

## 4. Add an Event Handler <a href="#id-4.-add-an-event-handler" id="id-4.-add-an-event-handler"></a>

Widget will trigger an event anytime there is changes in a resource. To listen to the events generated by the widget include below code snippet.

```
window.addEventListener("message", function (e) {
  <!--Here e.data holds the information about the event triggered. 
     e.data.type    = kyc || bank || tier
     e.data.status  = eventName
     e.data.message = additional message

 -->
  if(e.data.status === 'eventName'){
    //your action
  }
});
```

| Event Name              | Widget Type | Description                                                                                                                                           |
| ----------------------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| VERIFIED                | kyc         | KYC process is completed and user is verified.                                                                                                        |
| RETRY                   | kyc         | The user is moved to KYC retry status. Additional KYC information is required from the sender.                                                        |
| IN\_PROGRESS            | kyc         | User kyc has been submitted for verification. Their status will be updated once verification process is completed.                                    |
| DOCUMENT\_REQUESTED     | kyc         | User needs to provide additional KYC documentation for verification.                                                                                  |
| RETRY\_REQUESTED        | kyc         | User needs to review the submitted information and re-submit it.                                                                                      |
| REVIEW\_PENDING         | kyc         | User has submitted KYC information but is under compliance review.                                                                                    |
| SUSPENDED               | kyc         | The user has been suspended. User is unable to conduct any activity on the platform.                                                                  |
| BANK\_ADDED             | bank        | Sender’s bank addition process has been successfully completed.                                                                                       |
| BANK\_ERROR             | bank        | Error while adding sender’s bank. Please try again.                                                                                                   |
| EXIT\_EVENT             | bank        | Sender's has closed the bank widget.                                                                                                                  |
| CARD\_ADDED             | card        | Sender’s card addition process has been successfully completed.                                                                                       |
| ADD\_LIMIT\_EXCEEDED    | card        | User has exceeded the 7 day or 30 day card add attempt limit. To add a new card, user will need to wait until the appropriate time period has passed. |
| CARD\_ERROR             | card        | Error while adding sender’s card. Please try again.                                                                                                   |
| TIER2\_REQUESTED        | tier        | Tier 2 request sent successfully. User’s tier will be updated once the request has been reviewed by compliance.                                       |
| TIER3\_REQUESTED        | tier        | Tier 3 request sent successfully. User’s tier will be updated once the request has been reviewed by compliance.                                       |
| TIER\_REQUEST\_FAILED   | tier        | Error while upgrading sender’s tier. Please try again.                                                                                                |
| THREE\_DS\_VERIFIED     | 3ds         | 3DS verification successful.                                                                                                                          |
| THREE\_DS\_HOLD         | 3ds         | 3DS verification is on hold and hence, transaction is on hold as well. Please contact Machnet customer support.                                       |
| THREE\_DS\_FAILED       | 3ds         | 3DS verification is unsuccessful. Transaction is cancelled.                                                                                           |
| THREE\_DS\_IN\_PROGRESS | 3ds         | 3DS verification is in progress.                                                                                                                      |
