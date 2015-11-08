# Dashboard API

The Dashboard API is a RESTful API which uses JSON formatted data for its requests and responses. It used by administrative dashboards to communicate with the Yoyo Wallet platform.

##Authentication

As a dashboard developer your dashboard users will need to have a Yoyo Wallet user account.  

Users can be created / invited from the dashboard.


### Logging in via the API

> You POST a request to the API containing the user's name and password.

```json
{
  "email": "test@here.com",
  "password": "your_password"
}
```

```shell
curl 'https://api.yoyoplayground.net/v1/consumer-passcodes' \
  -u {ACCESS_KEY}:{SECRET_KEY} \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
 --data-binary $'{\n  "email": "{$USER_EMAIL}",\n  "password": "{$PASSWORD}"\n}' 
```

```swift
```

```java
```

> The above command returns JSON structured like this.

```json
{
  "data": {
    "token": "99EF65BE82564D1B92992D5394C36088",
    "expires": "2015-11-09T21:47:56.954145Z",
    "user_id": "5f237209-3bfe-45c3-9bc8-f0c4753e444"
  },
  "metadata": {}
}
```

For your dashboard to interact with the dashboard API on behalf of a user, the user will need a session token.

This request will then provide you with a session token which you can use for the HTTP_YOYO_TOKEN.

