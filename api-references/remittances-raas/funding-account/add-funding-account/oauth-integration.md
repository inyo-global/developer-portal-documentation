# OAuth Integration

## Introduction <a href="#introduction" id="introduction"></a>

Starting January 2021, our bank verification partner has entered into data access agreements with Chase, Capital One, JP Morgan Chase, Wells Fargo, Bank of America, and US Bank to start migrating into new financial access channels. As a part of this agreement, the bank verification will need to be done using an OAuth integration. This is an industry-standard authentication protocol for data sharing, which will give users increased control and transparency while improving overall connection stability.

## Overview of Bank Add Changes <a href="#overview-of-bank-add-changes" id="overview-of-bank-add-changes"></a>

In general, there are two ways to handle bank API: OAuth and non-OAuth. The existing bank add flow utilizes a non-OAuth flow and the user stays within the bank widget throughout the authentication and authorization process.

However, the OAuth flow requires the user to authenticate their account on their bank’s website. This means that the user will begin the bank add process from the existing widget however, they will be temporarily redirected to their Bank’s website to login and give permission after which, they are brought back to the bank add widget to complete the flow.

Banks Currently supporting OAuth Flow

1. Chase
2. Capital One
3. Wells Fargo
4. Bank of America
5. USAA
6. U.S. Bank
7. Robinhood

## Supporting OAuth Banks <a href="#supporting-oauth-banks" id="supporting-oauth-banks"></a>

In order to support OAuth banks, your work will mostly be limited to your front-end bank widget integration. Not all banks will support OAuth flow and you will only be able to tell whether a bank follows OAuth or Non-OAuth flow after the user has selected their bank. Therefore, we suggest you make the changes outlined below regardless of the banks you want to support.

<figure><img src="https://raas.docs.machnetinc.com/~gitbook/image?url=https%3A%2F%2F3520691358-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FizjNAsoBHHB2hfZJyPhr%252Fuploads%252FIRfphRR3lxxA2tACEGl1%252FOAuth%2520flow.jpg%3Falt%3Dmedia%26token%3D5c0b0942-4698-420a-aacd-f649ec39315e&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=96ae1b7&#x26;sv=1" alt=""><figcaption><p>OAuth and Non-OAuth Bank integrations</p></figcaption></figure>

{% hint style="warning" %}
The below integration guidelines are recommendations and the best integration for your service may vary.
{% endhint %}

### Widget for Web <a href="#widget-for-web" id="widget-for-web"></a>

{% hint style="info" %}
For non-OAuth bank, all events will need to be completed in Inyo's widget.
{% endhint %}

#### **Add an OAuth bank**

1. User opens the bank widget
2. User selects a bank that requires OAuth login
3. Users will be redirected to the bank website.
4. Once the authorization is completed, the user is redirected back to the widget iframe. All redirections will be handled by the widget itself.
5. OAuth bank relogin will be similar to re-login of other banks. Please refer to the [bank re-login section below](oauth-integration.md#oauth-bank-relogin) for additional details.

### Widget for mobile application <a href="#widget-for-mobile-application" id="widget-for-mobile-application"></a>

{% hint style="info" %}
For non-OAuth bank, all events will need to be completed in the widget in webview.
{% endhint %}

You need to host the widget separately in a web server as a web application. The hosted page will receive all the necessary parameters through query param ie. token, senderId etc. This integration recommendation will require the following steps.

1. Create a web page for the widget. A sample code is [here](oauth-integration.md#sample-code-and-steps-for-integration).
2. The web page will have an index.html page. Index page will contain the bank widget and deep link URL in apps.
3. Index page should receive all the necessary parameters through query parameters. As of now, we have set up the following parameters in the [sample snippet](oauth-integration.md#sample-code-and-steps-for-integration).
   1. token: widget token received from Machnet
   2. senderId: sender reference id received from Machnet
   3. brand: your brand name
   4. bankRelogin: true is bank re-login is required
   5. bankId: sender bank id inorder to load bank re-login widget

Note: You can add any other necessary params as per your requirement. A recommended way to pass query parameters is to hash or encode base64 query param values for security reasons.

1. While adding bank from the mobile app (iOS and android),
   1. User clicks on add bank
   2. User needs to be redirected to an external browser with the widget page link and necessary parameters.

{% hint style="info" %}
Example

If the widget web page is hosted on [https://widget.example.com](https://widget.example.com/), the apps should redirect users to [https://widget.example.com?token=\<Base64Encoded-token-value>\&senderId=\<Base64Encoded-sender-id-value>\&brand=\<brand-name>](https://widget.example.com/?token=) in an external browser.
{% endhint %}

1. Once the user completes adding a bank from the widget web page, users will be displayed with a success message and “Goto App” button.
2. On the mobile app, developers need to handle the deep link using app schemes. Whenever the user clicks on the “Goto App” button, the user should be returned to the same state where the user left the app.
3. Once a user is returned to the app, the [bank list needs to be re-fetched](../get-funding-account.md) so the user can see the newly added bank.

### **Sample code and steps for integration**

**Creating widget web page**

1. Create index.html file with following code.

```
<!DOCTYPE html>
<html lang="en">


<head>
   <title>Widget</title>
   <meta name="viewport" content="width=device-width, initial-scale=1.0" />
   <style>
       * {
           margin: 0;
           padding: 0;
           box-sizing: border-box;
       }


       body {
           background-color: white;
       }


       header {
           background-color: #fff;
           width: 100%;
       }


       .navbar {
           margin: auto;
           padding: 10px;
           display: flex;
           align-items: center;
       }


       @media (max-width: 400px) {
           .navbar {
               margin-bottom: 0;
           }
       }


       .logo {
           width: 150px;
       }


       .logo img {
           width: 100%;
       }


       .navbar h1 {
           font-size: 16px;
           font-family: arial;
           font-weight: 400;
           border-left: 2px solid #000;
           padding-left: 20px;
           margin-left: 8px;
       }


       .bank-added-message {
           display: none;
           padding: 40px 15px;
           font-family: Arial, Helvetica, sans-serif;
           background-color: #fff;
           border-top: 1px solid #e4e4e4;
           height: calc(100vh - 74px);
       }


       .bank-added-message p {
           margin-bottom: 20px;
       }


       .btn {
           width: 100%;
           padding: 10px;
       }
   </style>


</head>


<body>
   <header>
       <nav class="navbar">
           <div class="logo">
               <img src=".....logo" />
           </div>
           <h1>Add Bank</h1>
       </nav>
   </header>
   <div id="widget-root"></div>
   <div class="bank-added-message" id="bank-add-success">
       <p>
           Your bank has been added successfully. You can now return to the
           <span id="brand"></span> app to proceed with creating your
           transaction.
       </p>
       <button class="btn" id="goto-app">Goto App</button>
   </div>
   <script src="https://sandbox.api.machpay.com/v2/widget/widget.js" charset="utf-8"></script>
   <script>
       document.addEventListener("DOMContentLoaded", function (event) {


           const params = new URLSearchParams(location.search);
           const widgetWidth = window.innerWidth < 400 ? "100%" : "";
           const widgetHeight = window.innerHeight - 70;


           if (
              !params.has("senderId") ||
              !params.has("token") ||
              !params.has("affiliate") ||
              (params.has("bankRelogin") &&
                params.get("bankRelogin") &&
                !params.has("bankId"))
            ) {
                throw new Error("Missing required parameter");
            }


           let widgetConfig = {
       elementId: "widget-root",
       width: widgetWidth,
       height: widgetHeight,
       type: "bank",
       locale: params.has("locale") ? params.get("locale") : "en",
       version: "1.0",
       senderId: atob(params.get("senderId")),
       token: atob(params.get("token")),
       bankId: params.get("bankId") ? atob(params.get("bankId")) : "",
     };


     launchWidget();


     function launchWidget() {
        widgetConfig.isRedirect = false;
        let widget = new MachnetWidget(widgetConfig);
        widget.init();
     }


     function handleBankEvent(event) {
       if (event.data.status === "BANK_ADDED") {
         showGotoApp();
     }
 }


 window.addEventListener("message", handleBankEvent);


 const goToAppButton = document.getElementById("goto-app");
 goToAppButton.addEventListener("click", function () {
   if (/(iPhone|iPad|iPod)/.test(navigator.userAgent)) {
     window.location.href = CUSTOM_URL_SCHEME + "://";
   } else {
     window.location.href =
       "intent://" +
       CUSTOM_URL_SCHEME +
       "/#Intent;scheme=" +
       CUSTOM_URL_SCHEME +
       ";package=" +
       PACKAGE_NAME +
       ";end";
   }
 });
});


function showGotoApp() {
 let widget = document.getElementById("widget-root");
 widget.style.display = "none";


 affiliateName.innerHTML = params.get("BRAND_NAME");


 const goToAppButton = document.getElementById("goto-app");


 const successMessage = document.getElementById("bank-add-success");
 successMessage.style.display = "block";
}       
 </script>
</body>
</html>
```

2. Host these page in web server.
3. With this setup the widget web page is ready for integration in the app.

{% hint style="info" %}
Example

[https://widget.example.com?token=\<Base64Encoded-token-value>\&senderId=\<Base64Encoded-sender-id-value>\&brand=\<brand-name>](https://widget.example.com/?token=)
{% endhint %}

## **Integrating widget app in the apps**

For the integration we need to handle web events and deep linking using App Url schemes. To set up Deep Linking in an app, additional configuration is required in Xcode for iOS and Android Studio for Android. Outlined below are the steps.

### **iOS**

**Adding App Url Scheme and Identifier**

* Open your Xcode project and go to the "Info" tab.
* Then go to the very bottom and click the plus (+) sign under URL Types. Make sure to add url-scheme as your new Identifier and specify URL Schemes as url-scheme.
* In the "URL Schemes" section, enter a unique string that will identify your app. This string should be all lowercase and may include numbers and hyphens, but no spaces or special characters. In the identification and url schemes field, enter your unique app scheme.

<figure><img src="https://raas.docs.machnetinc.com/~gitbook/image?url=https%3A%2F%2F3520691358-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FizjNAsoBHHB2hfZJyPhr%252Fuploads%252F3QBLejcgmUeBnDuw62co%252Fios.jpg%3Falt%3Dmedia%26token%3D15d2cf91-6b0a-4bf9-9c40-69805ce25c74&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=70ca1e89&#x26;sv=1" alt=""><figcaption></figcaption></figure>

* Add new line in podfile.&#x20;

```
pod 'React-RCTLinking', :path => '../node_modules/react-native/Libraries/LinkingIOS'
```

* On your appDelegate.m file import the following.

```
#import <React/RCTLinkingManager.h>
```

* Insert the following in appDelegate.m file.

```
- (BOOL)application:(UIApplication *)application
openURL:(NSURL *)url
options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
{
  return [RCTLinkingManager application:application openURL:url options:options];


- (BOOL)application:(UIApplication *)application continueUserActivity:(nonnull NSUserActivity *)userActivity
restorationHandler:(nonnull void (^)(NSArray<id<UIUserActivityRestoring>> * _Nullable))restorationHandler
{
   return [RCTLinkingManager application:application
   continueUserActivity:userActivity
   restorationHandler:restorationHandler];
}
```

### **Android**

Open your Android Studio project and go to the "AndroidManifest.xml" file. Add an intent filter to your app's main activity, with the following information:

**Adding App Url Scheme and Identifier**

* Finalize the configuration by setting the scheme to url-scheme and defining the main route as route-name.

`<data android:scheme="CUSTOM_URL_SCHEME" android:host="your-url-host" />`

* Replace your-url-scheme with a unique string that will identify your app. This string should be all lowercase and may include numbers and hyphens, but no spaces or special characters.

```
<intent-filter android:autoVerify="true">
       <action android:name="android.intent.action.VIEW" />
       <category android:name="android.intent.category.DEFAULT" />
       <category android:name="android.intent.category.BROWSABLE" />
       <data android:scheme="CUSTOM_URL_SCHEME" android:host="CUSTOM_URL_SCHEME" />
</intent-filter>
```

* Add Intent within your activity tag, save your changes and build your app.

### **React Native Codebase**

Once the above changes have been made, in your App.js file import Linking from ‘react-native’.

You can use the Linking API to listen for changes to the app's deep link URL. To do this, you can use the addEventListener method to register a listener for the url event. Remember to also remove the event listener when it is no longer needed, using the removeEventListener method.

For example,

```
 useEffect(() => {
   const link = Linking.addEventListener('url', handleInitialUrl);


   return () => {
     link?.remove();
   };
 }, []);


 const handleInitialUrl = async () => {
   const url = await Linking.getInitialURL();


   if (url) {
     handleDeepLinking(url);
   }
 };


 const handleDeepLinking = async (event: any) => {
   // handle accordingly
 };
```

When the program is opened, the initial URL is retrieved using the `Linking.getInitialURL()` method. Deep linking uses the `Linking.addEventListener` method to add the event listener that watches for urls and calls the callback method you passed it.

Use the `handleDeepLinking` function to check if the url includes the custom app scheme and handle it accordingly.

### **Widget Component**

Once the widget is loaded check if Widget has widgetToken and referenceId. If the widgetType is bank then open the bank widget url in the default browser. To do that import Linking API from react-native and use openUrl function. openUrl function takes the URL as a parameter.

```
useEffect(() => {
  if (
     !isEmpty(Widget.token) &&
     !isEmpty(Widget.referenceId) &&
     WIDGET_TYPE.BANK === widgetType
   ) {


     const widgetUrl = ‘https://example.widget.machpay.com';
     const token = toBase64(Widget.token);
     const refID = toBase64(Widget.referenceId);


     let url: string = `${widgetUrl}/?senderId=${refID}&token=${token}&brand=${brand}`;


// for bank relogin case 
     if (!isEmpty(bankId)) {
       const encodedBankID = toBase64(bankId as string);


       url = `${widgetUrl}/?senderId=${refID}&token=${token}&locale=en&bankRelogin=true&bankId=${encodedBankID}&affiliate=${branch}`;
     }


     Linking.openURL(url);
     Navigation.dismissModal(componentId);
    }


}, [Widget, widgetType]);
```

This opens the bank widget url in the browser and allows you to complete the process in the browser. Once the process is complete, the url provides an event that can be handled as described earlier in the document.

The WebView component code has been shown below.

```
<WebView
      incognito
      source={{ html }}
      onMessage={onMessage}
      originWhitelist={["*"]}
      javaScriptEnabled={true}
      domStorageEnabled={true}
      startInLoadingState={true}
      injectedJavaScript={injectScript}
      onNavigationStateChange={handleWebViewNavigationStateChange}
      onLoadEnd={(event) => {
           const nativeEvent = event.nativeEvent;


           if (nativeEvent.url && nativeEvent.url.includes(originUrl)) {
                  setRedirect(true);
           }
     }}
/>;
```

## OAuth Bank Relogin <a href="#oauth-bank-relogin" id="oauth-bank-relogin"></a>

When added bank status is moved to LOGIN\_REQUIRED, following parameters need to be passed while loading bank widget inorder to re-login to bank account.

1. bankRelogin \[true]
2. bankId \[Base64 Encoded Bank Id]

{% hint style="info" %}
Example

[https://widget.example.com?token=\<Base64Encoded-token-value>\&senderId=\<Base64Encoded-sender-id-value>\&brand=\<brand-name>\&bankRelogin=true\&bankId=\<Base64Encoded-bank-id-value>](https://widget.example.com/?token=%3CBase64Encoded-token-value%3E\&senderId=%3CBase64Encoded-sender-id-value%3E\&brand=%3Cbrand-name%3E\&bankRelogin=true\&bankId=%3CBase64Encoded-bank-id-value%3E)
{% endhint %}
