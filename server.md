Title: aah Server
Desc: aah server is built using Go provided http.Server. It supports HTTP, HTTPS, UNIX Socket, Let's Encrypt cert and TLS Config. It provides a flexible configuration via aah.conf
Keywords: aah server, go http server, aah go server, http server, web server, let's encrypt, unix socket
---
# aah Server

aah server is built using Go provided `http.Server`. It supports HTTP, HTTPS, UNIX Socket, Let's Encrypt cert and TLS Config. It gives a flexible way to configure `server { ... }` in `aah.conf`.

Learn [Server Config](app-config.html#section-server), [Server Extension](server-extension.html), [Access Log](server-access-log.html), [Dump Request and Response](server-dump-log.html).

### Table of Contents

  * [HTTP](#http)
  * [HTTPS](#https)
      - [Let's Encrypt Auto Cert](#let-s-encrypt-auto-cert)
  * [UNIX Socket](#unix-socket)
  * [TLS Config](#tls-config)
      - [How to?](#how-to)
      - [Example: Hardening SSL Ciphers](#example-hardening-ssl-ciphers)

## HTTP

aah starts the HTTP server based on the `address` and `port` configured.

## HTTPS

aah starts the server if `server.ssl.enable` is set to `true` with the given SSL cert and key. In HTTPS mode, aah sets the header `Strict-Transport-Security` with `max-age=31536000; includeSubDomains`. Know more about [STS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet).

### Let's Encrypt Auto Cert

aah supports automatic Let's Encrypt certs. To enable this functionality, set config `server.ssl.lets_encrypt.enable` to `true`. For more options, have a look at the [configuration](app-config.html#section-server-ssl-lets-encrypt).

<div class="alert alert-info-blue">
<p><strong>Note:</strong> Let's Encrypt CA does not provide certificates for localhost.</p>
</div>

## UNIX Socket

To start the aah server on `UNIX` socket, set `server.address` to socket file.

**Example:**

```bash
address = "unix:/tmp/myapp.sock"
```

## TLS Config

aah HTTPS server mode is amenable in customizing TLS configuration via `aah.SetTLSConfig()`.

```go

func init()  {
  aah.SetTLSConfig(/* TLS config comes here */)
}
```

### How to?

The TLS config can be added by using either of the following two ways-

  * `aah.OnInit` event - This way is better since `aah.AppConfig()` values are readily accessible.
  * `func init()`

```go
// On file <app-base-dir>/app/init.go
func init() {
  // Using `aah.OnInit` event
  aah.OnInit(func(e *aah.Event) {
    // `aah.AppConfig()` values are readily accessible

    aah.SetTLSConfig(&tls.Config{
      // configure TLS
    })
  })
}

// Without using `aah.OnInit` event
func init() {
  aah.SetTLSConfig(&tls.Config{
    // configure TLS
  })
}
```

### Example: Hardening SSL Ciphers

```go
// On file <app-base-dir>/app/init.go
func init() {
  aah.OnInit(func(e *aah.Event) {
    // `aah.AppConfig()` values are readily accessible

    // Customizing a TLS config
    tlsCfg := &tls.Config{
      MinVersion:               tls.VersionTLS12,
      CurvePreferences:         []tls.CurveID{tls.CurveP521, tls.CurveP384, tls.CurveP256},
      PreferServerCipherSuites: true,
      CipherSuites: []uint16{
        tls.TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384,
        tls.TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256,
        tls.TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA,
        tls.TLS_RSA_WITH_AES_256_GCM_SHA384,
        tls.TLS_RSA_WITH_AES_256_CBC_SHA,
      },
    }

    aah.SetTLSConfig(tlsCfg)
  })
}
```
