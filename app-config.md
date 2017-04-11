# aah Application Configuration

The configuration syntax is used by aah framework is similar to HOCON syntax, not 100%. To learn more about configuration syntax and how to use them [click here](configuration.html).

aah configuration structure is very flexible, you can override every config settings in environment profiles.

### Table of Contents

  * [name](#name)
  * [description](#description)
  * [pidfile](#pidfile)
  * [server { ... }](#section-server)
    - [timeout { ... }](#section-timeout)
    - [ssl { ... }](#section-ssl)
  * [request { ... }](#section-request)
    - [id { ... }](#section-id)
  * [session { ... }](#section-session)
    - [store { ... }](#section-store)
  * [i18n { ... }](#section-i18n)
  * [format { ... }](#section-format)
  * [runtime { ... }](#section-runtime)
    - [debug { ... }](#section-debug)
    - [pooling { ... }](#section-pooling)
  * [render { ... }](#section-render)
    - [gzip { ... }](#section-gzip)
  * [view { ... }](#section-view)
  * [log { ... }](logging.html)
  * [env { ... }](#section-env) - Environment profile overrides


## name
Application name, non-whitespace is recommend.

Default value is base directory of application import path.
```bash
name = "mysampleapp"
```

## description
A friendly description of application purpose.
```bash
description = "aah framework web application"
```

## pidfile
aah application process ID aka PID. A path to save `pid` file.

Default location is `<app-base-dir>/appname.pid`.
```bash
pidfile = "mysampleapp.pid"
```

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
Server `port` number is used to bind on particular port number. For port `80`, put empty string or `80` as a value.

Default value is `8080`
```bash
port = "8080"
```

### max_header_bytes
HTTP server max header bytes size. It is mapped `http.Server.MaxHeaderBytes`.

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
Server grace shutdown timeout. Applicable only to `go1.8` and above. Valid time units are `s -> seconds`, `m -> minutes`.

Default value is `60s`
```bash
grace_shutdown = "60s"
```

### Section: ssl { ... }
HTTP server SSL/TLS configuration values. By default it is disabled.

### enable
To enable SSL/TLS on the aah server.

Default value is `false`.
```bash
enable = true
```

### cert
HTTP server certificate file. Path to the cert file. It is mandatory if `server.ssl.enable` is `true`.

Default value is `empty` string.
```bash
cert = ""
```

### key
HTTP server key file. Path to the key file. It is mandatory if `server.ssl.enable` is `true`.

Default value is `empty` string.
```bash
key = ""
```

## Section: request { ... }
Request configuration values.

### multipart_size
Request Multi-part size is used for form parsing when request `Content-Type` is `multipart/form-data`.

Default value is `32mb`.
```bash
multipart_size = "32mb"
```

### Section: id { ... }
aah framework encourages to have unique `Request Id` for each incoming request, it helps in tracebility. If request has already `X-Request-Id` HTTP header then it does not generate one.

Global unique identifier (GUID) generate implementation is based on [Mango DB ObjectId algorithm](https://docs.mongodb.com/manual/reference/method/ObjectId/).

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
HTTP header name to set generated Request ID. If request has already has HTTP header then it does not generate one.

Default value is `X-Request-Id`.
```bash
header = "X-Request-Id"
```

## Section: session { ... }
HTTP state management across multiple requests.

### mode
Session mode is to choose whether HTTP session should be persisted or destroyed at the end of each request. Supported values are -

* `stateless` - Session data is destroyed at end of each request
* `stateful` - Session data is persisted based on store type config

Default value is `stateless`
```bash
mode = "stateful"
```

### Section: store { ... }
Session store is to choose where session values should be persisted. Currently aah framework supports `cookie` and `file` as a store type. Also framework provide extensible `session.Storer` interface to add custom session store.

### type
Currently aah framework supports `cookie` and `file` as a store type.

Default store type value is `cookie`
```bash
type = "cookie"
```

### filepath
Filepath is used for file store to store session data in the file system. This is only applicable for `type="file"`, make sure application has Read/Write access to the directory. Provide absolute path.

No default value, it is mandatory for file system type store.
```bash
filepath = "/path/to/store/session/files"
```

### id_length
Session Identifier length. Identifier(ID) is generated using random bytes from `crypto/rand` and `HEX` encoding.

Default value is `32`
```bash
id_length = 32
```

### ttl
Time-to-live value for session data expiry. Valid time units are `m -> minutes`, `h -> hours` and `0`.

Default value is `0`, cookie is deleted when the browser is closed.
```bash
ttl = "0"
```

### prefix
HTTP session cookie name prefix value.

Default value is `aah` For e.g.: `aah_session`.
```bash
prefix = "aah"
```

### domain
HTTP session cookie domain value.

Default value is `empty` string.
```bash
domain = ""
```

### path
HTTP session cookie path value.

Default value is `/`.
```bash
path = "/"
```

### http_only
HTTP session cookie HTTPOnly value. This option prevents XSS (Cross Site Scripting) attacks, basically it disallows access of cookie to scripts like JavaScript.

Default value is `true`.
```bash
http_only = true
```

### secure
HTTP session cookie secure value. However, if aah server is not configured with SSL then aah framework sets this value as `false`.

Default value is `true`.
```bash
secure = true
```

### sign_key
HTTP session cookie value signing using `HMAC`. For server farm this value should be same in all instance. For HMAC sign & verify it is recommend to use key size is `32` or `64` bytes.

Default value is `64` bytes (generated when application gets created using `aah new` command).
```bash
sign_key = "generated-value"
```

### enc_key
HTTP session cookie value encryption and decryption using `AES`. For server farm this value should be same in all instance. AES algorithm is used, valid lengths are `16`, `24`, or `32` bytes to select `AES-128`, `AES-192`, or `AES-256`.

Default value is `32` bytes (generated when application gets created using `aah new` command).
```bash
enc_key = "generated-value"
```

### cleanup_interval
Cleanup Interval is used to clean the expired session objects from store. This is only applicable for non-cookie store type. Cleanup performed in dedicated goroutine. Valid time units are `m -> minutes`, `h -> hours`.

Default value is `30m`.
```bash
cleanup_interval = "30m"
```

## Section: i18n { ... }
Internalization and Localization configuration values.

### default
It is used as fallback value if framework is unable to determine the locale information from HTTP Request as per RFC7231.

Default value is `en`.
```bash
default = "en"
```

## Section: format { ... }
Application date and time format values. This is applicable to HTTP request param parsing and DB store/retrieve operation `(upcoming)`.

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
Used for `aah.Context`, `ahttp.Request`, `session.Session`.

Default value is `0`.
```bash
global = 150
```

### buffer
Used for `bytes.Buffer`.

Default value is `0`.
```bash
buffer = 60
```

## Section: render { ... }

### default
aah framework chooses the `Content-Type` value automatically based on configuration if `aah.Reply()` builder value is not set. It selects in the order of:

  * Based on URL file extension, supported `.html`, `.htm`, `.json`, `.js`, `.xml` and `.txt`
  * Request Accept Header - Most Qualified one as per [RFC7321](https://tools.ietf.org/html/rfc7231#section-5.3)
  * Based `render.default` value supported types are `html`, `json`, `xml` and `text`
  * Finally aah framework uses `http.DetectContentType` API

Default value is `empty` string.
```bash
default = "json"
```

### pretty
Pretty print option is helpful in `dev` environment profile. It is only applicable to JSON and XML.

Default value is `false`.
```bash
pretty = true
```

### Section: gzip { ... }
Gzip compression configuration for HTTP response.

### enable
By default Gzip compression is enabled in aah framework, however framework ensures HTTP client does accepts Gzip response otherwise it won't use Gzip compression.

Tips: If you have `nginx` or `apache` web server enabled with gzip in-front of aah server then set this value to `false`.

Default value is `true`.
```bash
enable = true
```

### level
Used to control Gzip compression levels. Valid levels are `1 = BestSpeed` to `9 = BestCompression`.

Default value is `1` BestSpeed.
```bash
level = 1
```

## Section: view { ... }

### engine
Choosing view engine for aah application. In the `upcoming` release framework will provide support to amber, pongo2, and jade. However you can implement on your own with simple `view.Enginer` interface.

Default value is `go`. Only `go` view engine is supported as of now.
```bash
engine = "go"
```

### ext
Choosing your own view file extension.

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
To use custom Go template delimiters for view files.

Default value is `{{.}}`
```bash
delimiters = "{{.}}"
```

## Section: env { ... }
Application environment profile overrides for `dev`, `qa`, `prod` etc.

## active
Indicates active profile name for application configuration.

Default value is `dev`.
```bash
active = "dev"
```

## include - This is for example purpose, you can use `include` at levels
Including all the environment profile overrides from `env` directory within config.
```bash
include "./env/*.conf"
```
