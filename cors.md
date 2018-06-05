Title: CORS (Cross-Origin Resource Sharing)
Desc: aah provides an easier and flexible way to configure CORS for domain level as well as for route level. CORS could be disabled at route level too.
Keywords: CORS, cors, Cross-Origin Resource Sharing, aah, aah framework
---
# CORS (Cross-Origin Resource Sharing)

Cross-Origin Resource Sharing (CORS) is a mechanism that uses additional HTTP headers to let a user agent gain permission to access selected resources from a server on a different origin (domain) than the site that is currently in use. [Read more](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

<span class="badge lb-sm">Since v0.10</span> aah provides an easier and flexible way to configure CORS for domain level as well as for route level. CORS could be disabled at route level too.

CORS configuration resides in [`routes.conf`](routes-config.html). It has two parts with the same set of configuration attributes.

  * [Domain level](#domain-level)
  * [Route level](#route-level)

**To enable CORS, place the following config at each domain configuration:**

```bash
cors {
  enable = true
}
```

## Domain Level

Configure CORS values for each domain/sub-domain in routes.conf at domain level.

```bash
# -----------------------------------------------------------------------------
# Cross-Origin Resource Sharing (CORS) configuration (Domain Level)
# Doc: https://docs.aahframework.org/cors.html#domain-level
# -----------------------------------------------------------------------------
cors {
  # At domain level.
  # Default value is `false`.
  enable = true

  # Used to control `Access-Control-Allow-Origin` header.
  # Default value is `*`.
  allow_origins = ["https://www.example.com", "http://sample.com"]

  # Used to control `Access-Control-Allow-Headers` header.
  # Default values are `Accept`, `AcceptLanguage`, `Authorization`, `Origin`.
  allow_headers = ["Accept", "Authorization", "Content-Type", "Origin"]

  # Used to control `Access-Control-Allow-Methods` header.
  # Default values are `GET`, `POST`, `HEAD`.
  allow_methods = ["GET", "POST", "HEAD"]

  # Used to control `Access-Control-Expose-Headers` header.
  # Default value is `cors.allow_headers` config if not provided.
  expose_headers = ["X-Custom-Header"]

  # Used to control `Access-Control-Max-Age` header.
  # Default value is `24h`.
  max_age = "48h"

  # Used to control `Access-Control-Allow-Credentials` header.
  # Default value is `false`.
  allow_credentials = true
}
```

## Route Level

Configure CORS values for each route in the domain. Undefined config attribute values are inherited from parent route or domain level config.

`cors { ... }` configuration goes into each route configuration in `routes.conf`.

```bash
# -----------------------------------------------------------------------------
# Cross-Origin Resource Sharing (CORS) configuration (Route Level)
# Doc: https://docs.aahframework.org/cors.html#route-level
# -----------------------------------------------------------------------------
cors {
  # Default value is `true`.
  #enable = true

  # Used to control `Access-Control-Allow-Origin` header.
  # Default value is inherited from parent route/domain.
  allow_origins = ["https://www.example.com", "http://sample.com"]

  # Used to control `Access-Control-Allow-Headers` header.
  # Default value is inherited from parent route/domain.
  allow_headers = [
    "Accept", "Authorization",
    "Content-Type", "Origin"
  ]

  # Used to control `Access-Control-Allow-Methods` header.
  # Default value is inherited from parent route/domain.
  allow_methods = ["GET", "POST", "HEAD"]

  # Used to control `Access-Control-Expose-Headers` header.
  # Default value is inherited from parent route/domain.
  expose_headers = ["X-Custom-Header"]

  # Used to control `Access-Control-Max-Age` header.
  # Default value is inherited from parent route/domain.
  max_age = "48h"

  # Used to control `Access-Control-Allow-Credentials` header.
  # Default value is inherited from parent route/domain.
  allow_credentials = true
}
```
