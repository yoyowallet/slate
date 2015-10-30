# Authentication

Every request to the API must be an authenticated request.  When you join the Yoyo Wallet platform you will be given an access_key and a secret_key.  These keys are used to authenticate your requests to the platform.

Requests to the platform API are authenticated using basic auth.

To authenticate a request, send an “AUTHORIZATION” header of form:

Basic base64($access_key:$secret_key)

`Authorization: <base64 encrypted $access_key:$secret_key>`

<aside class="notice">
You must replace <code>$access_key_$secret_key</code> with your real API credentials.
</aside>

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request

curl 'https://api.test.yoyowallet.net/v1/accounts/a' 
  -u $access_key:$secret
```

```objective_c
	iOS SDK Code TBD
```

```java
	Android SDK Code TBD
```


> Make sure to replace `$access_key` and `$secret_key` with your real API credentials.


