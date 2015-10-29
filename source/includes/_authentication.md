# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: blahblahblah"
```

```objective_c
@interface Person : NSObject {
  @public
  NSString *name;
  @private
  int age;
}

@property(copy) NSString *name;
@property(readonly) int age;

-(id)initWithAge:(int)age;
@end
```

```java
public class java {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}

```


> Make sure to replace `blahblahblah` with your API key.

Yoyo Wallet uses API keys to allow access to the API. 

Yoyo Wallet expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: blahblahblah`

<aside class="notice">
You must replace <code>blahblahblah</code> with your personal API key.
</aside>


