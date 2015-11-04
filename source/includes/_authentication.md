# Authentication

Every request to the API must be an authenticated request.  When you join the Yoyo Wallet platform you will be given an access_key and a secret_key.  These keys are used to authenticate your requests to the platform.

Requests to the platform API are authenticated using basic auth.

To authenticate a request, send an “AUTHORIZATION” header of form:

Basic base64({ACCESS_KEY}:{SECRET_KEY})

`Authorization: <base64 encrypted {ACCESS_KEY}:{SECRET_KEY}>`

<aside class="notice">
You must replace <code>{ACCESS_KEY}_{SECRET_KEY}</code> with your real API credentials.
</aside>

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request

curl 'https://api.test.yoyowallet.net/v1/accounts/{ACCOUNT_ID}' \
  -u {ACCESS_KEY}:{SECRET_KEY}
```

```objective_c
	iOS SDK Code TBD
```

```java
	Android SDK Code TBD
```


> Make sure to replace `{ACCESS_KEY}` and `{SECRET_KEY}` with your real API credentials.


