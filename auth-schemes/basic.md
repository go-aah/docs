Title: aah Basic Auth Scheme
Desc: aah provides Basic based auth scheme with flexible options.
Keywords: basic, basic auth, auth scheme, authentication
---
# Basic Auth Scheme

aah provides easy to use Basic auth scheme and it supports two realms. Choose per use case. <br><br>

Realm | Description
---- | -----------
File | Having known set of users defined in the config file. Plus aah supports roles and permissions in the configuration.
Dynamic | Subject's data resides in datasource (such as credentials, roles, permissions). Implementing interface `authc.Authenticator` and `authz.Authorizer` to provide authentication and authorization details. Approach is similar to Form auth scheme. Refer [aah RESTFul APIs Basic Auth]({{aah-examples}}/rest-api-basic-auth) example.

### Table of Contents

  * [Configuration](#configuration)
      - [File Realm](#realm-file)
          - [Credentials config file Format](#credentials-config-file-format)
          - [Example Config of realm file](#file-realm-example-config)
      - [Dynamic Realm](#realm-dynamic)
  * [Example Config: basic_auth](#example-config-basic-auth)

## Configuration

aah supports one or more `basic` auth scheme.

### Realm: File

`basic_scheme_name { ... }` configuration goes under section `security.auth_schemes { ... }`.

```bash
# -----------------------------------------------------------------------------
# Basic auth scheme
# Choose unique key name, it gets used as route auth.
#
# Doc: https://docs.aahframework.org/auth-schemes/basic.html
# -----------------------------------------------------------------------------
basic_scheme_key {
  # Auth scheme name.
  # Supported values are `form`, `oauth2`, `basic` and `generic`.
  #
  # It is required value, no default.
  scheme = "basic"

  # Realm name is used for `Www-Authenticate` HTTP header.
  #
  # Note: Modern browsers are not respecting this values now a days.
  # However aah does its due diligence.
  realm_name = "Protected"

  # Basic auth realm file path. Path have to be absolute path.
  #
  # It is required value, no default.
  file_realm = "/path/to/basic-realm-file.conf"

  # Password encoder is used to configure password algorithm. aah validates
  # subject credentials with application provided credentials.
  #
  # Doc: https://docs.aahframework.org/password-encoders.html
  #
  # Default value is `bcrypt`.
  #password_encoder = "bcrypt"
}
```

### Credentials config file Format

Repeat this configuration section for every user. Roles and Permissions attributes are optional.

```bash
<username> {
  # Password is require value.
  password = "encrypted password value"

  # Roles attribute is optional.
  roles = ["role1", "role2"]

  # Permissions attribute is optional.
  permissions = [
    "permission1:read",
    "permission2:read,write",
  ]
}
```

### File Realm: Example Config

```bash
jeeva {
  password = "$2y$10$2A4GsJ6SmLAMvDe8XmTam.MSkKojdobBVJfIU7GiyoM.lWt.XV3H6"
  roles = ["admin", "supervisor"]
  permissions = [
    "newsletter:read,write"
  ]
}

mark {
  password = "$2y$10$2A4GsJ6SmLAMvDe8XmTam.MSkKojdobBVJfIU7GiyoM.lWt.XV3H6"
  permissions = [
    "newsletter:read,write"
  ]
}
```

### Realm: Dynamic

`basic_scheme_name { ... }` configuration goes under section `security.auth_schemes { ... }`.

```bash
# -----------------------------------------------------------------------------
# Basic auth scheme
# Choose unique key name, it gets used as route auth.
#
# Doc: https://docs.aahframework.org/auth-schemes/basic.html
# -----------------------------------------------------------------------------
basic_scheme_key {
  # Auth scheme name.
  # Supported values are `form`, `oauth2`, `basic` and `generic`.
  #
  # It is required value, no default.
  scheme = "basic"

  # Realm name is used for `Www-Authenticate` HTTP header.
  #
  # Note: Modern browsers are not respecting this values now a days.
  # However aah does its due diligence.
  realm_name = "Protected"

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
}
```

## Example Config: basic_auth

Configuration from [aah RESTFul APIs Basic Auth]({{aah-examples}}/rest-api-basic-auth) example.

```bash
# -----------------------------------------------------------------------------
# Basic auth scheme
# Choose unique key name, it gets used as route auth.
#
# Doc: https://docs.aahframework.org/auth-schemes/basic.html
# -----------------------------------------------------------------------------
basic_auth {
  scheme = "basic"
  realm_name = "Protected APIs"
  authenticator = "security/BasicAuthenticationProvider"
  authorizer = "security/BasicAuthorizationProvider"
}
```
