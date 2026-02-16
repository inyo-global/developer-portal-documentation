---
description: >-
  Tokenization is the process where we take the card information, encrypt using
  the highest standards, and returning a code that only your API keys can read.
---

# Tokenizing cards

Because of the PCI requirements, Inyo doesn't allow any of the participants of the payment flow to store card holder data, unless they hold the approved PCI certification and the necessary compliance approvals are issued. As a modern and robust alternative, we employ card tokenization, using vaults and HSM to securely encrypt data, providing our partners a complete solution that is ready to be implemented and deployed in any scenario.

The only way to interact with the tokenization is utilizing our tokenizer library.  Over the next months, native SDKs to the iOS and Android platform will also be available.

Tokenizer is a component that takes over an existing form and requires the additional of a few javascript lines to interact, but provides greater customization.

Parameters available to the tokenizer class

| Param           | Expected values                                                            | Description                                                                                                                                                                                         |
| --------------- | -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| targetId        | #id                                                                        | Id of the container that contains the inputs with the card holder data (pan, expiration date, name and cvv).                                                                                        |
| publicKey       | 12323:123:12313:1231231                                                    | Public key used to identify the merchant and generate the tokens.                                                                                                                                   |
| successCallback | function                                                                   | Callback that will be called when a token has created succesfully.                                                                                                                                  |
| errorCallback   | function                                                                   | Callback that will be called when an error happens during the token creating.                                                                                                                       |
| storeLaterUse   | true or false                                                              | Whether it's an one-time token or recurring.                                                                                                                                                        |
| threeDSData     | <p>{<br>   enable: true,<br>   successUrl: "",<br>   failUrl: "",<br>}</p> | <p>3DS component. <br>Inform the server if should be enforced or not. Please note that enforcement is not mandating 3ds, as not all the banks support and it's up to the bank to accept or not.</p> |

In order to make sure tokenizer is not tempered with, you must require integrity checksum as sha384, as is:

```
<script src="https://cdn.simpleps.com/sandbox/inyo.js" 
integrity="..." crossorigin="anonymous"></script>
```

{% code title="sample-tokenization.html" %}
```html

<!doctype html>
<html lang="en">

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta name="description" content="">

    <title>Inyo - Sample Payment Page</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css>"
        integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">

</head>

<body class="bg-light">

    <form id="_frm" class="needs-validation" novalidate>
        <div class="container">

            <div class="py-5 text-center">
                <h2>Payment Details</h2>
            </div>

            <div class="row">

                <div class="col-md-3">&nbsp;</div>

                <div class="col-md-6">
                    <h4 class="mb-3">Billing address</h4>

                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="firstName">First name</label>
                            <input type="text" class="form-control" id="firstName" placeholder="" value="" required
                                autofocus>
                            <div class="invalid-feedback">
                                Valid first name is required.
                            </div>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="lastName">Last name</label>
                            <input type="text" class="form-control" id="lastName" placeholder="" value="" required>
                            <div class="invalid-feedback">
                                Valid last name is required.
                            </div>
                        </div>
                    </div>

                    <div class="mb-3">
                        <label for="email">Email</label>
                        <input type="email" class="form-control" id="email" value="" placeholder="you@example.com"
                            required>
                        <div class="invalid-feedback">
                            Please enter a valid email address for updates.
                        </div>
                    </div>

                    <div class="mb-3">
                        <label for="address">Address</label>
                        <input type="text" class="form-control" id="address" value="" placeholder="1234 Main St"
                            required>
                        <div class="invalid-feedback">
                            Please enter your billing address.
                        </div>
                    </div>

                    <div class="mb-3">
                        <label for="address2">Address 2 <span class="text-muted">(Optional)</span></label>
                        <input type="text" class="form-control" id="address2" placeholder="Apartment or suite">
                    </div>

                    <div id="_pay">
                        <div class="d-block my-3">
                            <hr>
                            <h4 class="mb-3">Payment - Debit card (only)</h4>
                            <div class="row">

                                <div class="col-md-6 mb-3">
                                    <label for="cc-name">Name on card</label>
                                    <input type="text" class="form-control" data-field="cardholder" id="cc-name"
                                        placeholder="" required>
                                    <small class="text-muted">Full name as displayed on card</small>
                                    <div class="invalid-feedback">
                                        Name on card is required
                                    </div>
                                </div>
                                <div class="col-md-6 mb-3">
                                    <label for="cc-number">Debit card number</label>
                                    <input type="text" pattern="[0-9]{4}[0-9]{4}[0-9]{4}[0-9]{4}" data-field="pan"
                                        class="form-control" id="cc-number" placeholder="" maxlength="19" required>
                                    <div class="invalid-feedback">
                                        Debit card number is wrong
                                    </div>
                                </div>
                            </div>
                            <div class="row">
                                <div class="col-md-3 mb-3">
                                    <label for="cc-expiration">Expiration</label>
                                    <input type="text" pattern="[0-9]{2}/[0-9]{2}" data-field="expirationDate"
                                        class="form-control" id="cc-expiration" placeholder="mm/yy" maxlength="5"
                                        required>
                                    <div class="invalid-feedback">
                                        Expiration date required
                                    </div>
                                </div>
                                <div class="col-md-3 mb-3">
                                    <label for="cc-expiration">CVV</label>
                                    <input type="text" pattern="[0-9]{3,4}" data-field="securitycode"
                                        class="form-control" id="cc-cvv" placeholder="" maxlength="4" required>
                                    <div class="invalid-feedback">
                                        Security code required
                                    </div>
                                </div>
                            </div>
                            <button class="btn btn-primary btn-lg btn-block" id="_btn" type="button">Confirm my
                                payment</button>
                        </div>

                    </div>

                </div>

            </div>
            
        </div>

    </form>

    <footer class="my-5 pt-5 text-muted text-center text-small">
        <p class="mb-1">© Inyo.global</p>
    </footer>

    <script src="https://cdn.simpleps.com/sandbox/inyo.js"></script>

    <script>
        var tokenizer = null;

        document.addEventListener('DOMContentLoaded', () => {
        
        tokenizer = new InyoTokenizer({
                targetId: '#_pay',
                storeLaterUse: true,
                publicKey: publicKey,
                threeDSData: {
                    enable: true,
                    successUrl: "http://localhost/3ds/success",
                    failUrl: "http://localhost/3ds/fail",
                },
                successCallback: this.#successCallback.bind(this),
                errorCallback: this.#errorCallback.bind(this)
            });

            document.getElementById('_btn').addEventListener('click', () => {

                var f = document.getElementById('_frm');

                if (f.checkValidity() !== false) {
                    tokenizer.tokenizeCard();
                }
                else {
                    f.classList.add('was-validated');
                }

            });

        });

        function errorCallback(responseObj) {

            var elements = ['#cc-number', '#cc-expiration', '#cc-cvv'];

            elements.forEach(function(selector) {
                var element = document.querySelector(selector);
                if (element) {
                    element.classList.remove('is-invalid');
                }
            });

            if (responseObj.code == 'INVALID_PAN')
                document.querySelector('#cc-number').classList.add('is-invalid');
            else if (responseObj.code == 'INVALID_EXPIRY_DATE')
                document.querySelector('#cc-expiration').classList.add('is-invalid');
            else if (responseObj.code == 'INVALID_CVV')
                document.querySelector('#cc-cvv').classList.add('is-invalid');

        }

        function successCallback(responseObj) {

            console.log(responseObj);

            if (responseObj.step == 'WAITING_TRANSACTION') 
            {

                alert('Success tokenizing');

                // send data to the backend server

            } 
            else 
            {
                alert('Couldnt process the request');
            }

        }
    </script>
</body>
</html>
```
{% endcode %}

The code provided adds a few steps to the existing HTML code, which can be summarized as:

*   Load an external javascript code, adding the tag

    ```html
    <script src="https://cdn.simpleps.com/sandbox/inyo.js>"></script>
    ```
*   Instantiate the tokenizer object when the DOM is ready, by passing the following params:

    ```jsx
    tokenizer = new InyoTokenizer({
                    targetId: '#_pay', //container the holds the payment fields
                    publicKey: 'yourpublickey', // key provided by inyo
                    successCallback: successCallback, //success callback
                    errorCallback: errorCallback // error callback
                });
    ```
* Add the necessary attributes to the fields “card holder, pan, expiration date and cvv”. Those attributes will instruct the library to perform the necessary bindings.

```jsx
Card holder:
<input type="text" data-field="cardholder" id="cc-name" placeholder="" required>

Pan:
<input type="text" data-field="pan" class="form-control" id="cc-number">

Expiration Date:
<input type="text" data-field="expirationDate" class="form-control" id="cc-expiration">

Cvv:
<input type="text" data-field="securitycode" class="form-control" id="cc-cvv">
```

To finalize, register the callbacks to receive notifications when the token was created, or if an error has prevented it to finalize.

```jsx
function errorCallback(responseObj) {

	    console.log(responseObj);

            var elements = ['#cc-number', '#cc-expiration', '#cc-cvv'];

            elements.forEach(function(selector) {
                var element = document.querySelector(selector);
                if (element) {
                    element.classList.remove('is-invalid');
                }
            });

            if (responseObj.code == 'INVALID_PAN')
                document.querySelector('#cc-number').classList.add('is-invalid');
            else if (responseObj.code == 'INVALID_EXPIRY_DATE')
                document.querySelector('#cc-expiration').classList.add('is-invalid');
            else if (responseObj.code == 'INVALID_CVV')
                document.querySelector('#cc-cvv').classList.add('is-invalid');

        }

        function successCallback(responseObj) {

            console.log(responseObj);

            if (responseObj.step == 'WAITING_TRANSACTION') 
            {

                alert('Success tokenizing');

                // send data to the backend server
                // see sample above

            } 
            else 
            {
                alert('Couldnt process the request');
            }

        }
```

With the card tokenized, you should be able to communicate with your backend, to execute internal activities, as well as interact with our payment api to charge the customer card. You should not store one-time tokens, but you are allowed to store the recurring ones, keeping in mind that no card holder data can be collected and stored.

