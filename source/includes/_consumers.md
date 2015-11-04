# Consumer API

##Authentication

As a mobile app developer your app users will need to have a Yoyo Wallet consumer account.  

Consumer registration and authentication follow the same process.  This is a two factor authentication process to verify you are the owner of the mobile phone.

### 1. Request a passcode

First you must request a passcode.  The passcode is a unique 5 digit number that will be sent to your mobile device.


> You POST a request to the API containing the consumers phone number and email address.

```json
{
	"email": "tommy.tester@example.com", 
	"phone": "441234567890"
}
```

```shell
curl 'https://api.test.yoyowallet.net/v1/consumer-passcodes' \
  -u {ACCESS_KEY}:{SECRET_KEY} \
  -H 'Content-Type: application/json' \ 
  -H 'Accept: application/json' \
  --data-binary $'{"email": "tommy.tester@example.com", "phone": "441234567890"}'
```

```objective_c
	iOS SDK Code TBD
```

```java
	Android SDK Code TBD
```



> The above command returns JSON structured like this. The "id" field is your passcode "id":

```json
{
  "data": {
   "id": "dc05a248-8172-11e5-aa5b-0abe262b2ce9",
   "phone": "441234567890",
   "account_id": "$account_id",
   "email": "tommy.tester@example.com",
   "expires_at": "2015-11-02 15:33:22.227997813 +0000 UTC"
  },
  "metadata": {}
 }
```


`POST https://api.test.yoyowallet/v1/consumer-passcodes`


This request will either create a new consumer account on the Yoyo Wallet platform, or allow an existing consumer to authenticate.

#### Request Data

Parameter | Type |Description
--------- |  ---- |-----------
email | string |Consumer's email address (optional)
phone | string | Consumer's mobile phone number


If the request contains a valid email address and phone number an SMS message will be sent the the user to help authenticate them.

The response returned contains a "id" field.  This is the identifier of your "passcode" which will be used in your 2 factor authentication request.

#### Response Data

Parameter | Type |Description
--------- | ---- | -----------
id | string | passcode id
phone | string | Consumer's mobile phone number
account_id | string | This is your Yoyo Wallet account Id
email | string | Consumer's email address
expires_at | string | This is the time the passcode will expire. Normally passcodes are valid for 30 minutes.



### 2. Create consumer session (verify passcode)

Once you have received an SMS message with your passcode you can perform the second API call to create a consumer session.

> You POST a request to the API verifying the passcode and generating a session token.

```json
{
  "device_id": "e73e3d3b-a95d-4d3c-b9ee-de5492540582+a0b11fa0-57f9-43fc-a541-f868994cf7bb",
  "device_timestamp": 1446368400,
  "passcode_code": "18442",
  "passcode_id": "dc05a248-8172-11e5-aa5b-0abe262b2ce9"
}
```

```shell
curl 'https://api.test.yoyowallet.net/v1/consumer-sessions' \
  -u {ACCESS_KEY}:{SECRET_KEY} \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  --data-binary $'{"device_id": "e73e3d3b-a95d-4d3c-b9ee-de5492540582+a0b11fa0-57f9-43fc-a541-f868994cf7bb", \
  "device_timestamp": 1446368400, \
  "passcode_code": "18442", \
  "passcode_id": "dc05a248-8172-11e5-aa5b-0abe262b2ce9"}'
```

```objective_c
	iOS SDK Code TBD
```

```java
	Android SDK Code TBD
```


> The above command returns JSON structured like this. The "id" field is your passcode "id":

```json

  "data": {
   "consumer_id": "9ae50d73-3bb4-4b55-b258-f93f4f027fb9",
   "otp_seed": "N2U3MmQ4Y2Q3YmE4NGQ4MDg2OTAzYzNhZGMzYmIxMGUAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA==",
   "token": "296be3889bdf40d9bb9570ff4bf2660f",
   "barcode_token": "144648288416"
  },
  "metadata": {}
 }
```


`POST https://api.test.yoyowallet/v1/consumer-sessions`

#### Request Data

Parameter | Type |Description
--------- | ---- | -----------
device_id | string |unique identfier for your mobile device
device_timestamp | int64 | current time on your device as a unix timestamp
passcode_code | string | the value from the SMS message sent to your device
passcode_id | string | the id of the passcode response in the previous request


#### Response Data

Parameter | Type | Description
--------- | ---- | -----------
consumer_id | string | unique identfier for the consumer
otp_seed | string | seed value needed to calculate the one-time-password
token | string | the session token to represent the consumer
barcode_token | string | embedded in a barcode to identify a session.


### 3. Using the consumer's session token

Once you have a valid sesion token you will need to include it in all subsequent requests to interact as that consumer.
Sessions tokens are passed to the API in an HTTP header called 'HTTP_YOYO_TOKEN'

If you make a request without a valid access token your request will be rejected.

```json
{
  "message": "No session found with token xxx",
  "description": "",
  "errors": []
}
```

## Wallet

### Adding a card

> You POST a request to the API containing the consumers card details.

```json
{
	"bin": "123456",
	"number": "123456789012345678",
	"exp_month": 12,
	"exp_year": 2018,
	"cvv": 999,
	"postcode": "N1 7ER"
}
```

```shell
curl 'https://api.test.yoyowallet.net/v1/consumers/{CONSUMER_ID}/cards/' \
  -u {ACCESS_KEY}:{SECRET_KEY} \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  --data-binary $'{	"bin": "123456", "number": "123456789012345678", "exp_month": 12, "exp_year": 2018, "cvv": 999, "postcode": "N1 7ER" }'
```

```objective_c
	iOS SDK Code TBD
```

```java
	Android SDK Code TBD
```

<aside class="warning">
Note - in our playground environment we are connected to the Stripe sandbox, so make sure you use <a href='https://stripe.com/docs/testing'>Stripe's test card numbers</a>
</aside>

> The above command returns JSON structured like this. The "id" field is your card "id":

```json
{
  "data": {
    "id": "14ab0c2f-b998-4845-815c-ee6328a73ab3",
    "active": true,
    "brand": "Visa",
    "exp_month": 12,
    "exp_year": 2018,
    "is_default": true,
    "last4": "5678",
    "postcode": "N1 7ER"
  },
  "metadata": {}
}
```

`POST https://api.test.yoyowallet.net/v1/consumers/{CONSUMER_ID}/cards/`

#### Request Data

Parameter | Type | Description
--------- | ----|  -----------
bin | string | Bank identification number (first 6 digits of card number)
number | string | credit card number
exp_month | integer | expiry month MM
exp_year | integer | expiry year CCYY
cvv | integer | 3 digit card verification value
postcode | string | post code that card is registered at

If the card is successfully added to the consumers wallet you will receive a response like below.

#### Response Data

Parameter | Type | Description
--------- | ---- | -----------
id | string | unique identifier for card
active | boolean | card is active
brand | string | type of card eg. Visa, Mastercard, Amex etc.
exp_month | integer | expiry month MM
exp_year | integer | expiry year CCYY
is_default | boolean | card is default one to use in wallet
last4 | string | last 4 digits of card number
postcode | string | post code that card is registered at

### Removing a card

To delete a card from your wallet, simply send a DELETE request identifying your card, using the consumer's session token.

> You DELETE a request to the API to remove a card

```shell
curl 'https://api.test.yoyowallet.net/v1/consumers/{CONSUMER_ID}/cards/' \
  -X "DELETE" \
  -u {ACCESS_KEY}:{SECRET_KEY}  \
  -H 'HTTP_YOYO_TOKEN: {TOKEN}'
```

```objective_c
	iOS SDK Code TBD
```

```java
	Android SDK Code TBD
```

`DELETE https://api.test.yoyowallet.net/v1/consumers/{CONSUMER_ID}/cards/{CARD_ID}`


### Viewing cards


> You GET a request to the API to retrieve your wallet details.


```shell
curl 'https://api.test.yoyowallet.net/v1/consumers/{CONSUMER_ID}/cards' \
  -u {ACCESS_KEY}:{SECRET_KEY} \
  -H 'Accept: application/json' \
  -H 'HTTP_YOYO_TOKEN: {TOKEN}' 
```

```objective_c
	iOS SDK Code TBD
```

```java
	Android SDK Code TBD
```


> The above command returns JSON structured like this. The "id" field is your passcode "id":

```json
{
  "data": [
  {
    "id": "14ab0c2f-b998-4845-815c-ee6328a73ab3",
    "active": true,
    "brand": "Visa",
    "exp_month": 12,
    "exp_year": 2018,
    "is_default": true,
    "last4": "5678",
    "postcode": "N1 7ER"
  },
    {
    "id": "85960a0f-0ac2-480d-9188-7d5389b5a82c",
    "active": true,
    "brand": "Amex",
    "exp_month": 3,
    "exp_year": 2017,
    "is_default": false,
    "last4": "1234",
    "postcode": "N1 7ER"
  },
  ],
  "metadata": {}
}
```

`GET https://api.test.yoyowallet.net/v1/consumers/{CONSUMER_ID}/cards`

#### Request Data

Parameter | Type |Description
--------- | ---- | -----------
consumer_id | string | unique identifer of the consumer


#### Response Data

Same response format as adding a card.

## Transactions

When a consumer makes purchases via the Yoyo Wallet platform, they will be recorded as transactions against the consumer's account.

> You GET a request to the API to retrieve your wallet details.


```shell
curl 'https://api.test.yoyowallet.net/v1/consumers/{CONSUMER_ID}/transactions' \
  -u {ACCESS_KEY}:{SECRET_KEY} \
  -H 'Accept: application/json' \
  -H 'HTTP_YOYO_TOKEN: {TOKEN}' 
```

```objective_c
	iOS SDK Code TBD
```

```java
	Android SDK Code TBD
```


> The above command returns JSON structured like this. The "id" field is your passcode "id":

```json
{
  "data": [
    {
      "id": "string",
      "account_id": "string",
      "account_name": "string",
      "amount": 0,
      "barcode": "string",
      "consumer_id": "string",
      "consumer_name": "string",
      "created_at": "2015-11-03T13:15:28.078Z",
      "currency": "string",
      "discount": 0,
      "reference": "string",
      "status": "string",
      "transaction_key_id": "string",
      "basket": [
        {
          "amount": 0,
          "discount": 0,
          "product_code": "string",
          "product_description": "string",
          "quantity": 0
        }
      ],
      "refunds": [
        {
          "id": "string",
          "amount": 0,
          "barcode": "string",
          "created_at": "2015-11-03T13:15:28.078Z",
          "currency": "string",
          "status": "string"
        }
      ]
    }
  ],
  "metadata": {}
}
```

`GET https://api.test.yoyowallet.net/v1/consumers/{CONSUMER_ID}/transactions`

#### Request Data

Parameter | Type |Description
--------- |  ----| -----------
consumer_id | path | unique identifer of the consumer
pagination | query | [see details](#pagination)

#### Response Data

Parameter | Type | Description
--------- |  ---- |-----------
id | string |unique identifier of transaction
account_id | string |unique identifier of platform account
account_name | string |platform account name
amount | integer | transaction amount £1.25 = 125
barcode | string | barcode used for transaction
consumer_id | string | unique identfier for the consumer
consumer_name | string | consumer name
created_at | timestamp | transaction creation time
currency | string | currency code
discount | integer | discount amount £1.25 = 125
reference | string | reference
status | string | transaction status (COMPLETED/DENIED)
transaction_key_id | string | unique identifier of transaction key

#### Basket Data

Basket data is the list of items the consumer purchased during the transaction.  Basket data is an array of Basket items.

Parameter | Type | Description
--------- |  ---- |-----------
amount | integer | basket item amount £1.25 = 125
discount | integer | basket item discount amount £1.25 = 125
product_code | string | unique identifier for product
product_name | string | product name

#### Refund Data

Refund data is when a previous completed transaction is refunded, partially or fully. Refund data is an array of Refund items.

Parameter | Type | Description
--------- |  ---- |-----------
id | string |unique identifier of refund
amount | integer | basket item amount £1.25 = 125
barcode | string | barcode used for transaction
created_at | timestamp | transaction creation time
currency | string | currency code
status | string | transaction status (COMPLETED/DENIED)

## Vouchers