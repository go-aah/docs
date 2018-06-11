Title: aah Form Auth Scheme
Desc: aah provides Form based auth scheme with flexible options.
Keywords: form, auth scheme, authentication, form based auth
---
# Form Auth Scheme (aka Form based Auth)

aah provides easy to use form-based auth. Three steps to make use of Form auth scheme -

  * Implement `authc.Authenticator` to provide subject's authentication info
  * Implement `authz.Authorizer` to provide subject's [roles and permissions](/security-permissions.html)
  * Configure form auth URLs as needed

Refer [aah Form Auth]({{aah-examples}}/form-based-auth) example.

### Table of Contents

  * [Configuration](#configuration)
      - [form_scheme_key { ... }](#section-form-scheme-key)
  * [Example Config: form_auth](#example-config-form-auth)

## Configuration

aah supports one or more `form` auth scheme. For e.g.: using datasource, Active Directory, REST APIs, etc.

### Section: form_scheme_key { ... }

`form_scheme_key { ... }` configuration goes under section `security.auth_schemes { ... }`.

```bash
# -----------------------------------------------------------------------------
# Form auth scheme
# Choose unique key name, it gets used as route auth.
#
# Doc: https://docs.aahframework.org/auth-schemes/form.html
# -----------------------------------------------------------------------------
form_scheme_key {
  # Auth scheme name.
  # Supported values are `form`, `oauth2`, `basic` and `generic`.
  #
  # It is required value, no default.
  scheme = "form"

  # To provide subject's authentication info during a login flow, implement
  # interface `authc.Authenticator` and configure here. aah validates the
  # credential using configured password encoder.
  #
  # It is required value, no default.
  authenticator = "security/AuthenticationProvider"

  # To provide subject's Roles and Permissions, implement interface
  # `authz.Authorizer` and configure here.
  #
  # It is required value, no default.
  authorizer = "security/AuthorizationProvider"

  # Password encoder is used to configure password algorithm. aah validates
  # subject credentials with application provided credentials.
  #
  # Doc: https://docs.aahframework.org/password-encoders.html
  #
  # Default value is `bcrypt`.
  #password_encoder = "bcrypt"

  # To configure field names to extract `AuthenticationToken`
  # from the HTTP request.
  field {
    # Default value is `username`
    #identity = "username"

    # Default value is `password`
    #credential = "password"
  }

  # Configure form auth scheme URLs such as login page URL, login submit URL,
  # login failure URL and default target URL.
  url {
    # Login page URL.
    # Default value is `/login.html`.
    #login = "/login.html"

    # Login submit URL, used to submit login credential for authentication via
    # POST request.
    #
    # Route name is derived from user defined auth scheme key.
    # For e.g.: auth scheme key is `form_auth` then route name would be
    # `form_auth_login_submit__aah`. On login page -
    # `<form method="post" action="{{ rurl . "form_auth_login_submit__aah" }}">`
    #
    # Default value is `/login`.
    #login_submit = "/login"

    # Login failure URL, on failure during authentication process, aah
    # sends user to this URL.
    #
    # Default value is `/login.html?error=true`.
    #login_failure = "/login.html?error=true"

    # By default aah redirects user to requested page after success authentication
    # otherwise sends user to this URL.
    #
    # Default value is `/`.
    #default_target = "/"

    # Always redirect to `default_target` URL, regardless of the requested page/URL.
    #
    # Default value is `false`.
    #always_to_default = false
  }
}
```

## Example Config: form_auth

Configuration from [aah Form Auth]({{aah-examples}}/form-based-auth) example.

```bash
# -----------------------------------------------------------------------------
# Form auth scheme
# Choose unique key name, it gets used as route auth.
#
# Doc: https://docs.aahframework.org/auth-schemes/form.html
# -----------------------------------------------------------------------------
form_auth {
  scheme = "form"
  authenticator = "security/FormAuthenticationProvider"
  authorizer = "security/FormAuthorizationProvider"
}
```
