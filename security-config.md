Title: aah Security Configuration
Desc: aah Security configuration to configure Authentication, Authorization, Session Management, Security Headers, CSRF, etc.
Keywords: security config, security configuration, authentication, authorization, session config, auth config, csrf, HOCON
---
# aah Security Configuration

aah Security configuration is to configure Authentication, Authorization, Session Management, Secure Headers, Anti-CSRF, etc. The configuration syntax is used by aah framework is very similar to HOCON syntax. Learn more about [configuration syntax](configuration.html).

### Table of Contents

* [security { ... }](#section-security)
  - [auth_schemes { ... }](#section-auth-schemes) <span class="badge lb-xs">Since v0.7</span>
      - [form auth](authentication.html#form-based-auth)
      - [basic auth](authentication.html#basic-auth)
      - [generic auth](authentication.html#generic-auth)
  - [session { ... }](#section-session)
  - [anti_csrf { ... }](#section-anti-csrf) <span class="badge lb-xs">Since v0.9</span>
  - [http_header { ... }](#section-http-header) <span class="badge lb-xs">Since v0.8</span>
      - [X-XSS-Protection](#header-x-xss-protection)
      - [X-Content-Type-Options](#header-x-content-type-options)
      - [X-Frame-Options](#header-x-frame-options)
      - [Referrer-Policy](#header-referrer-policy)
      - [Strict-Transport-Security (STS, aka HSTS)](#header-strict-transport-security-sts-aka-hsts)
      - [Content-Security-Policy (CSP)](#header-content-security-policy-csp)
      - [Public-Key-Pins (PKP, aka HPKP)](#header-public-key-pins-pkp-aka-hpkp)
      - [X-Permitted-Cross-Domain-Policies](#header-x-permitted-cross-domain-policies)

## Section: security { ... }
To configure application security related configuration in the section.

---

### Section: auth_schemes { ... }
`auth_schemes` section is used to configure application authentication and authorization. Out-of-the-box aah framework supports following schemes-

  * [FormAuth](authentication.html#form-based-auth) - Register your own dynamic integration of Authentication and Authorization.
  * [BasicAuth](authentication.html#basic-auth) - File realm or implement your own dynamic integration of Authentication and Authorization.
  * [GenericAuth](authentication.html#generic-auth) - brings more possibilities.

---

### Section: session { ... }
HTTP state management across HTTP requests.

### security.session.mode
Session mode is to choose whether HTTP session should be persisted or destroyed at the end of each request. Supported values are -

* `stateless` - Session data is destroyed at end of each request
* `stateful` - Session data is persisted based on store type config

Default value is `stateless` for API and `stateful` for Web application.
```cfg
mode = "stateful"
```

### Section: store { ... }
Session store is to configure where session values should be persisted. Currently aah framework supports `cookie` and `file` as a store type. Also framework provides extensible `session.Storer` interface to add your custom session store.

### security.session.store.type
Currently aah framework supports `cookie` and `file` as a store type.

<div class="alert alert-info-blue">
<p><strong>Note:</strong> If you're using cookie session store, you have to be mindful about cookie size limit <code>4kb</code>.</p>
</div>

Default store type value is `cookie`.
```cfg
type = "cookie"
```

### security.session.store.filepath
Filepath is used for file store to store session data in the file system. This is only applicable for `store.type = "file"`, make sure application has Read/Write access to the directory. Provide absolute path.

Default value is `<app-base-dir>/sessions`.
```cfg
filepath = "/path/to/store/session/files"
```

### security.session.id_length
Session Identifier length. Identifier(ID) is generated using random bytes from `crypto/rand` and `HEX` encoding.

Default value is `32`
```cfg
id_length = 32
```

### security.session.ttl
Time-to-live value for session data expiry. Valid time units are `m -> minutes`, `h -> hours` and `0`.

Default value is `0`, cookie is deleted when the browser is closed.
```cfg
ttl = "0"
```

### security.session.prefix
HTTP session cookie name prefix value.

Default value is `aah` For e.g.: `aah_session`.
```cfg
prefix = "aah"
```

### security.session.domain
HTTP session cookie domain value.

Default value is `empty` string.
```cfg
domain = ""
```

### security.session.path
HTTP session cookie path value.

Default value is `/`.
```cfg
path = "/"
```

### security.session.http_only
HTTP session cookie HTTPOnly value. This option is to prevents XSS (Cross Site Scripting) attacks, basically it disallows access of cookie to scripts like JavaScript.

Default value is `true`.
```cfg
http_only = true
```

### security.session.secure
HTTP session cookie secure value. However, if aah server is not configured with SSL then aah framework sets this value as `false`.

Default value is `true`.
```cfg
secure = true
```

### security.session.sign_key
HTTP session cookie value signing using `HMAC`. For server farm this value should be same in all instance. For HMAC sign & verify it is recommend to use key size is `32` or `64` bytes.

Default value is `64` bytes (generated when application gets created using `aah new` command).
```cfg
sign_key = "generated-value"
```

### security.session.enc_key
HTTP session cookie value encryption and decryption using `AES`. For server farm this value should be same in all the instances. AES algorithm is used, valid lengths are `16`, `24`, or `32` bytes to select `AES-128`, `AES-192`, or `AES-256`.

Default value is `32` bytes (generated when application gets created using `aah new` command).
```cfg
enc_key = "generated-value"
```

### security.session.cleanup_interval
Cleanup Interval is used to clean the expired session data from the store. This is only applicable for non-cookie store type. Cleanup performed in dedicated goroutine. Valid time units are `m -> minutes`, `h -> hours`.

Default value is `30m`.
```cfg
cleanup_interval = "30m"
```

---

### Section: anti_csrf { ... }
Documents the settings belongs to Anti-CSRF Protection.

### security.anti_csrf.enable
Enabling Anti-CSRF Protection.

Default value is `true`.
```cfg
enable = true
```

### security.anti_csrf.secret_length
Anti-CSRF secret length. Salted cipher token length `secret_length * 2`.

Default value is `32`.
```cfg
secret_length = 64
```

### security.anti_csrf.header_name
HTTP Header name for cipher token.

Default value is `X-Anti-CSRF-Token`.
```cfg
header_name = "X-Anti-CSRF-Token"
```

### security.anti_csrf.form_field_name
Form field name for cipher token

Default value is `anti_csrf_token`.
```cfg
form_field_name = "anti_csrf_token"
```

### security.anti_csrf.prefix
Anti-CSRF secure cookie prefix.

Default value is `aah`. Cookie name would be `aah_anti_csrf`.
```cfg
prefix = "aah"
```

### security.anti_csrf.domain
Default value is `empty` string.
```cfg
domain = ""
```

### security.anti_csrf.path
Default value is `/`.
```cfg
path = "/"
```

### security.anti_csrf.ttl
Time-to-live for Anti-CSRF secret. Valid time units are "m = minutes", "h = hours" and 0.

Default value is `24h`.
```cfg
ttl = "24h"
```

### security.anti_csrf.sign_key
Anti-CSRF cookie value signing using `HMAC`. For server farm this should be same in all instance. For HMAC sign & verify it recommend to use key size is `32` or `64` bytes.

Default value is `64` bytes (`aah new` generates strong one).
```cfg
sign_key = "<typically generated by 'aah new' cmd>"
```

### security.anti_csrf.sign_key
Anti-CSRF cookie value encryption and decryption using `AES`. For server farm this should be same in all instance. AES algorithm is used, valid lengths are `16`, `24`, or `32` bytes to select `AES-128`, `AES-192`, or `AES-256`.

Default value is `32` bytes (`aah new` generates strong one).
```cfg
enc_key = "<typically generated by 'aah new' cmd>"
```

---

## Section: http_header { ... }

<span class="badge lb-sm">Since v0.8</span> aah provides application secure headers with many safe defaults. Typically `non-empty` header values from configuration gets added in the response header.

Framework writes the secure response headers appropriately based on Content-Type.

<div class="alert alert-info-green">
<p><strong>Tip:</strong>
<ul>
  <li>Quick way to verify your application secure headers - <a href="https://securityheaders.io"> https://securityheaders.io</a></li>
  <li>Exclude header from writing, just put <code>empty string</code> as a value.</li>
</ul>
</p>
</div>

### Header: X-XSS-Protection
Designed to enable the cross-site scripting (XSS) filter built into modern web browsers. This is usually enabled by default, but using this header will enforce it.

Learn more:

  * https://www.owasp.org/index.php/OWASP_Secure_Headers_Project#xxxsp
  * https://www.keycdn.com/blog/x-xss-protection/

Encouraged to make use of header `Content-Security-Policy` with enhanced policy to reduce XSS risk along with header `X-XSS-Protection`.

Default values is `1; mode=block`.
```cfg
xxssp = "1; mode=block"
```

### Header: X-Content-Type-Options
Prevents Content Sniffing or MIME sniffing.

Learn more:

  * https://www.owasp.org/index.php/OWASP_Secure_Headers_Project#xcto
  * https://en.wikipedia.org/wiki/Content_sniffing

Default value is `nosniff`.
```cfg
xcto = "nosniff"
```

### Header: X-Frame-Options
Prevents Clickjacking.

Learn more:

  * https://www.owasp.org/index.php/OWASP_Secure_Headers_Project#xfo
  * https://www.keycdn.com/blog/x-frame-options/

Default value is `SAMEORIGIN`.
```cfg
xfo = "SAMEORIGIN"
```

### Header: Referrer-Policy
This header governs which referrer information, sent in the Referer header, should be included with requests made.

Referrer Policy has been a W3C Candidate Recommendation since 26 January 2017.

Learn more:

  * https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy
  * https://scotthelme.co.uk/a-new-security-header-referrer-policy/
  * https://www.w3.org/TR/referrer-policy/

Default value is `no-referrer-when-downgrade`.
```cfg
rp = "no-referrer-when-downgrade"
```

### Header: Strict-Transport-Security (STS, aka HSTS)
STS header that lets a web site tell browsers that it should only be communicated with using HTTPS, instead of using HTTP.

Learn more:

  * https://www.owasp.org/index.php/OWASP_Secure_Headers_Project#hsts
  * https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security

<div class="alert alert-info-blue">
<p><strong>Note:</strong> Framework checks that application uses SSL on startup then applies this header. Otherwise it does not apply.</p>
</div>

```cfg
sts {
  # The time, in seconds, that the browser should remember that this site
  # is only to be accessed using HTTPS. Valid time units are
  # "s -> seconds", "m -> minutes", "h - hours".
  # Default value is `30 days` in hours.
  #max_age = "720h"

  # If enabled the STS rule applies to all of the site's subdomains as well.
  # Default value is `false`.
  #include_subdomains = true

  # Before enabling preload option, please read about pros and cons from above links.
  # Default value is `false`.
  #preload = false
}
```

### Header: Content-Security-Policy (CSP)
Provides a rich set of policy directives that enable fairly granular control over the resources that a page is allowed. Prevents XSS risks.

Learn more:

  * https://content-security-policy.com/
  * https://developers.google.com/web/fundamentals/security/csp/
  * https://www.owasp.org/index.php/OWASP_Secure_Headers_Project#csp

Read above references and define your policy.

<div class="alert alert-info-yellow">
<p><strong>Note:</strong>
<ul>
  <li>It is highly recommended to verify your policy directives in report only mode before enabling this header. Since its highly controls how your page is rendered.</li>
  <li>Only applied to <code>prod</code> environment profile.</li>
</ul>
</p>
</div>

No default values, you have to provide it.
```cfg
csp {
  # Set of directives to govern the resources load on a page.
  #directives = ""

  # By default, violation reports aren't sent. To enable violation reporting,
  # you need to specify the report-uri policy directive.
  report_uri = ""

  # Puts your `Content-Security-Policy` in report only mode, so that you can verify
  # and then set `csp_report_only` value to false.
  # Don't forget to set the `report-uri` for validation.
  report_only = true
}
```

### Header: Public-Key-Pins (PKP, aka HPKP)
This header prevents the Man-in-the-Middle Attack (MITM) with forged certificates.

Learn more:

  * https://scotthelme.co.uk/hpkp-http-public-key-pinning/
  * https://developer.mozilla.org/en-US/docs/Web/HTTP/Public_Key_Pinning

Read above references and define your keys.

<div class="alert alert-info-yellow">
<p><strong>Note:</strong>
<ul>
  <li>HPKP has the potential to lock out site/users for a long time if used incorrectly! The use of backup certificates and/or pinning the CA certificate is recommended.</li>
  <li>It is highly recommended to verify your PKP in report only mode before enabling this header</li>
  <li>Framework checks that application uses SSL on startup then applies this header. Otherwise it does not apply</li>
  <li>Only applied to <code>prod</code> environment profile.</li>
</ul>
</p>
</div>

No default values, you have to provide it.
```cfg
pkp {
  # The Base64 encoded Subject Public Key Information (SPKI) fingerprint.
  # These values gets added as `pin-sha256=<key1>; ...`.
  #keys = [
  #"X3pGTSOuJeEVw989IJ/cEtXUEmy52zs1TZQrU06KUKg=",
  #"MHJYVThihUrJcxW6wcqyOISTXIsInsdj3xK8QrZbHec="
  #]

  # The time that the browser should remember that this site is only to be
  # accessed using one of the defined keys.
  # Valid time units are "s -> seconds", "m -> minutes", "h - hours".
  max_age = "720h"

  # If enabled the PKP keys applies to all of the site's subdomains as well.
  # Default value is `false`.
  include_subdomains = false

  # By default, Pin validation failure reports aren't sent. To enable Pin validation
  # failure reporting, you need to specify the report-uri.
  report_uri = ""

  # Puts your `Public-Key-Pins` in report only mode, so that you can verify
  # and then set `pkp_report_only` value to false.
  # Don't forget to set the `report-uri` for validation.
  report_only = true
}
```

### Header: X-Permitted-Cross-Domain-Policies
Restrict Adobe Flash Player's or PDF documents access via crossdomain.xml, and this header.

Learn more:

  * https://www.owasp.org/index.php/OWASP_Secure_Headers_Project#xpcdp
  * https://www.adobe.com/devnet/adobe-media-server/articles/cross-domain-xml-for-streaming.html

Default value is `master-only`.
```cfg
xpcdp = "master-only"
```
