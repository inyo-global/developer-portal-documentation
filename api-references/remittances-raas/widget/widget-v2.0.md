---
description: Read on setting up widget
---

# Widget v2.0

It is required to use widget v2.0 to provide the KYC information which are sensitive and cannot be updated via KYC API. You need to follow the steps mentioned below to initialize and setup the widget v2.0.

To initialize the widget, you will need to first generate a Widget Token, which will only be valid for a certain time period. You will need to pass this token along with the Sender ID to widget initialization snippets.

**`GET /v2/senders/{senderId}/widget-token`**

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

## 1. Include the Widget Script <a href="#id-1.-include-the-widget-script" id="id-1.-include-the-widget-script"></a>

```
<script src="https://sandbox.api.machpay.com/v2/widget/widget.js" charset="utf-8"></script>
```

## 2. Create a div where widget needs to be placed. <a href="#id-2.-create-a-div-where-widget-needs-to-be-placed" id="id-2.-create-a-div-where-widget-needs-to-be-placed"></a>

```
<div id="widget-root"></div>
```

## 3. Initialize the Widget <a href="#id-3.-initialize-the-widget" id="id-3.-initialize-the-widget"></a>

```
<script>
  var widget = new MachnetWidget({
    elementId: "widget-root",
    width: "100%",
    height: "200px",
    type: "kyc",
    locale: "en",
    version: '2.0',
    fields: 'senderIdInfo|senderFullSSN',
    stylesheet: "https://example.com/mystyle.css",
    senderId: "62018cb0-e183-4a72-a691-844c213a693a",
    token: "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzZW5kZXJJZCI6Ik1qaGtZbVU1WkRBdFpURXlNQzAwTkRreExXSTRNREl0WVRCak16ZGlaV1EzWTJKbFxyXG4iLCJleHAiOjE1NjE2NDExNTcsIm10b0lkIjoiT1FcclxuIn0.f0yKZUt35VC_Mo2D8vk0Cbc_D-c2zAv9MAtmQi2zah8",
  });
  widget.init();
</script>
```

| Property Name | Required | Type                                            | Description                                                                                                                                                            |
| ------------- | -------- | ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| elementId     | Yes      | String                                          | Target element where widget will be rendered.                                                                                                                          |
| senderId      | Yes      | String                                          | `senderId` for whom the widget should be rendered.                                                                                                                     |
| width         | Yes      | Standard Size Dimension eg. 100px, 100%, 100em  | Width of widget                                                                                                                                                        |
| height        | Yes      | Standard Size Dimension e.g. 100px, 100%, 100em | Height of widget                                                                                                                                                       |
| type          | Yes      | String                                          | Type of Widget. `kyc` Widget v2.0 supports `kyc` widget type only.                                                                                                     |
| locale        | No       | String                                          | By default, this is set to English, For Korean pass **ko** and for English pass **en**. If left blank or invalid locale is sent it will fall back to default language. |
| stylesheet    | No       | URL                                             | This stylesheet will override the default design of the widget.                                                                                                        |
| token         | Yes      | String                                          | Unique Token that needs to be generated using Generate Widget Token API.                                                                                               |
| version       | Yes      | String                                          | 1.0 is the default value. To use version 2.0 please include the version number in the script.                                                                          |
| fields        | Yes      | String                                          | The kyc fields provided will be rendered by the widget. To render multiple fields, fields will be concatenated and delimited by a pipe '\|' character.                 |

## 4. Add an Event Handler <a href="#id-4.-add-an-event-handler" id="id-4.-add-an-event-handler"></a>

Widget will trigger an event anytime there is changes in a resource. To listen to the events generated by the widget include below code snippet.

```
window.addEventListener("message", function (e) {
  <!--Here e.data holds the information about the event triggered. 
     e.data.type    = kyc
     e.data.status  = eventName
     e.data.message = additional message

 -->
  if(e.data.status === 'eventName'){
    //your action
  }
});
```

| Event Name                             | Widget Type | Description                                               |
| -------------------------------------- | ----------- | --------------------------------------------------------- |
| KYC FIELD FOUR\_DIGIT\_SSN SUBMITTED   | kyc         | Four digit SSN has been submitted by the user.            |
| KYC FIELD FULL\_DIGIT\_SSN SUBMITTED   | kyc         | Full SSN has been submitted by the user.                  |
| KYC FIELD ID\_INFO SUBMITTED           | kyc         | ID details have been submitted by the user.               |
| KYC FIELD PROOF\_OF\_ADDRESS SUBMITTED | kyc         | Proof of address document has been submitted by the user. |
| KYC FIELD SOURCE\_OF\_FUND SUBMITTED   | kyc         | Source of fund details have been submitted by the user.   |
