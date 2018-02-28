Title: CORS (Cross-Origin Resource Sharing)
Desc: aah framework CORS implementation.
Keywords: CORS, cors, Cross-Origin Resource Sharing, aahframework
---
# CORS (Cross-Origin Resource Sharing)

Cross-Origin Resource Sharing (CORS) is a mechanism that uses additional HTTP headers to let a user agent gain permission to access selected resources from a server on a different origin (domain) than the site currently in use. [Read more](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

<span class="badge lb-sm">Since v0.10</span> aah framework provides easy to use and flexible way for configure CORS for domain level as well as each route level. Also CORS could be disabled at each route level.

To enable CORS place following config at each domain configuration:
```cfg
cors {
  enable = true
}
```
<br>

CORS configuration live in [`routes.conf`](routes-config.html). Has two parts and config attributes are more or less same.

## Domain Level

Configure CORS values for each domain/sub-domain in the routes.conf. [Learn more](#section-cors) about each attribute.

```cfg
cors {
  enable = true
  allow_origins = ["https://www.example.com", "http://sample.com"]
  allow_headers = ["Accept", "Authorization", "Content-Type", "Origin"]
  allow_methods = ["GET", "POST", "HEAD"]
  expose_headers = ["X-Custom-Header"]
  max_age = "48h"
  allow_credentials = true
}
```

## Route Level

Configure CORS values for each routes in the domain. Undefined config attribute values are inherited from domain level OR Parent route config. [Learn more](#section-cors) about each attribute.

## Section: cors { ... }

The following config attributes applicable to domain level as well as route level.

### enable
Used to enable/disable CORS for domain/route.

Default value is `false` - for domain level.<br>
Default value is `true` - for route level.
```cfg
enable = true
```

### allow_origins
Used to specify value for `Access-Control-Allow-Origin` header.

Default value is `*` - for domain level.<br>
Default value for route level - Inherited from parent route/domain.
```cfg
allow_origins = ["*"]
```

### allow_headers
Used to specify value for `Access-Control-Allow-Headers` header.

Default values are `Accept`, `AcceptLanguage`, `Authorization`, `Origin` - for domain level.<br>
Default value for route level - Inherited from parent route/domain.
```cfg
allow_headers = [
  "Accept",
  "Authorization",
  "Content-Type",
  "Origin",
  "X-Custom-Header"
]
```

### allow_methods
Used to specify value for `Access-Control-Allow-Methods` header.

Default values are `GET`, `POST`, `HEAD` - for domain level.<br>
Default value for route level - Inherited from parent route/domain.
```cfg
allow_methods = ["GET", "POST", "HEAD", "PUT", "DELETE"]
```

### expose_headers
Used to specify value for `Access-Control-Expose-Headers` header.

Default value - `cors.allow_headers` values are used if not provided - for domain level.<br>
Default value for route level - Inherited from parent route/domain.

```cfg
expose_headers = ["X-Custom-Header"]
```

### max_age
Used to specify value for `Access-Control-Max-Age` header.

Default value is `24h` - for domain level.<br>
Default value for route level - Inherited from parent route/domain.
```cfg
max_age = "48h"
```

### allow_credentials
Used to specify value for `Access-Control-Allow-Credentials` header.

Default value is `false` - for domain level.<br>
Default value for route level - Inherited from parent route/domain.
```cfg
allow_credentials = true
```
