# Sessions

Most interactions with the API require a valid session.  Sessions are passed to the API in an HTTP header called 'HTTP_YOYO_TOKEN'

If you make a request without a valid access token your request will be rejected.

```json
{
  "message": "No session found with token xxx",
  "description": "",
  "errors": []
}
```