Title: aah Application Configuration
Desc: aah configuration syntax and structure is easy and flexible. aah supports the concept of profile that helps easily organize the configurations across different environments that are defined by profile.
Keywords: app config, application configuration, aah.conf, HOCON, configuration, external config
---
# Application Configuration

aah configuration syntax and structure is easy and flexible. It is very much similar to HOCON syntax. Learn more about [configuration syntax](configuration.html).

aah supports the concept of profile that helps easily organize the configurations across different environments that are defined by profile.

aah application configuration file called `aah.conf`.

### Configurations

  * [name](#name)
  * [description](#description)
  * [type](#type)
  * [instance_name](#instance-name)
  * [pid_file](#pid-file)
  * [server { ... }](#section-server)    
    - [timeout { ... }](#section-server-timeout)
    - [redirect { ... }](#section-server-redirect) <sup class="new-sup">new</sup>
    - [ssl { ... }](#section-server-ssl)
        - [redirect_http { ... }](#section-server-ssl-redirect-http)
        - [lets_encrypt { ... }](#section-server-ssl-lets-encrypt)
    - [websocket { ... }](websocket.html) <sup class="new-sup">new</sup>
    - [access_log { ... }](server-access-log.html)
    - [dump_log { ... }](server-dump-log.html)
  * [request { ... }](#section-request)
    - [id { ... }](#section-request-id)
    - [content_negotiation { ... }](#section-request-content-negotiation)
    - [auto_bind { ... }](#section-request-auto-bind)
  * [i18n { ... }](#section-i18n)
    - [param_name { ... }](#section-i18n-param-name)
  * [format { ... }](#section-format)
  * [runtime { ... }](#section-runtime)
    - [debug { ... }](#section-runtime-debug)
  * [cache { ... }](#section-cache)
    - [static { ... }](/static-files.html#cache-control)
  * [render { ... }](#section-render)
    - [secure_json { ... }](#section-render) <sup class="new-sup">new</sup>
    - [gzip { ... }](#section-render-gzip)
  * [view { ... }](#section-view)
  * [security { ... }](security-config.html)
  * [log { ... }](log-config.html)
  * [env { ... }](#section-env) - Environment profile(s) gets defined in this section.


## name

Application name, non-whitespace is recommended.

```bash
# Default value is `basename` of import path.
name = "mysampleapp"
```

## description

A friendly description of application.

```bash
desc = "aah framework web application"
```

## type

Application type, typically these `web`, `api`, `websocket`.

```bash
type = "api"
```

## instance_name

Application instance name could be used when running aah application cluster. It is used in context based logging and it helps to distinguishes each application instance from log information.

Supply `instance_name` value via aah external config or Environment variable.

```bash
instance_name = $AAH_INSTANCE_NAME
```

## pid_file

Configure file path of application PID file to be written. Ensure that the application process has appropriate permissions and the existence of the directory.

```bash
# Default value is `<app-base-dir>/<app-binary-name>.pid`.
pid_file = "/path/to/pidfile.pid"
```

---

## Section: server { ... }

aah server configurations, such as address, port, server header, timeout, WebSocket, SSL, Let's Encrypt, access log, dump log, etc.

`server { ... }` configuration goes into `aah.conf`.

```bash
# -----------------------------------------------------------------------------
# Server configuration
# Doc: https://docs.aahframework.org/app-config.html#section-server
# -----------------------------------------------------------------------------
server {
  # Address is used to bind against host address, IP address, UNIX socket.
  #
  # For unix socket: unix:/tmp/aahframework.sock
  # Default value is `empty` string.
  #address = ""

  # Port is used to bind server listener on particular port.
  #
  # For standard port `80` and `443`, put empty string or a value
  # Default value is 8080.
  #port = ""

  # Header value is written as HTTP header `Server: aah-go-server`.
  #
  # To exclude header `Server` from writing, simply comment it out.
  header = "aah-go-server"  

  # Mapped to `http.Server.MaxHeaderBytes`.
  # Default value is `1mb`.
  #max_header_bytes = "1mb"

  # HTTP server keep alive option.
  # Default value is `true`.
  #keep_alive = true
}
```

## Section: server.timeout { ... }

aah server timeout for read, write and graceful shutdown.

`timeout { ... }` configuration goes under the config section `server { ... }`.

```bash
# -----------------------------------------------------------------------------
# aah server timeout configuration
# Doc: https://docs.aahframework.org/app-config.html#section-server-timeout
#
# Valid time units are "s = seconds", "m = minutes"
# -----------------------------------------------------------------------------
timeout {
  # Mapped to `http.Server.ReadTimeout`, is the maximum duration that takes to
  # read the entire request, including the body.
  #
  # Default value is `90s`.
  #read = "90s"

  # Mapped to `http.Server.WriteTimeout`, is the maximum duration before timing
  # out writes of the response. It is reset whenever a new request's header is
  # read.
  #
  # Default value is `90s`.
  #write = "90s"

  # aah server graceful shutdown timeout
  #
  # Default value is `60s`.
  #grace_shutdown = "60s"
}
```

## Section: server.redirect { ... }

aah server redirect configuration for `WWW => Non-WWW` and vice versa.

`redirect { ... }` configuration goes under config section `server { ... }`.

```bash
# -----------------------------------------------------------------------------
# Server redirects configuration (www => non-www, vice versa), its applicable
# to all domains/subdomains configured in `routes.conf`
#
# NOTE: Its applicable to hostname only; not protocol. For `http => https`
# refer to config section `server.ssl.redirect_http`.
# -----------------------------------------------------------------------------
redirect {
  # Enabling redirects.
  # Default value is `false`.
  #enable = true

  # Possible values are
  #   - `www`     - aahframework.org      => www.aahframework.org
  #   - `non-www` - www.aahframework.org  => aahframework.org
  # Default value is `non-www`.
  #to = "www"

  # Redirect code.
  # Default value is 301 MovedPermanently RFC 7231.
  #code = 301
}
```

## Section: server.ssl { ... }

aah server SSL/TLS configurations. By default, they are disabled.

`ssl { ... }` configuration goes under the config section `server { ... }`.

```bash
# -----------------------------------------------------------------------------
# aah server SSL configuration
# Doc: https://docs.aahframework.org/app-config.html#section-server-ssl
# -----------------------------------------------------------------------------
ssl {
  # To enable SSL/TLS on the aah go server.
  # Default value is `false`.
  #enable = false

  # SSL cert file, it is required if `server.ssl.enable = true`.
  #
  # Default value is `empty` string.
  #cert = ""

  # SSL key file, it is required if `server.ssl.enable = true`.
  #
  # Default value is `empty` string.
  #key = ""

  # By default Go enables the HTTP/2 on SSL/TLS.
  #
  # For some reason, if use case needs disabling HTTP/2; not to worry,
  # aah provides the ability to disable HTTP/2.
  #
  # Default value is `false`.
  #disable_http2 = true
}
```

## Section: server.ssl.redirect_http { ... }

aah server HTTP to HTTPS redirects configuration.

`redirect_http { ... }` configuration goes under the config section `server.ssl { ... }`.

```bash
# -----------------------------------------------------------------------------------
# Redirect configuration HTTP => HTTPS functionality does protocol switch, it works
# with domain and subdomains.
#
# Doc: https://docs.aahframework.org/app-config.html#section-server-ssl-redirect-http
#
# For Example:
#   http://aahframework.org      => https://aahframework.org
#   http://www.aahframework.org  => https://www.aahframework.org
#   http://docs.aahframework.org => https://docs.aahframework.org
# -----------------------------------------------------------------------------------
redirect_http {
  # Enabling HTTP => HTTPS redirects.
  #
  # Default value is `false`.
  #enable = true

  # Port no. of HTTP requests to listen.
  # For standard port `80` put empty string or a value.
  #
  # It is required value, no default.
  port = "8080"

  # Redirect HTTP status code
  #
  # Default value is `307`.
  #code = 307
}
```

## Section: server.ssl.lets_encrypt { ... }

`lets_encrypt { ... }` configuration goes under the config section `server.ssl { ... }`.

```bash
# ----------------------------------------------------------------------------------
# Let’s Encrypt is a free, automated, and open certificate authority (CA).
# It provides free digital certificates in order to enable HTTPS (SSL/TLS)
# for websites to create a more secure and privacy-respecting Web.
#
# Doc: https://docs.aahframework.org/app-config.html#section-server-ssl-lets-encrypt
#
# NOTE: Let’s Encrypt does not provide certificates for `localhost`.
# ----------------------------------------------------------------------------------
lets_encrypt {
  # To get SSL certificate from Let's Encrypt CA, enable it.
  #
  # Don't forget to enable `server.ssl.enable = true`.
  #
  # Default value is `false`.
  #enable = false

  # Host policy controls which domains the autocert will attempt
  # to retrieve new certificates for. It does not affect cached certs.
  #
  # It is required value, no default.
  #host_policy = ["example.org", "docs.example.org"]

  # Renew before optionally specifies how early the certificates should
  # be renewed before they expire.
  #
  # Default value is `10` days.
  #renew_before = 10

  # Email optionally specifies a contact email address. This is used in
  # Let's Encrypt to notify any problems with the issued certificates.
  # If the Client's account key is already registered, then Email is not used.
  #email = "jeeva@myjeeva.com"

  # Force RSA makes the autocert generate certificates with 2048-bit RSA keys.
  # If false, a default is used. Currently, the default is EC-based keys
  # using the P-256 curve.
  #force_rsa = false

  # Cache optionally stores and retrieves previously-obtained certificates
  # autocert manager. By default, certs will only be cached for the lifetime
  # of the autocert manager.
  #
  # autocert manager passes the Cache certificates data encoded in PEM,
  # with private/public parts combined in a single `Cache.Put` call,
  # private key first.
  #
  # Default value is `empty` string.
  #cache_dir = "/Users/jeeva/autocert"
}
```

## Section: request { ... }

HTTP request configurations, such as Max body size, Request ID, Content Negotiation, Action auto bind parameters, etc.

`request { ... }` configuration goes into `aah.conf`.

```bash
# -----------------------------------------------------------------------------
# Request configuration
# Doc: https://docs.aahframework.org/app-config.html#section-request
# -----------------------------------------------------------------------------
request {
  # Max request body size for all incoming HTTP requests except `MultipartForm`.
  #
  # It can also be overridden at individual route level in `routes.conf`.
  #
  # Default value is `5mb`.
  #max_body_size = "5mb"

  # Default value is `32mb`. Choose a value that is best suitable for the
  # individual app use case
  #
  # Note: To be removed in-favor of config `request.max_body_size` in upcoming
  # version of aah.
  #multipart_size = "32mb"
}
```

## Section: request.id { ... }

HTTP request ID for traceability.

`id { ... }` configuration goes under the config section `request { ... }`.

```bash
# -----------------------------------------------------------------------------
# aah framework encourages to have a unique `Request ID` for each incoming
# request. It helps in traceability. If a request already has `X-Request-Id`
# HTTP header, then aah does not generate one.
#
# GUID is generated using MongoDB ObjectId algorithm.
#
# Doc: https://docs.aahframework.org/app-config.html#section-request-id
# -----------------------------------------------------------------------------
id {
  # To enable/disable request ID generation.
  # Default value is `true`.
  enable = true

  # HTTP header name for Request ID. If a request already has HTTP header, then
  # aah does not generate one.
  #
  # Default value is `X-Request-Id`, customize it per use case.
  #header = "X-Request-Id"
}
```

## Section: request.content_negotiation { ... }

HTTP request Content Negotiation such as Offered and Accepted.

`content_negotiation { ... }` configuration goes under the config section `request { ... }`.

```bash
# --------------------------------------------------------------------------------------
# Content Negotiation is used to validate what is being `offered` and `accepted`
# by the server in-terms of request and response. It is also known as
# `Content-Type` restrictions.
#
# Perfect for REST API, can also be used for web application if needed.
#
# Doc: https://docs.aahframework.org/app-config.html#section-request-content-negotiation
# --------------------------------------------------------------------------------------
content_negotiation {
  # To enable/disable Content Negotiation.
  # Default value is `false`.
  #enable = true

  # Accepted - `Content-Type` HTTP header RFC 2616, 10.4.16
  #
  # For example: Client sends Content-Type header as `application/xml`.
  # However, server only supports JSON payload as request body.
  # Then, aah responds with 415 Unsupported Media Type.
  #
  # Default value is empty list and disabled.
  #accepted = ["application/json", "text/json"]

  # Offered - `Accept` HTTP header RFC 2616, 10.4.7
  #
  # For example: Client sends Accept header as `application/xml`.
  # However, server supports serving JSON alone i.e. `application/json`.
  # In that case, aah server responds with 406 Not Acceptable.
  #
  # Default value is empty list and disabled.
  #offered = ["application/json", "text/json"]
}
```

## Section: request.auto_bind { ... }

`auto_bind { ... }` configuration goes under the config section `request { ... }`.

```bash
# -----------------------------------------------------------------------------
# Auto Bind configuration for Controller Action parameters.
# Doc: https://docs.aahframework.org/app-config.html#section-request-auto-bind
# -----------------------------------------------------------------------------
auto_bind {
  # Priority is used to select the bind source priority.
  #   P -> Path Parameter
  #   F -> Form Parameter
  #   Q -> Query Parameter
  #
  # If the value is not found it initializes with Go zero value.
  #
  # For Example:
  # Let's say controller action named `OrderInfo` and it has parameter called
  # `orderId`.
  # So, aah tries to parse and bind based on the priority.
  # The `orderId` present in `Path` as well as in `Form`. aah picks the value
  # from `Path`.
  #
  # NOTE: It is recommended to have unique names in the request parameter.
  #
  # Default value is `PFQ`.
  #priority = "PFQ"

  # Tag Name is used to bind values to struct exported fields.
  # Default value is `bind`.
  #tag_name = "bind"
}
```

## Section: i18n { ... }

i18n - Internationalization, Localization configurations.

`i18n { ... }` configuration goes into `aah.conf`.

```bash
# -----------------------------------------------------------------------------
# i18n Internationalization configuration.
# Doc: https://docs.aahframework.org/app-config.html#section-i18n
# -----------------------------------------------------------------------------
i18n {
  # Default config is used as fallback if aah is unable to determine the
  # locale from HTTP Request.
  #
  # Default value is `en`.
  #default = "en"

   # parameter or URL Query parameter.
  param_name {
    # Path Param name
    #
    # i.e. `/:lang/home.html`, `/:lang/aboutus.html`, etc.
    # For e.g.: `/en/home.html`, `/en/aboutus.html`, `/zh-CN/home.html`,
    # `/zh-CN/aboutus.html` etc.
    #
    # Default value is `lang`.
    #path = "locale"

    # Query Param name
    #
    # i.e `?lang=en`, `?lang=zh-CN`, etc.
    #
    # Default value is `lang`.
    #query = "locale"
  }
}
```

## Section: format { ... }

`format { ... }` configuration goes into `aah.conf`.

```bash
# -----------------------------------------------------------------------------
# Format Configuration
#
# Any parse error result in 400 Bad Request.
#
# Doc: https://docs.aahframework.org/app-config.html#section-format
# -----------------------------------------------------------------------------
format {
  # Time format configuration
  # Date and Time format config values are used to parse in the order in which
  # they are defined.
  #
  # It is used in the auto parse and bind request parameters on type `time.Time`.
  time = [
    "2006-01-02T15:04:05Z07:00",
    "2006-01-02T15:04:05Z",
    "2006-01-02 15:04:05",
    "2006-01-02"
  ]
}
```

## Section: runtime { ... }

aah runtime configurations, such as stack trace buffer size, capture all goroutines stack, strip `GOPATH` information from stack trace log.

`runtime { ... }` configuration goes into `aah.conf`.

```bash
# -----------------------------------------------------------------------------
# Runtime configuration
# Doc: https://docs.aahframework.org/app-config.html#section-runtime
# -----------------------------------------------------------------------------
runtime {

}
```

## Section: runtime.debug { ... }

`debug { ... }` configuration goes under the config section `runtime { ... }`.

```bash
# -----------------------------------------------------------------------------
# Debug configuration
# Used to collect stack trace at runtime and parsing of stack trace.
#
# Doc: https://docs.aahframework.org/app-config.html#section-runtime-debug
# -----------------------------------------------------------------------------
debug {
  # Stack buffer size for collecting all go routines stack trace dump at runtime.
  # Default value is `2mb`.
  #stack_buffer_size = "2mb"

  # Whether to collect all the Go routines details or not.
  # Default value is `false`.
  #all_goroutines = true

  # Whether to strip source `src` base path from file path on stack trace.
  # Default value is `false`.
  #strip_src_base = true
}
```

## Section: render { ... }

aah reply/response configurations, such as default fallback content type, secure JSON, Gzip compression, etc.

`render { ... }` configuration goes into `aah.conf`.

```bash
# -----------------------------------------------------------------------------
# Render configuration
# Doc: https://docs.aahframework.org/app-config.html#section-render
# -----------------------------------------------------------------------------
render {
  # aah infers the `Content-Type` value automatically
  # if `Reply()` builder does not have value.
  #
  # It infers in the order of:
  #  - URL file extension - supports `.html`, `.htm`, `.json`, `.js`, `.xml`
  #    and `.txt`,
  #
  #  - Request Accept Header - Most Qualified one as per RFC 7321 .
  #      * Supports vendor type as per RFC 4288.
  #
  #  - Fallback to `render.default` value - supports `html`, `json`, `xml` and `text`.
  #
  # Default value is `empty` string.
  #default = "html"

  # Secure JSON config is to prevent Cross Site Script Inclusion (XSSI) attacks
  # on JSON response payload (aka JSON vulnerability). XSSI attack is only
  # successful if the returned JSON response is executable as JavaScript.
  #
  # aah prefixes JSON response to make them non-executable on
  # method `Reply().JSONSecure()`.
  #
  # Default value is `)]}',\n`.
  #secure_json {
  #  prefix = ")]}',\n"
  #}
}
```

## Section: render.gzip { ... }

HTTP Response Gzip compression configuration.

`gzip { ... }` configuration goes under the config section `render { ... }`.

```bash
# -----------------------------------------------------------------------------
# Gzip Configuration
# Doc: https://docs.aahframework.org/app-config.html#section-render-gzip
# -----------------------------------------------------------------------------
gzip {
  # By default, aah does Gzip compression. However, aah ensures that HTTP
  # client is capable to accept Gzip response.
  #
  # NOTE: If you have web server (`nginx`, `httpd`, etc) enabled with Gzip
  # in-front of aah server, disable the gzip in aah application.
  # There is no benefit in doing double compression; just CPU cycle gets wasted.
  #
  # Default value is `true`.
  #enable = true

  # Gzip compression levels.
  # Valid levels are  1 = BestSpeed to 9 = BestCompression.
  #
  # Default value is `4`.
  #level = 4
}
```

## Section: cache { ... }

aah cache configurations, such as Static files HTTP Cache-Control header, etc.

`cache { ... }` configuration goes into `aah.conf`.

```bash
# -----------------------------------------------------------------------------
# Cache configuration
# Doc: https://docs.aahframework.org/app-config.html#section-cache
# -----------------------------------------------------------------------------
cache {

}
```

## Section: view { ... }

aah view engine configurations, such as

`view { ... }` configuration goes into `aah.conf`.

```bash
# -----------------------------------------------------------------------------
# View configuration
# Doc: https://docs.aahframework.org/app-config.html#section-view
# -----------------------------------------------------------------------------
view {
  # Application view engine.
  # Default value is `go`.
  engine = "go"

  # View file extension.
  #
  # Default value is chosen based on `view.engine` while creating a new app
  # via command `aah new`.
  ext = ".html"

  # Config case_sensitive is used to resolve view file name and path.
  # For e.g.: "/views/pages/app/login.tmpl" == "/views/pages/App/Login.tmpl"
  #
  # Default value is `false`.
  #case_sensitive = false

  # To use custom Go template delimiters for view files.
  # Default value is `{{.}}`.
  #delimiters = "{{.}}"

  # aah supports multi-layouts for views. The page layout name of aah is
  # `master.html`. aah supports non-layout too.
  #
  # NOTE: To use layouts with this setting, just provide layout name explicitly
  # via methods `Reply().HTMLlf` or `Reply().HTMLl`.
  #
  # Default value is `true`. Available since v0.6
  #default_layout = false
}
```

## Section: security { ... }

aah security configurations are defined in separate file to organize better and included using `include` reference in the aah.conf.

`security { ... }` configuration goes into `aah.conf` via file inclusion. Refer to [Security Config](security-config.html).

```bash
# -----------------------------------------------------------------------------
# Security configuration
# Doc: https://docs.aahframework.org/security-config.html
# -----------------------------------------------------------------------------
include "./security.conf"
```

## Section: env { ... }

aah supports the concept of profile that helps easily organize the configurations across different environments that are defined by profile.

For e.g: `dev`, `qa`, `prod` etc.

`env { ... }` configuration goes into `aah.conf`.

```bash
# -----------------------------------------------------------------------------
# Environment Profiles
#
# For e.g.: dev, qa, prod, etc.
#
# Doc: https://docs.aahframework.org/app-config.html#section-env
# -----------------------------------------------------------------------------
env {
  # Active profile name for the application configuration.
  #
  # For e.g.: To activate environment profile via application binary
  # /path/to/binary/aahwebsite -profile prod
  #
  # Default value is `dev`.
  #active = "dev"

  # Environment profile configurations
  # Load all the configuration files from `appbasedir/config/env/*.conf`.
  include "./env/*.conf"
}
```
