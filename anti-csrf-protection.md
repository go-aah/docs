Title: Anti-CSRF
Desc: aah provides automatic Anti-CSRF (Cross Site Request Forgery) for the aah web application.
Keywords: anti-csrf, csrf protection, anti-xsrf, xsrf protection, csrf
---
# Anti-CSRF (Cross Site Request Forgery protection)

<span class="badge lb-sm">Since v0.9</span> aah provides automatic Anti-CSRF (Cross Site Request Forgery protection) for the aah web application. CSRF attack occurs when a malicious website contains a link, a form button or some JavaScript that is intended to perform some action on your website. CSRF uses the credentials of a logged-in user who visits the malicious site in their browser. A related type of attack, `login CSRF`, where an attacking site tricks user browser to log into a site with someone else's credentials, is also covered.

The first defense against CSRF attack is to ensure that GET requests (and other `safe` methods, as defined by [RFC 7231, 4.2.1](https://tools.ietf.org/html/rfc7231.html#section-4.2.1)) are side effect free. Requests via `unsafe` methods, such as POST, PUT, and DELETE on **HTML forms are automatically protected**.

### Table of Contents

  * [Configuration](#configuration)
  * [How to use it?](#how-to-use-it)
  * [AJAX Requests](#ajax-requests)
  * [Rejected Requests](#rejected-requests)
  * [How it works?](#how-it-works)
  * [Limitations](#limitations)

## Configuration

`anti_csrf { ... }` configuration goes under the config section `security { ... }`.

```bash
# -----------------------------------------------------------------------------
# Anti-CSRF (Cross Site Request Forgery protection)
#
# Doc: https://docs.aahframework.org/anti-csrf-protection.html
# -----------------------------------------------------------------------------
anti_csrf {
  # Enabling Anti-CSRF.
  # Default value is `true`.
  #enable = true

  # Anti-CSRF secret length. Salted cipher token length `secret_length * 2`.
  # Default value is `32`.
  #secret_length = 32

  # HTTP Header name for cipher token
  # Default value is `X-Anti-CSRF-Token`.
  #header_name = "X-Anti-CSRF-Token"

  # Form field name for cipher token
  # Default value is `anti_csrf_token`.
  #form_field_name = "anti_csrf_token"

  # Anti-CSRF secure cookie prefix
  # Default value is `aah`. Cookie name would become `aah_anti_csrf`.
  #prefix = "aah"

  # Default value is `empty` string.
  #domain = ""

  # Default value is `/`.
  #path = "/"

  # Time-to-live for Anti-CSRF secret. Valid time units are "m = minutes",
  # "h = hours" and 0.
  #
  # Default value is `24h`.
  #ttl = "24h"

  # SameSite attribute support
  # Refer to https://tools.ietf.org/html/draft-ietf-httpbis-cookie-same-site-00
  #
  # Introduced in v0.13.0 release.
  #
  # Supported values are `default`, `lax` and `strict`.
  # Default value is `default`.
  samesite = "default"

  # Anti-CSRF cookie value signing using `HMAC`. For server farm this
  # should be same in all instance. For HMAC sign & verify it recommend to use
  # key size is `32` or `64` bytes.
  #
  # Default value is `64` bytes (Command `aah new` generates strong one using `crypto/rand`).
  sign_key = "a0b4537382d31be684ad7a92537ecca96367f151309201abbdc6df2f976776c7"

  # Anti-CSRF cookie value encryption and decryption using `AES`. For server
  # farm this should be same in all instance. AES algorithm is used, valid
  # lengths are `16`, `24`, or `32` bytes to select `AES-128`, `AES-192`, or `AES-256`.
  #
  # Default value is `32` bytes (Command `aah new` generates strong one using `crypto/rand`).
  enc_key = "2976a9d457266ef2f864c1d94055f9bf"

  # Old sign key for the smooth key rotation of Anti-CSRF cookie signing into 
  # new signing key. Refer to `sign_key` for more information.
  #
  # Introduced in v0.13.0 release.
  #
  # Default value is `empty` string.
  old_sign_key = "729f9a8596d04815c0ee5667a96434fc067c7917b2ed45bbbdb817856f608b0c"

  # Old encryption key for the smooth key rotation of Anti-CSRF cookie encryption 
  # into new key. Refer to `enc_key` for more information.
  #
  # Introduced in v0.13.0 release.
  #
  # Default value is `empty` string.
  old_enc_key = "50549e23027862586ec20c3c87664edd"

  # Configure trusted origin hosts here.
  #
  # Introduced in v0.13.0 release.
  #
  # Default value is `[]` empty list.
  trusted_origins = ["example1.com", "example.com", "example3.com:8080"]
}
```

## How to use it?

aah **automatically protects all the HTML forms** on the page. Anti-CSRF is enabled by default for web applications. Explicit action is required for Web forms.

<div class="alert alert-info-blue">
<p><strong>Note:</strong>
<ul>
  <li>Disabling Anti-CSRF is not recommended for web applications.</li>
  <li>Form field name and HTTP header can be customized via configuration, refer <a href="#configuration">here</a></li>
</ul>
</p>
</div>

## AJAX Requests

For AJAX request, the Anti-CSRF token can be stored in a HTML meta tag and in each XMLHttpRequest (set a custom `X-Anti-CSRF-Token` header to the value of the meta tag). This is often easier because many JavaScript frameworks provide hooks that allow headers to be set on every request.

### Acquiring the token

Add HTML meta on the page.

```html
<meta name="anti_csrf_token" content="{{ anticsrftoken . }}">
```
Once the meta tag is created, instruct a library like jQuery to automatically add the token to all request headers.

### Setting the token on the AJAX request

Finally, set the header on the AJAX request, while protecting the CSRF token from being sent to other domains using [settings.crossDomain](https://api.jquery.com/jQuery.ajax/) in jQuery 1.5.1 and newer:

```js
function csrfSafeMethod(method) {
    // these HTTP methods do not require Anti-CSRF
    return (/^(GET|HEAD|OPTIONS|TRACE)$/.test(method));
}

$.ajaxSetup({
    beforeSend: function(xhr, settings) {
        if (!csrfSafeMethod(settings.type) && !this.crossDomain) {
            var antiCSRFToken = $('meta[name="anti_csrf_token"]').attr('content');
            xhr.setRequestHeader('X-Anti-CSRF-Token', antiCSRFToken);
        }
    }
});
```

## Rejected Requests

By default, a `403 Forbidden` response is sent to the user if an incoming request fails the verification performed by Anti-CSRF. This should usually only be seen when there is a genuine Cross Site Request Forgery, or when, due to a programming error. The Anti-CSRF token has not been included with a HTML form.

The error page is generic, so providing view for `403` error code via [Centralized Error Handler](centralized-error-handler.html) is recommended.

Anti-CSRF failures are logged as warnings in the application log.

## How it works?

The Anti-CSRF is based on the following things:

  * An Anti-CSRF cookie (that) is based on a random crypto secret value, which other sites will not have access to.
      - Secure Cookie (AES Encrypted and HMAC Signed) is sent with every response. In order to protect against [BREACH](http://breachattack.com/) attacks, the cipher token is not simply the secret; a random salt is prepended to the secret and used to scramble it.
      - Secure Cookie is set to `HttpOnly`. Thus, the scripting (JavaScript) cannot have access to the cookie.
      - For security reasons, the value of the crypto secret is changed whenever a user logs in.
  * A hidden form field with the name `anti_csrf_token` is present in all outgoing HTML forms. The value of this field is, again, the value of the secret, with a salt which is both added to it and used to scramble it. The salt is regenerated on every call to template func `anitcsrftoken` so that the form field value is changed in every such response. Also it works with multiple forms on the same page.
      - Hidden field is automatically added into while parsing `views` files where `<form>...</form>` tag is present. No action required.
  * For all incoming requests that are not using HTTP GET, HEAD, OPTIONS or TRACE, a Anti-CSRF cookie or HTTP header must be present, and the `anti_csrf_token` field must be present and correct. If it isn't, the user will get a `403 Forbidden`.
      - When validating the `anti_csrf_token` field value or `X-Anti-CSRF-Token` header value, only the secret, not the full token, is compared with the secret in the cookie value. This allows the use of ever-changing tokens. While each request may use its own token, the secret remains common to all.
  * In addition, for `HTTPS` requests, strict `referer` checking is done. This means that even if a subdomain can set or modify cookies on your domain, it can't force a user to post to your application since that request won't come from your own exact domain.
      - This also addresses a man-in-the-middle attack that's possible under HTTPS when using a session independent secret, due to the fact that HTTP `Set-Cookie` headers are (unfortunately) accepted by clients even when they are talking to a site under HTTPS. (Referer checking is not done for HTTP requests because the presence of the Referer header isn't reliable enough under HTTP.)
      - Expanding the accepted `origin` referrers beyond the current host or cookie domain can be done via  `security.anti_csrf.trusted_origins = ["domain1.com", "domain2.com"]` setting.

It deliberately ignores GET requests (and other requests that are defined as `safe` by [RFC 7231](https://tools.ietf.org/html/rfc7231.html)). These requests ought never to have any potentially dangerous side effects, and so a CSRF attack with a GET request ought to be harmless. RFC 7231 defines POST, PUT, and DELETE as `unsafe`, and all other methods are also assumed to be unsafe, for maximum protection.

The Anti-CSRF cannot protect against man-in-the-middle attacks, so use HTTPS with [HTTP Strict Transport Security](security-config.html#header-strict-transport-security-sts-aka-hsts). It also assumes validation of the HOST header and that there aren't any cross-site scripting vulnerabilities on your site (because XSS vulnerabilities already let an attacker do anything a CSRF vulnerability allows and much worse).

## Limitations

Subdomains within a site will be able to set cookies on the client for the whole domain. By setting the cookie and using a corresponding token, subdomains will be able to circumvent the CSRF protection. The only way to avoid this is to ensure that subdomains are controlled by trusted users (or, are at least unable to set cookies). Note that even without CSRF, there are other vulnerabilities, such as session fixation, that makes giving subdomains to untrusted parties a bad idea, and these vulnerabilities cannot easily be fixed with current browsers.

----

## Attribution

This documentation includes content from django and Laravel CSRF documentation.
