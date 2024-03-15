-> create virtual environment

-> pip install django django-restframework environs django-khalti 

-> create project

-> create app

-> include apps inside INSTALLED_APPS
    'rest_framework',
    'khalti',
    'integration.apps.payment',

-> include following SDK in frontend if you are using vanilla JS:

    <html>
    <head>
        <script src="https://khalti.s3.ap-south-1.amazonaws.com/KPG/dist/2020.12.17.0.0.0/khalti-checkout.iffe.js"></script>
    </head>
    <body>
        ...
        <!-- Place this where you need payment button -->
        <button id="payment-button">Pay with Khalti</button>
        <!-- Place this where you need payment button -->
        <!-- Paste this code anywhere in you body tag -->
        <script>
            var config = {
                // replace the publicKey with yours
                "publicKey": "test_public_key_c41fa2f081e44957809cd4bf0e2496eb",
                "productIdentity": "1234567890",
                "productName": "Dragon",
                "productUrl": "http://gameofthrones.wikia.com/wiki/Dragons",
                "paymentPreference": [
                    "KHALTI",
                    "EBANKING",
                    "MOBILE_BANKING",
                    "CONNECT_IPS",
                    "SCT",
                    ],
                "eventHandler": {
                    onSuccess (payload) {
                        // hit merchant api for initiating verfication
                        console.log(payload);
                    },
                    onError (error) {
                        console.log(error);
                    },
                    onClose () {
                        console.log('widget is closing');
                    }
                }
            };

            var checkout = new KhaltiCheckout(config);
            var btn = document.getElementById("payment-button");
            btn.onclick = function () {
                // minimum transaction amount must be 10, i.e 1000 in paisa.
                checkout.show({amount: 1000});
            }
        </script>
        <!-- Paste this code anywhere in you body tag -->
        ...
    </body>
    </html>

-> add khalti secret key in setting.py:

    KHALTI_SECRET_KEY=<your khalti secret key>
    KHALTI_VERIFY_URL='https://khalti.com/api/v2/payment/verify/'

-> include khalti urls in urls.py:

    path('khalti/',include('khalti.urls')),

-> post request token and amount you get after successful payment from frontend
(pass following data as form data):

    {
        "token" : "<your-token>"
        "amount" : "<your-amount>"
    }
