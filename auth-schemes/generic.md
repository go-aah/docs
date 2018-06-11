Title: aah Generic Auth Scheme
Desc: aah provides Generic based auth scheme with flexible options.
Keywords: generic, generic auth, auth scheme, authentication, jwt auth, jwt token
---
# Generic Auth Scheme

aah provides generic auth scheme as an extensible feature. It means authentication responsibility lies with user, aah doesn't validate the credentials and its quiet flexible. For example: Implementing JWT auth scheme, etc.

Two steps to make use of generic auth scheme -

  * Implement `authc.Authenticator` to authenticate after successful authentication provide subject's authentication info to aah
      - aah treats any non-nil error as `401 Unauthorized`
      - For e.g.: returning `authc.ErrAuthenticationFailed`, `authc.ErrSubjectNotExists` appropriately
  * Implement `authz.Authorizer` to provide subject's [roles and permissions](/security-permissions.html)

Refer [aah RESTFul API JWT auth]({{aah-examples}}/rest-api-jwt-auth) example.

### Table of Contents

  * [Configuration](#configuration)
      - [generic_scheme_key { ... }](#section-form-scheme-key)
  * [Example Config: jwt_auth](#example-config-jwt-auth)

## Configuration

aah supports one or more `generic` auth scheme.

### Section: generic_scheme_key { ... }

`generic_scheme_key { ... }` configuration goes under section `security.auth_schemes { ... }`.

```bash
# -----------------------------------------------------------------------------
# Generic auth scheme
# Choose unique key name, it gets used as route auth.
#
# Doc: https://docs.aahframework.org/auth-schemes/generic.html
# -----------------------------------------------------------------------------
generic_auth_key {
  # Auth scheme name.
  # Supported values are `form`, `oauth2`, `basic` and `generic`.
  #
  # It is required value, no default.
  scheme = "generic"

  # Implement `` To provide subject's authentication info during a login flow, implement
  # interface `authc.Authenticator` and configure here.
  #
  # It is required value, no default.
  authenticator = "security/AuthenticationProvider"

  # To provide subject's Roles and Permissions, implement interface
  # `authz.Authorizer` and configure here.
  #
  # It is required value, no default.
  authorizer = "security/AuthorizationProvider"

  # Header names are used to extract `AuthenticationToken` from the HTTP request.
  header {
    # Default value is 'Authorization'.
    #identity = "Authorization"

    # Optional credential header
    # Typically it's not used, however in the industry people do use it
    # Default value is empty string.
    #credential = "X-AuthPass"
  }
}
```

## Example Config: jwt_auth

JWT auth scheme implemented using generic auth scheme. Configuration from [aah RESTFul API JWT auth]({{aah-examples}}/rest-api-jwt-auth) example.

```bash
# -----------------------------------------------------------------------------
# Generic auth scheme
# Choose unique key name, it gets used as route auth.
#
# Doc: https://docs.aahframework.org/auth-schemes/generic.html
# -----------------------------------------------------------------------------
jwt_auth {
  scheme = "generic"
  authenticator = "security/AuthenticationProvider"
  authorizer = "security/AuthorizationProvider"

  # JWT configuration
  # Signing key and method.
  sign {
    key = "5440b995f33446bb8b17a06e65eaa7597f2c4b181aac46929dcf8025d608b1a1"
    method = "HS256"
  }
}
```
