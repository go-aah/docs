Title: aah Application Configuration
Desc: aah configuration structure is very flexible, you can override every config settings in environment profiles. Learn about aah application configuration.
Keywords: app config, application configuration, aah.conf, HOCON, configuration
---
# aah Application Configuration

The configuration syntax is used by aah framework is very similar to HOCON syntax. To learn more about **[configuration syntax](configuration.html)**.

aah configuration structure is very flexible it is called as `aah.conf`. You can override every config value in the environment profiles.

You can add your custom config values into app config. Also it can be overridden in the environment profiles.

Reference to [Routes Config](routes-config.html), [Security Config](security-config.html), [Log Config](log-config.html).

### Table of Contents

  * [name](#name)
  * [description](#description)
  * [pidfile](#pidfile)
  * [server { ... }](#section-server)
    - [timeout { ... }](#section-timeout)
    - [ssl { ... }](#section-ssl)
        - [lets_encrypt { ... }](#section-lets-encrypt)
  * [request { ... }](#section-request)
    - [id { ... }](#section-id)
    - [store { ... }](#section-store)
  * [i18n { ... }](#section-i18n)
  * [format { ... }](#section-format)
  * [runtime { ... }](#section-runtime)
    - [debug { ... }](#section-debug)
    - [pooling { ... }](#section-pooling)
  * cache { ... }
    - [static { ... }](/static-files.html#cache-control)
  * [render { ... }](#section-render)
    - [gzip { ... }](#section-gzip)
  * [view { ... }](#section-view)
  * [log { ... }](log-config.html)
  * [env { ... }](#section-env) - Environment profile overrides

Have a look at [aahframework.org app configuration](https://github.com/go-aah/website/blob/master/config/aah.conf). It is moderate one, it gives an idea on how you can do it for your application.

## name
Application name, non-whitespace is recommend.

Default value is `basename` of import path.
```bash
name = "mysampleapp"
```

## description
A friendly description of application purpose.
```bash
desc = "aah framework web application"
```

---

## Section: server { ... }
HTTP server configuration values.

### address
Server `address` is used to bind against host address, IP address, UNIX socket.

Default value is `empty` string.
```bash
address = ""

# for unix socket
address = "unix:/tmp/aahframework.sock"
```

### port
Server `port` number is used to bind on particular port number. For port `80` and `443`, put empty string or actual value.

Default value is `8080`
```bash
port = "8080"

# for 80
port = "80" # port = ""

# for 443
port = "443" # port = ""
```

### max_header_bytes
HTTP server max header bytes size. It is mapped to `http.Server.MaxHeaderBytes`.

Default value is `1mb`
```bash
max_header_bytes = "1mb"
```

### keep_alive
HTTP server keep-alive option.

Default value is `true`
```bash
keep_alive = true
```

### Section: timeout { ... }
This section is used supply server timeout configuration values.

### read
Server read timeout is mapped to `http.Server.ReadTimeout`. Valid time units are `s -> seconds`, `m -> minutes`.

Default value is `90s`
```bash
read = "90s"
```

### write
Server write timeout is mapped to `http.Server.WriteTimeout`. Valid time units are `s -> seconds`, `m -> minutes`.

Default value is `90s`
```bash
write = "90s"
```

### grace_shutdown
Server grace shutdown timeout. Used when app receive the SIGINT, SIGTERM and does graceful shutdown. Valid time units are `s -> seconds`, `m -> minutes`.

Default value is `60s`
```bash
grace_shutdown = "60s"
```

### Section: ssl { ... }
HTTP server SSL/TLS configuration values. By default it is disabled.

### enable
To enable SSL/TLS on the aah go server.

Default value is `false`.
```bash
enable = true
```

### cert
HTTPS server certificate file. Path to the cert file. It is required value if `server.ssl.enable = true`.

Default value is `empty` string.
```bash
cert = ""
```

### key
HTTPS server cert key file. Path to the key file. It is required value if `server.ssl.enable = true`.

Default value is `empty` string.
```bash
key = ""
```

### disable_http2
Go lang by default enables the HTTP/2 on TLS. For some reason if your use case needs disabling HTTP/2; not to worry, aah framework covers it for you. Just set this config value to `true`.

Default value is `false`.

```bash
disable_http2 = true
```

### Section: lets_encrypt { ... }

### enable
To enable Let's Encrypt CA auto SSL/TLS certs on the aah go server.

Let’s Encrypt is a free, automated, and open certificate authority (CA), they provide free digital certificates in order to enable HTTPS (SSL/TLS) for websites to create a more secure and privacy-respecting Web.

Default value is `false`. Don't forget to enable `server.ssl.enable = true`.
```bash
enable = true
```

### host_policy
Host policy controls which domains the `autocert` will attempt to retrieve new certificates for. It does not affect cached certs. It is array of domain and sub-domain names.

It is required, no default value.
```bash
host_policy = ["example.org", "docs.example.org"]
```

### renew_before
Renew before optionally specifies how early certificates should be renewed before they expire.

Default value is `10` days.
```bash
renew_before = 10
```

### email
Email optionally specifies a contact email address. This is used by CAs, such as Let's Encrypt, to notify about problems with issued certificates. If the Client's account key is already registered, Email is not used.

Default value is `empty` string.
```bash
email = "jeeva@myjeeva.com"
```

### force_rsa
Force RSA makes the `autocert` generate certificates with 2048-bit RSA keys. If false, a default is used.

Default is `EC`-based keys using the `P-256` curve.
```bash
force_rsa = false
```

### cache_dir
Cache optionally stores and retrieves previously-obtained certificates autocert manager. By default certs will only be cached for the lifetime of the autocert manager.

Autocert manager passes the Cache certificates data encoded in PEM, with private/public parts combined in a single Cache.Put call, private key first.

Default value is `empty` string.
```bash
cache_dir = "/path/to/store/cache/certs"
```

---

## Section: request { ... }
Request configuration values.

### multipart_size
Request Multi-part size is used for form parsing when request `Content-Type` is `multipart/form-data`.

Default value is `32mb`.
```bash
multipart_size = "32mb"
```

### Section: id { ... }
aah framework encourages to have unique `Request Id` for each incoming request, it is helpful for traceability. If request already has `X-Request-Id` HTTP header then it does not generate one.

Global Unique Identifier (GUID) generate implementation is based on [Mango DB ObjectId algorithm](https://docs.mongodb.com/manual/reference/method/ObjectId/).

* 4-byte value representing the seconds since the Unix epoch,
* 3-byte machine identifier,
* 2-byte process id, and
* 3-byte counter, starting with a random value.

### enable
To enable/disable request ID generation.

Default value is `true`.
```bash
enable = true
```

### header
HTTP header name for generated Request ID. If request already has HTTP header then it does not generate one.

Default value is `X-Request-Id`.
```bash
header = "X-Request-Id"
```

---

## Section: i18n { ... }
Internationalization and Localization configuration values.

### default
It is used as fallback value if framework is unable to determine the locale information from HTTP Request as per RFC7231 and `lang` Query parameter is not present.

Default value is `en`.
```bash
default = "en"
```

---

## Section: format { ... }
Date and time format values. This is used by framework while parsing HTTP request parameters and DB store/retrieve operation `(upcoming)`.

### date
Default value is `2006-01-02`.
```bash
date = "2006-01-02"
```

### datetime
Default value is `2006-01-02 15:04:05`.
```bash
datetime = "2006-01-02 15:04:05"
```

---

## Section: runtime { ... }
aah application runtime configuration values used for debugging, object pooling, etc.

### Section: debug { ... }

### stack_buffer_size
Choose an appropriate buffer size for collecting all goroutines stack trace dump based on your case.

Default value is `2mb`.
```bash
stack_buffer_size = "2mb"
```

### all_goroutines
Whether to collect all the Go routines details or not.

Default value is `false`.
```bash
all_goroutines = false
```

### Section: pooling { ... }
Pooling configuration is used to reduce GC overhead from framework. Tune these value based on your use case. Pool doesn't create object unless it's needed.

### global
Used for `aah.Context`, `ahttp.Request`, `bytes.Buffer`.

Default value is `500`.
```bash
global = 500
```

### buffer
Used for `bytes.Buffer`.

Default value is `200`.
```bash
buffer = 200
```

---

## Section: render { ... }

### default
aah framework identifies the `Content-Type` value automatically. If `aah.Reply()` builder value is not set. It identifies in the order of:

  * Based on URL file extension, supported `.html`, `.htm`, `.json`, `.js`, `.xml` and `.txt`
  * Request Accept Header - Most Qualified one as per [RFC7321](https://tools.ietf.org/html/rfc7231#section-5.3)
  * Config `render.default` value supported types are `html`, `json`, `xml` and `text`
  * Finally aah framework uses `http.DetectContentType` method

Default value is `empty` string.
```bash
default = "json"
```

### pretty
Pretty print option is very helpful in `dev` environment profile. It is only applicable to JSON and XML Content-Type.

Default value is `false`.
```bash
pretty = true
```

### Section: gzip { ... }
Gzip compression configuration for HTTP response.

### enable
By default Gzip compression is enabled in aah framework, however framework ensures HTTP client does accepts Gzip response otherwise it won't use the Gzip compression.

**Tips:** If you have `nginx` or `apache` web server enabled with gzip in-front of aah go server then set this value to `false`.

Default value is `true`.
```bash
enable = true
```

### level
Used to control Gzip compression levels. Valid levels are `1 = BestSpeed` to `9 = BestCompression`.

Default value is `5`.
```bash
level = 3
```

---

## Section: view { ... }

### engine
Choosing view engine for aah application. In the `upcoming` releases framework will provide support to amber, pongo2, and jade. However you can implement on your own with very simple `view.Enginer` interface.

Default value is `go`. Only `go` view engine is supported as of now.
```bash
engine = "go"
```

### ext
Choosing your own view file extension. It is used while parsing view template files.

Default value is `html`.
```bash
ext = ".html"
```

### case_sensitive
Whether you need a case sensitive view file resolve or not.

Default value is `false`.
```bash
# for e.g.: "/views/pages/app/login.tmpl" == "/views/pages/App/Login.tmpl"
case_sensitive = false
```

### delimiters
To use custom Go template delimiters on view files.

Default value is `{{.}}`
```bash
delimiters = "{{.}}"
```

### default_layout
By default aah framework chooses the default app layout as `master.html` if you do not provide one. However you may have a need that certain pages without layout. So use this option to disable the default layout for HTML rendering.

_**Note:**_ With this setting you can still use layouts, just provide layout name via `Reply().HTMLlf` or `Reply().HTMLl` methods.

Default value is `true`. Available since `v0.6`.
```bash
default_layout = false
```

---

## Section: env { ... }
Application environment profiles, to override app config for respective environment. For e.g: `dev`, `qa`, `prod` etc.

## active
Indicates active profile name for application configuration.

Default value is `dev`.
```bash
active = "dev"
```

## include - This is for example purpose, you can use `include` at any levels
Including all the environment profile overrides from `env` directory within `config` directory.
```bash
include "./env/*.conf"
```
