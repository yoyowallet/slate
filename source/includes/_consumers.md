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
curl 'https://api.test.yoyowallet.net/v1/consumer-passcodes' 
  -u $access_key:$secret_key 
  -H 'Content-Type: application/json' 
  -H 'Accept: application/json' 
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

Parameter | Description
--------- |  -----------
email | Consumer's email address (optional)
phone | Consumer's mobile phone number


If the request contains a valid email address and phone number an SMS message will be sent the the user to help authenticate them.

The response returned contains a "id" field.  This is the identifier of your "passcode" which will be used in your 2 factor authentication request.

#### Response Data

Parameter | Description
--------- |  -----------
id | passcode id
phone | Consumer's mobile phone number
account_id | This is your Yoyo Wallet account Id
email | Consumer's email address
expires_at | This is the time the passcode will expire. Normally passcodes are valid for 30 minutes.



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
curl 'https://api.test.yoyowallet.net/v1/consumer-sessions' 
  -u $access_key:$secret_key 
  -H 'Content-Type: application/json' 
  -H 'Accept: application/json' 
  --data-binary $'{"device_id": "e73e3d3b-a95d-4d3c-b9ee-de5492540582+a0b11fa0-57f9-43fc-a541-f868994cf7bb",
  "device_timestamp": 1446368400,
  "passcode_code": "18442",
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

Parameter | Description
--------- |  -----------
device_id | unique identfier for your mobile device
device_timestamp | current time on your device as a unix timestamp
passcode_code | the value from the SMS message sent to your device
passcode_id | the id of the passcode response in the previous request


#### Response Data

Parameter | Description
--------- |  -----------
consumer_id | unique identfier for the consumer
otp_seed | seed value needed to calculate the one-time-password
token | the session token to represent the consumer
barcode_token | embedded in a barcode to identify a session.


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

## Managing your wallet

### Add a card

> You POST a request to the API containing the consumers phone number and email address.

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
curl 'https://api.test.yoyowallet.net/v1/consumers/{$consumer_id}/cards/' 
  -u $access_key:$secret_key 
  -H 'Content-Type: application/json' 
  -H 'Accept: application/json' 
  --data-binary $'{	"bin": "123456", "number": "123456789012345678", "exp_month": 12, "exp_year": 2018, "cvv": 999, "postcode": "N1 7ER" }'
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

`POST https://api.test.yoyowallet.net/v1/consumers/{$consumer_id}/cards/`

#### Request Data

Parameter | Description
--------- |  -----------
bin | Bank identification number (first 6 digits of card number)
number | credit card number
exp_month | expiry month MM
exp_year | expiry year CCYY
cvv | 3 digit card verification value
postcode | post code that card is registered at

If the card is successfully added to the consumers wallet you will receive a response like below.

#### Response Data

Parameter | Description
--------- |  -----------
id | unique identifier for card
active | card is active
brand | type of card eg. Visa, Mastercard, Amex etc.
exp_month | expiry month MM
exp_year | expiry year CCYY
is_default | card is default one to use in wallet
last4 | last 4 digits of card number
postcode | post code that card is registered at


### View your wallet

## Transactions

## Vouchers

## Passcodes