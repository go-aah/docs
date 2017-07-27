Title: aah Server
Desc: aah server is in-built web server provided by Go lang. HTTP, HTTPS, UNIX Socket, Let's Encrypt, Custom TLS Config, etc. Server provides flexible configuration on aah.conf.
Keywords: aah server, go http server, aah go server, http server, web server, let's encrypt, unix socket
---
# aah Server

aah server is a in-built HTTP server provided by Go lang. Framework provides flexible configuration `server { ... }` in the `aah.conf` for the users.

Reference to [Server Config](app-config.html#section-server), [Server Extension](server-extension.html).

### Table of Contents

  * [HTTP](#http)
  * [HTTPS](#https)
      - [Let's Encrypt Auto Cert](#let-s-encrypt-auto-cert)
  * [UNIX Socket](#unix-socket)
  * [Custom TLS Config](#custom-tls-config)
      - [How to do?](#how-to-do)
      - [Example: To improve SSL score](#example-to-improve-ssl-score)

## HTTP

Starts the server based on provided `address` and `port` with HTTP protocol.

## HTTPS

Starts the server if `server.ssl.enable` set to `true` with given SSL cert and key. In the HTTPS mode by default framework sets the header `Strict-Transport-Security` with `max-age=31536000; includeSubDomains`, know more about [STS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet).

### Let's Encrypt Auto Cert

aah framework supports auto Let's Encrypt certs, you can set `server.ssl.lets_encrypt.enable` to `true` to enable it. Have a look on configuration [here](app-config.html#section-lets-encrypt) for more options.

_Note: Let's Encrypt does not provide certificates for localhost._

## UNIX Socket

To start the aah application on `UNIX` socket; set the `server.address` to socket file.

**Example:**
```cfg
address = "unix:/tmp/myapp.sock"
```

## Custom TLS Config

aah provides flexible way to provide custom TLS configuration for the server via `aah.AddServerTLSConfig(...)`.

### How to do?

There are two ways you can add the custom TLS config-
  * Via `aah.OnInit` event - It is `recommended` approach. Since you have access to `aah.AppConfig()` values.
  * Via `func init()`

```go
// Via `aah.OnInit` event - recommended approach
func init() {
  aah.OnInit(func(e *aah.Event) {
    // `aah.AppConfig()` values available for you

    aah.AddServerTLSConfig(&tls.Config{
      // config goes here
    })
  })
}

// Via `func init()`
func init() {
  aah.AddServerTLSConfig(&tls.Config{
    // config goes here
  })
}
```

### Example: To improve your SSL score
```go
func init() {
  // You can use `aah.OnStart` event too.
  aah.OnInit(func(e *aah.Event) {
    // `aah.AppConfig()` values available for you

    // Customizing a TLS config
    tlsCfg := &tls.Config{
      MinVersion:               tls.VersionTLS12,
      CurvePreferences:         []tls.CurveID{tls.CurveP521, tls.CurveP384, tls.CurveP256},
      PreferServerCipherSuites: true,
      CipherSuites: []uint16{
        tls.TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384,
        tls.TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256, // Required for HTTP/2
        tls.TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA,
        tls.TLS_RSA_WITH_AES_256_GCM_SHA384,
        tls.TLS_RSA_WITH_AES_256_CBC_SHA,
      },
    }

    aah.AddServerTLSConfig(tlsCfg)
  })
}
```
