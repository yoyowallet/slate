# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: blahblahblah"
```

```go
  
    Write some go code here..

```

```python
import yoyowallet

api = yoyowallet.authorize('blahblahblah')
```


> Make sure to replace `blahblahblah` with your API key.

Yoyo Wallet uses API keys to allow access to the API. 

Yoyo Wallet expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: blahblahblah`

<aside class="notice">
You must replace <code>blahblahblah</code> with your personal API key.
</aside>


