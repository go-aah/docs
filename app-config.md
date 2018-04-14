Title: aah.conf Application Configuration
Desc: aah configuration structure is very flexible, you can override every config settings in environment profiles also external config file.
Keywords: app config, application configuration, aah.conf, HOCON, configuration
---
# aah Application Configuration

The configuration syntax is used by aah framework is very similar to HOCON syntax. To learn more about **[configuration syntax](configuration.html)**.

aah configuration structure is very flexible it is called as `aah.conf`. You can override every config value in the environment profiles also external config file.

You can add your custom config values into app config. Also it can be overridden in the environment profiles.

Reference to [Routes Config](routes-config.html), [Security Config](security-config.html), [Log Config](log-config.html).

### Table of Contents

  * [name](#name)
  * [description](#description)
  * [type](#type) <span class="badge lb-xs">Since v0.10</span>
  * [instance_name](#instance-name) <span class="badge lb-xs">Since v0.9</span>
  * [pid_file](#pid-file) <span class="badge lb-xs">Since v0.8</span>
  * [server { ... }](#section-server)
    - [timeout { ... }](#section-server-timeout)
    - [ssl { ... }](#section-server-ssl)
        - [redirect_http { ... }](#section-server-ssl-redirect-http) <span class="badge lb-xs">Since v0.9</span>
        - [lets_encrypt { ... }](#section-server-ssl-lets-encrypt)
    - [access_log { ... }](server-access-log.html#access-log-configuration) <span class="badge lb-xs">Since v0.7</span>
  * [request { ... }](#section-request)
    - [id { ... }](#section-request-id)
    - [content_negotiation { ... }](#section-request-content-negotiation) <span class="badge lb-xs">Since v0.8</span>
    - [auto_bind { ... }](#section-request-auto-bind) <span class="badge lb-xs">Since v0.8</span>
  * [i18n { ... }](#section-i18n)
    - [param_name { ... }](#section-i18n-param-name) <span class="badge lb-xs">Since v0.7</span>
  * [format { ... }](#section-format)
  * [runtime { ... }](#section-runtime)
    - [debug { ... }](#section-runtime-debug)
  * cache { ... }
    - [static { ... }](/static-files.html#cache-control)
  * [render { ... }](#section-render)
    - [gzip { ... }](#section-render-gzip)
  * [view { ... }](#section-view)
  * [security { ... }](security-config.html)
  * [log { ... }](log-config.html)
  * [env { ... }](#section-env) - Environment profile overrides

Have a look at [aahframework.org app configuration](https://github.com/go-aah/website/blob/master/config/aah.conf). It is moderate one, it gives an idea on how you can do it for your application.

## name
Application name, non-whitespace is recommend.

Default value is `basename` of import path.
```cfg
name = "mysampleapp"
```

## description
A friendly description of application purpose.
```cfg
desc = "aah framework web application"
```

## type
Application type, typically either Web or API.
```cfg
type = "api"
```


## instance_name

<span class="badge lb-sm">Since v0.9</span> Application instance name is used when you're running aah application cluster. This value is used in the context based logging, it distinguishes your instance log from other instances.

Typically you can pass `instance_name` value via aah external config or Environment variable.
```cfg
instance_name = $AAH_INSTANCE_NAME
```

## pid_file
<span class="badge lb-sm">Since v0.8</span> Configure file path of application PID file to be written. Ensure application has appropriate permission and directory exists.

Default value is `<app-base-dir>/<app-binary-name>.pid`
```cfg
pid_file = "/path/to/pidfile.pid"
```

---

## Section: server { ... }
HTTP server configuration values.

### server.address
Server `address` is used to bind against host address, IP address, UNIX socket.

Default value is `empty` string.
```cfg
address = ""

# for unix socket
address = "unix:/tmp/aahframework.sock"
```

### server.port
Server `port` number is used to bind on particular port number. For port `80` and `443`, put empty string or actual value.

Default value is `8080`
```cfg
port = "8080"

# for 80
port = "80"
# OR
port = ""

# for 443
port = "443"
# OR
port = ""

```

### server.header
<span class="badge lb-sm">Since v0.8</span> Header value written as `Server` HTTP header. If you do not want to include `Server` header, comment it out.

Default value is `aah-go-server`.
```cfg
header = "aah-go-server"
```  

### server.max_header_bytes
HTTP server max header bytes size. It is mapped to `http.Server.MaxHeaderBytes`.

Default value is `1mb`
```cfg
max_header_bytes = "1mb"
```

### server.keep_alive
HTTP server keep-alive option.

Default value is `true`
```cfg
keep_alive = true
```

### Section: server.timeout { ... }
This section is used supply server timeout configuration values.

### server.timeout.read
Server read timeout is mapped to `http.Server.ReadTimeout`. Valid time units are `s -> seconds`, `m -> minutes`.

Default value is `90s`
```cfg
read = "90s"
```

### server.timeout.write
Server write timeout is mapped to `http.Server.WriteTimeout`. Valid time units are `s -> seconds`, `m -> minutes`.

Default value is `90s`
```cfg
write = "90s"
```

### server.timeout.grace_shutdown
Server grace shutdown timeout. Used when app receive the SIGINT, SIGTERM and does graceful shutdown. Valid time units are `s -> seconds`, `m -> minutes`.

Default value is `60s`
```cfg
grace_shutdown = "60s"
```

### Section: server.ssl { ... }
HTTP server SSL/TLS configuration values. By default it is disabled.

### server.ssl.enable
To enable SSL/TLS on the aah go server.

Default value is `false`.
```cfg
enable = true
```

### server.ssl.cert
HTTPS server certificate file. Path to the cert file. It is required value if `server.ssl.enable = true`.

Default value is `empty` string.
```cfg
cert = ""
```

### server.ssl.key
HTTPS server cert key file. Path to the key file. It is required value if `server.ssl.enable = true`.

Default value is `empty` string.
```cfg
key = ""
```

### server.ssl.disable_http2
Go lang by default enables the HTTP/2 on TLS. For some reason if your use case needs disabling HTTP/2; not to worry, aah framework covers it for you. Just set this config value to `true`.

Default value is `false`.

```cfg
disable_http2 = true
```

### Section: server.ssl.redirect_http { ... }
<span class="badge lb-sm">Since v0.9</span> Redirect HTTP => HTTPS functionality does protocol switch, so it works with domain and subdomains.

```cfg
For example:
   http://aahframework.org      => https://aahframework.org
   http://www.aahframework.org  => https://www.aahframework.org
   http://docs.aahframework.org => https://docs.aahframework.org
```

### server.ssl.redirect_http.enable
To enabling HTTP => HTTPS redirects.

Default is value is `false`.
```cfg
enable = true
```

### server.ssl.redirect_http.port
Port no. of HTTP requests to listen. For standard port `80` put empty string or a value.

It is required value, no default.
```cfg
port = "8080"
```

### server.ssl.redirect_http.code
Redirect code to be used when redirecting HTTP request.

Default value is `307`.
```cfg
code = 307
```

### Section: server.ssl.lets_encrypt { ... }

### server.ssl.lets_encrypt.enable
To enable Let's Encrypt CA auto SSL/TLS certs on the aah go server.

Let’s Encrypt is a free, automated, and open certificate authority (CA), they provide free digital certificates in order to enable HTTPS (SSL/TLS) for websites to create a more secure and privacy-respecting Web.

<div class="alert alert-info-blue">
<p><strong>Note:</strong> Let’s Encrypt does not provide certificates for localhost.</p>
</div>

Default value is `false`. Don't forget to enable `server.ssl.enable = true`.
```cfg
enable = true
```

### server.ssl.lets_encrypt.host_policy
Host policy controls which domains the `autocert` will attempt to retrieve new certificates for. It does not affect cached certs. It is array of domain and sub-domain names.

It is required, no default value.
```cfg
host_policy = ["example.org", "docs.example.org"]
```

### server.ssl.lets_encrypt.renew_before
Renew before optionally specifies how early certificates should be renewed before they expire.

Default value is `10` days.
```cfg
renew_before = 10
```

### server.ssl.lets_encrypt.email
Email optionally specifies a contact email address. This is used by CAs, such as Let's Encrypt, to notify about problems with issued certificates. If the Client's account key is already registered, Email is not used.

Default value is `empty` string.
```cfg
email = "jeeva@myjeeva.com"
```

### server.ssl.lets_encrypt.force_rsa
Force RSA makes the `autocert` generate certificates with 2048-bit RSA keys. If false, a default is used.

Default is `EC`-based keys using the `P-256` curve.
```cfg
force_rsa = false
```

### server.ssl.lets_encrypt.cache_dir
Cache optionally stores and retrieves previously-obtained certificates autocert manager. By default certs will only be cached for the lifetime of the autocert manager.

Autocert manager passes the Cache certificates data encoded in PEM, with private/public parts combined in a single Cache.Put call, private key first.

Default value is `empty` string.
```cfg
cache_dir = "/path/to/store/cache/certs"
```

---

## Section: request { ... }
Request configuration values.

### request.max_body_size
<span class="badge lb-sm">Since v0.8</span> Max request body size for all incoming HTTP requests except `MultipartForm`. Also you can override this size for individual route on specific cases in `routes.conf` if need be.

Default value is `5mb`.
```cfg
max_body_size = "10mb"
```

### request.multipart_size
Request Multi-part size is used for form parsing when request `Content-Type` is `multipart/form-data`.

Default value is `32mb`.
```cfg
multipart_size = "32mb"
```

### Section: request.id { ... }
aah framework encourages to have unique `Request Id` for each incoming request, it is helpful for traceability. If request already has `X-Request-Id` HTTP header then it does not generate one.

Global Unique Identifier (GUID) generate implementation is based on [Mango DB ObjectId algorithm](https://docs.mongodb.com/manual/reference/method/ObjectId/).

* 4-byte value representing the seconds since the Unix epoch,
* 3-byte machine identifier,
* 2-byte process id, and
* 3-byte counter, starting with a random value.

### request.id.enable
To enable/disable request ID generation.

Default value is `true`.
```cfg
enable = true
```

### request.id.header
HTTP header name for generated Request ID. If request already has HTTP header then it does not generate one.

Default value is `X-Request-Id`.
```cfg
header = "X-Request-Id"
```

### Section: request.content_negotiation { ... }
<span class="badge lb-sm">Since v0.8</span> Content negotiation is used to validate what is being `offered` and `accepted` by server in-terms of request and response. Also known as `Content-Type` restrictions.

### request.content_negotiation.enable
To enable/disable Content Negotiation for your application.

Default value is `false`.
```cfg
enable = true
```

### request.content_negotiation.accepted
Accepted - `Content-Type` HTTP header [RFC2616](https://tools.ietf.org/html/rfc2616#section-10.4.16).

<u>For example:</u> Client sends `Content-Type` header as `application/xml`. However server only supports JSON payload as request body. Then server responds with `415 Unsupported Media Type`.

Default value is empty list and disabled.
```cfg
accepted = ["application/json", "text/json"]
```

### request.content_negotiation.offered
Offered - `Accept` HTTP header [RFC2616](https://tools.ietf.org/html/rfc2616#section-10.4.7).

<u>For example:</u> Client sends Accept header as `application/xml`. However server only supports serving JSON i.e. `application/json`. Then server responds with `406 Not Acceptable`.

Default value is empty list and disabled.
```cfg
offered = ["application/json", "text/json"]
```

## Section: request.auto_bind { ... }
Auto Bind configuration used to bind request parameters to controller action parameters.

### request.auto_bind.priority
Priority is used to select the bind source priority.
```cfg
P -> Path Parameter
F -> Form Parameter
Q -> Query Parameter
```

<u>For example:</u> Let's say you have a controller action named `OrderInfo` and its has parameter called `orderId`. So framework tries to parse and bind based on the priority. The `orderId` present in `Path` and `Form`, framework parse and binds the value from `Path`.

Typically recommended to have unique names in the request parameter though :) If value is not found then it returns with default Go zero value.

Default value is `PFQ`.
```cfg
priority = "PFQ"
```

### request.auto_bind.tag_name
Tag Name is used for bind values to struct exported fields.

Default value is `bind`.
```cfg
tag_name = "bind"
```

---

## Section: i18n { ... }
Internationalization and Localization configuration values.

### i18n.default
It is used as fallback value if framework is unable to determine the locale information from HTTP Request as per RFC7231 and `lang` Query parameter is not present.

Default value is `en`.
```cfg
default = "en"
```

### Section: i18n.param_name { ... }
Overriding Request Locale `Accept-Language` header value via URL Path parameter or URL Query parameter.

### i18n.param_name.path
Specify URL Path Parameter name i.e. `/:lang/home.html`, `/:lang/aboutus.html`, etc.  For e.g.: `/en/home.html`, `/en/aboutus.html`, `/zh-CN/home.html`, `/zh-CN/aboutus.html` etc.

Default value is `lang`
```cfg
path = "locale"
```

### i18n.param_name.query
Specify URL Query Param name i.e `?lang=en`, `?lang=zh-CN`, etc.

Default value is `lang`
```cfg
query = "locale"
```

---

## Section: format { ... }
Date, Time format values. These formats used to parse in the order they defined while [Auto Parse and Bind](request-parameters-auto-bind.html) into controller action parameters.

Any parse error result in 400 Bad Request.

### format.time
```cfg
time = [
  "2006-01-02T15:04:05Z07:00",
  "2006-01-02T15:04:05Z",
  "2006-01-02 15:04:05",
  "2006-01-02"
]
```

---

## Section: runtime { ... }
aah application runtime configuration values used for debugging, object pooling, etc.

### Section: runtime.debug { ... }

### runtime.debug.stack_buffer_size
Choose an appropriate buffer size for collecting all goroutines stack trace dump based on your case.

Default value is `2mb`.
```cfg
stack_buffer_size = "2mb"
```

### runtime.debug.all_goroutines
Whether to collect all the Go routines details or not.

Default value is `false`.
```cfg
all_goroutines = false
```

### runtime.debug.strip_src_base
Whether to strip source `src` base path from file path.

Default value is `false`.
```cfg
strip_src_base = true
```    

---

## Section: render { ... }

### render.default
aah framework identifies the `Content-Type` value automatically, when `aah.Reply()` builder Content-Type value is not set. It identifies in the order of:

  * Based on URL file extension, supported `.html`, `.htm`, `.json`, `.js`, `.xml` and `.txt`
  * Request Accept Header - Most Qualified one as per [RFC7321](https://tools.ietf.org/html/rfc7231#section-5.3)
  * Config `render.default` value supported types are `html`, `json`, `xml` and `text`
  * Finally aah framework uses `http.DetectContentType` method

Default value is `empty` string.
```cfg
default = "json"
```

### secure_json { ... }

Secure JSON is to prevent against Cross Site Script Inclusion (XSSI) attacks on JSON response payload aka JSON vulnerability. XSSI attack is only successful if the returned JSON response is executable as JavaScript.

### prefix
`aah` prevent an attack by prefixing JSON response to make them non-executable.

Default value id `)]}',\n`.
```cfg
prefix = ")]}',\n"
```


### Section: render.gzip { ... }
Gzip compression configuration for HTTP response.

### render.gzip.enable
By default Gzip compression is enabled in aah framework, however framework ensures HTTP client does accepts Gzip response otherwise it won't use the Gzip compression.

<div class="alert alert-info-green">
<p><strong>Tip:</strong> If you have `nginx` or `apache` web server enabled with gzip in-front of aah go server then set this value to `false`. There is not point in doing double compression.<p>
</div>

Default value is `true`.
```cfg
enable = true
```

### render.gzip.level
Used to control Gzip compression levels. Valid levels are `1 = BestSpeed` to `9 = BestCompression`.

Default value is `5`.
```cfg
level = 3
```

---

## Section: view { ... }

### view.engine
Choosing view engine for aah application. You could implement on your own with very simple interface `view.Enginer`.

Default value is `go`.
```cfg
engine = "go"
```

### view.ext
Choosing your own view file extension. It is used while find, filter and parse view files.

Default value is chosen based on `view.engine`, while creating a new app using command `aah new`.
```cfg
ext = ".html"
```

### view.case_sensitive
Whether you need a case sensitive view file resolve or not.

Default value is `false`.
```cfg
# for e.g.: "/views/pages/app/login.tmpl" == "/views/pages/App/Login.tmpl"
case_sensitive = false
```

### view.delimiters
To use custom Go template delimiters on view files.

Default value is `{{.}}`
```cfg
delimiters = "{{.}}"
```

### view.default_layout
By default aah framework chooses the default app layout as `master.html` if you do not provide one. However you may have a need that certain pages without layout. So use this option to disable the default layout for HTML rendering.

<div class="alert alert-info-blue">
<p><strong>Note:</strong> With this setting you can still use layouts, just provide layout name via <code>Reply().HTMLlf</code> or <code>Reply().HTMLl</code> methods</p>
</div>

Default value is `true`. <span class="badge lb-sm">Since v0.6</span>
```cfg
default_layout = false
```

---

## Section: env { ... }
Application environment profiles, to override app config for respective environment. For e.g: `dev`, `qa`, `prod` etc.

## env.active
Indicates active profile name for application configuration.

Default value is `dev`.
```cfg
active = "dev"
```

## include - This is for example purpose, you can use `include` at any levels
Including all the environment profile overrides from `env` directory within `config` directory.
```cfg
include "./env/*.conf"
```
