Title: aah Form Auth Scheme
Desc: aah provides Form based auth scheme with flexible options.
Keywords: form, auth scheme, authentication, form based auth
---
# Form Auth Scheme (aka Form based Auth)

aah provides an easy to use form-based auth. Three steps are involved in making use of Form auth scheme -

  * Implement `authc.Authenticator` to provide subject's authentication info
  * Implement `authz.Authorizer` to provide subject's [roles and permissions](/security-permissions.html)
  * Configure form auth URLs as needed

Refer [aah Form Auth]({{aah_examples_url}}/form-based-auth) example.

### Table of Contents

  * [Configuration](#configuration)
      - [form_scheme_key { ... }](#section-form-scheme-key)
  * [Auto Semantic Route Configuration](#auto-semantic-route-configuration)
  * [Example Config: form_auth](#example-config-form-auth)

## Configuration

aah supports one or more `form` auth schemes such as using datasource, Active Directory, REST APIs, etc.

### Section: form_scheme_key { ... }

`form_scheme_key { ... }` configuration goes under section `security.auth_schemes { ... }`.

```bash
# -----------------------------------------------------------------------------
# Form auth scheme
# Choose a unique key name. It gets used as route auth.
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
  password_encoder = "bcrypt"

  # Configure field names to extract `AuthenticationToken`
  # from the HTTP request.
  field {
    # Default value is `username`
    identity = "username"

    # Default value is `password`
    credential = "password"
  }

  # Configure form auth scheme URLs such as login page URL, login submit URL,
  # login failure URL and default target URL.
  url {
    # Login page URL.
    # Default value is `/login.html`.
    login = "/login.html"

    # Login submit URL, used to submit login credential for authentication via
    # POST request.
    #
    # Route name is derived from the user-defined auth scheme key.
    # For example:
    # auth scheme key ~ `form_auth` then route name => `form_auth_login_submit__aah`.
    # On login page -
    #   `<form method="post" action="{{ rurl . "form_auth_login_submit__aah" }}">`
    #
    # Default value is `/login`.
    login_submit = "/login"

    # Login failure URL, on failure during authentication process, aah
    # sends user to this URL.
    #
    # Default value is `/login.html?error=true`.
    login_failure = "/login.html?error=true"

    # By default, aah redirects the user to the requested page after successful
    # authentication; otherwise, aah sends the user to this URL.
    #
    # Default value is `/`.
    default_target = "/"

    # Always redirects to `default_target` URL regardless of the requested page/URL.
    #
    # Default value is `false`.
    always_to_default = false
  }
}
```

## Auto Semantic Route Configuration

<span class="badge lb-sm">Since v0.11.0</span> aah does auto semantic route configuration of route `login submit` for all domains if auth scheme `form` is configured in it.

  * aah scans `security.auth_schemes { ... }` to check whether auth scheme `form` is configured for the domain
  * If configured then aah automatically adds the route named `<auth-scheme-name>_login_submit__aah` for the domain
  * On view files, form action URL could be accessed as `{{ rurl . "<auth-scheme-name>_login_submit__aah" }}`
  * Also it could be overriden if user configures route named `<auth-scheme-name>_login_submit__aah` then aah won't add it


## Example Config: form_auth

Configuration from [aah Form Auth]({{aah_examples_url}}/form-based-auth) example.

```bash
# -----------------------------------------------------------------------------
# Form auth scheme
# Choose a unique key name. It gets used as route auth.
#
# Doc: https://docs.aahframework.org/auth-schemes/form.html
# -----------------------------------------------------------------------------
form_auth {
  scheme = "form"
  authenticator = "security/FormAuthenticationProvider"
  authorizer = "security/FormAuthorizationProvider"
}
```

Auto Semantic route added as -

```bash
form_auth_login_submit__aah {
  # path value form `security.auth_schemes.form_auth.url.login_submit`
  path = "/login"
  method = "POST"
  auth = "form_auth"
  # max_body_size value from `request.max_body_size`
  max_body_size = "5mb" 
}
```

On view file, form action URL accessed as -

```html
<form method="post" action="{{ rurl . "form_auth_login_submit__aah" }}">
```