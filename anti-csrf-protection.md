Title: Anti-CSRF Protection
Desc: Anti-CSRF Protection by aah.
Keywords: anti-csrf, csrf, anti-xsrf, xsrf
---
# Anti-CSRF Protection

<span class="badge lb-sm">Since v0.9</span> aah provides automatic [Anti-CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet) (Cross Site Request Forgery) protection for the aah web application. This type of attack occurs when a malicious website contains a link, a form button or some JavaScript that is intended to perform some action on your website, using the credentials of a logged-in user who visits the malicious site in their browser. A related type of attack, ‘login CSRF', where an attacking site tricks a user's browser into logging into a site with someone else's credentials, is also covered.

The first defense against CSRF attacks is to ensure that GET requests (and other 'safe' methods, as defined by [RFC 7231#section-4.2.1](https://tools.ietf.org/html/rfc7231.html#section-4.2.1)) are side effect free. Requests via 'unsafe' methods, such as POST, PUT, and DELETE on HTML forms automatically protected.

Reference to [Anti-CSRF configuration](security-config.html#section-anti-csrf).

## How to use it

aah automatically protects all the HTML forms on the page. Anti-CSRF protection is enabled by default.

<div class="alert alert-info-blue">
<p><strong>Note:</strong>
<ul>
  <li>Disabling Anti-CSRF protection is not recommended for web application.</li>
  <li>Form field name and HTTP header can be customized in security configuration, refer <a href="security-config.html#section-anti-csrf">here</a></li>
</ul>
</p>
</div>

### AJAX Requests
For AJAX request you could store the Anti-CSRF token in a HTML meta tag and on each XMLHttpRequest, set a custom `X-Anti-CSRF-Token` header to the value of the meta tag. This is often easier, because many JavaScript frameworks provide hooks that allow headers to be set on every request.

#### Acquiring the token
Add HTML meta on the page.
```html
<meta name="anit_csrf_token" content="{{ anitcsrftoken . }}">
```
Then, once you have created the meta tag, you can instruct a library like jQuery to automatically add the token to all request headers.

#### Setting the token on the AJAX request
Finally, you'll have to actually set the header on your AJAX request, while protecting the CSRF token from being sent to other domains using [settings.crossDomain](https://api.jquery.com/jQuery.ajax/) in jQuery 1.5.1 and newer:

```js
function csrfSafeMethod(method) {
    // these HTTP methods do not require Anti-CSRF protection
    return (/^(GET|HEAD|OPTIONS|TRACE)$/.test(method));
}

$.ajaxSetup({
    beforeSend: function(xhr, settings) {
        if (!csrfSafeMethod(settings.type) && !this.crossDomain) {
            var antiCSRFToken = $('meta[name="anit_csrf_token"]').attr('content');
            xhr.setRequestHeader('X-Anti-CSRF-Token', antiCSRFToken);
        }
    }
});
```

## Rejected Requests
By default, a `403 Forbidden` response is sent to the user if an incoming request fails the verification performed by Anti-CSRF. This should usually only be seen when there is a genuine Cross Site Request Forgery, or when, due to a programming error, the Anti-CSRF token has not been included with a HTML form.

The error page is generic, so you may want to provide your own view for `403` error code via [Centralized Error Handler](centralized-error-handler.html).

Anti-CSRF failures are logged as warnings in the application log.

## How it works
The Anti-CSRF protection is based on the following things:

  * A Anti-CSRF cookie that is based on a random crypto secret value, which other sites will not have access to.
      - Secure Cookie (AES Encrypted and HMAC Signed) is sent with every response. In order to protect against [BREACH](http://breachattack.com/) attacks, the cipher token is not simply the secret; a random salt is prepended to the secret and used to scramble it.
      - Secure Cookie is set to `HttpOnly`, so scripting (JavaScript) cannot have access to the cookie.
      - For security reasons, the value of the crypto secret is changed each time a user logs in.
  * A hidden form field with the name `anti_csrf_token` present in all outgoing HTML forms. The value of this field is, again, the value of the secret, with a salt which is both added to it and used to scramble it. The salt is regenerated on every call to template func `anitcsrftoken` so that the form field value is changed in every such response. Also it works with multiple forms on the same page.
      - Hidden field is automatically added into `views` where `<form>...</form>` tag is present during application start. No action required.
  * For all incoming requests that are not using HTTP GET, HEAD, OPTIONS or TRACE, a Anti-CSRF cookie must be present, and the `anti_csrf_token` field must be present and correct. If it isn't, the user will get a `403 Forbidden`.
      - When validating the `anti_csrf_token` field value or `X-Anti-CSRF-Token` header value, only the secret, not the full token, is compared with the secret in the cookie value. This allows the use of ever-changing tokens. While each request may use its own token, the secret remains common to all.
  * In addition, for `HTTPS` requests, strict `referer` checking is done. This means that even if a subdomain can set or modify cookies on your domain, it can't force a user to post to your application since that request won't come from your own exact domain.
      - This also addresses a man-in-the-middle attack that's possible under HTTPS when using a session independent secret, due to the fact that HTTP `Set-Cookie` headers are (unfortunately) accepted by clients even when they are talking to a site under HTTPS. (Referer checking is not done for HTTP requests because the presence of the Referer header isn't reliable enough under HTTP.)
      - **`Upcoming`** Compare referer against configured domain. This setting supports subdomains. For example, `security.anti_csrf = ".example.com"` will allow POST requests from www.example.com and api.example.com. If the setting is not set, then the referer must match the HTTP Host header.

      - **`Upcoming`** Expanding the accepted referers beyond the current host or cookie domain can be done with the `security.anti_csrf.trusted_origins = ["domain1.com", "domain2.com"]` setting.

It deliberately ignores GET requests (and other requests that are defined as 'safe' by [RFC7231](https://tools.ietf.org/html/rfc7231.html)). These requests ought never to have any potentially dangerous side effects, and so a CSRF attack with a GET request ought to be harmless. RFC7231 defines POST, PUT, and DELETE as 'unsafe', and all other methods are also assumed to be unsafe, for maximum protection.

The Anti-CSRF protection cannot protect against man-in-the-middle attacks, so use HTTPS with [HTTP Strict Transport Security](security-config.html#header-strict-transport-security-sts-aka-hsts). It also assumes validation of the HOST header and that there aren't any cross-site scripting vulnerabilities on your site (because XSS vulnerabilities already let an attacker do anything a CSRF vulnerability allows and much worse).

## Limitations

Subdomains within a site will be able to set cookies on the client for the whole domain. By setting the cookie and using a corresponding token, subdomains will be able to circumvent the CSRF protection. The only way to avoid this is to ensure that subdomains are controlled by trusted users (or, are at least unable to set cookies). Note that even without CSRF, there are other vulnerabilities, such as session fixation, that make giving subdomains to untrusted parties a bad idea, and these vulnerabilities cannot easily be fixed with current browsers.

----
## Acknowledgement

This page documentation includes content from django and Laravel CSRF documentation.
