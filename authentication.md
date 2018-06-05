Title: Authentication
Desc: Understanding aah authentication design, implementation
Keywords: authentication, security, subject, principals, credentials, form auth, basic auth, jwt, oauth, generic
---
# aah Authentication

<span class="badge lb-sm">Since v0.7</span> Authentication is the process of identity verification – system trying to validate an subject/user is who they say they are. To do so, a subject/user needs to provide some sort of proof of identity that your system understands and trust.

Familiarize yourself with aah framework security [Terminology](security-terminology.html), [Design](security-design.html) and [Permissions](security-permissions.html).

### Table of Contents

  * [Terminology to know](#terminology-to-know)
  * [Authentication Scheme](#authentication-scheme)
  * [Authenticator Interface](#authenticator-interface)
  * [How to check Subject is Authenticated on view files?](#how-to-check-subject-is-authenticated-on-view-files)
  * [OOTB Auth Schemes](#ootb-auth-schemes)
      - [Form Auth Scheme (aka Form based Auth)](#form-auth-scheme-aka-form-based-auth)
      - [Basic Auth Scheme](#basic-auth-scheme)
      - [Generic Auth Scheme](#generic-auth-scheme)
  * [Password Encoders](password-encoders.html)


## Terminology to know

Term | Description
---- | -----------
Subject | Security specific user `view` of an application user. It can be a human being, a third-party process, a server connecting to an application, or even a cron job. Basically, it is anything or anyone communicating with application.
Principals | A subjects identifying attributes. First name, last name, email address, username, social security number, etc.
Credential | Secret data that are used to verify identities. Passwords, x509 certificates, etc.
Authenticator | An application implements the interface `authc.Authenticator` to provide authentication information for authenticating subject/user.

## Authentication Scheme

aah supports one or more Authentication Scheme (aka Auth Scheme) for the application. Auth schemes are configured in `security.conf`, configuration goes under -

```bash
security {
  auth_schemes {
    # auth scheme config goes here
  }
}
```

Those defined auth scheme can be mapped per route basis in `routes.conf` via attributes called `default_auth` and `auth`.

Auth Scheme should satisfy interface `security/scheme.Schemer`.

```go
// Schemer interface is used to create new Auth Scheme for aah framework.
type Schemer interface {
	// Init method gets called by framework during an application start.
	//
	// `keyName` is value of security auth scheme key. For e.g.:
	// `security.auth_schemes.<keyname>`.
	Init(appCfg *config.Config, keyName string) error

	// Scheme method returns the auth scheme name. For e.g.: form, basic, oauth, generic, etc.
	Scheme() string

	// SetAuthenticator method is used to set user provided Authentication implementation.
	// Value from `security.auth_schemes.<keyname>.authenticator`
	SetAuthenticator(authenticator authc.Authenticator) error

	// SetAuthorizer method is used to set user provided Authorization implementation.
	// Value from `security.auth_schemes.<keyname>.authorizer`
	SetAuthorizer(authorizer authz.Authorizer) error

	// DoAuthenticate method called by aah SecurityManager to get Subject authentication
	// information.
	DoAuthenticate(authcToken *authc.AuthenticationToken) (*authc.AuthenticationInfo, error)

	// DoAuthorizationInfo method called by aah SecurityManager to get
	// Subject's authorization information only after successful authentication.
	DoAuthorizationInfo(authcInfo *authc.AuthenticationInfo) *authz.AuthorizationInfo

	// ExtractAuthenticationToken method called by aah SecurityManager to
	// extract identity details from the HTTP request.
	ExtractAuthenticationToken(r *ahttp.Request) *authc.AuthenticationToken
}
```

**Out-of-the-Box** aah supports following Auth Scheme -

  * Form Auth Scheme
  * Basic Auth Scheme
  * Generic Auth Scheme
  * many more to come.

If you have build auth scheme for aah, please let me know. It will be listed here.

## Authenticator Interface

Based on chosen auth scheme, aah user have to implement interface `security/authc.Authenticator` to provide `security/authc.AuthenticationInfo`. For e.g.: Form Auth scheme.

```go
// Authenticator interface is implemented by user application to provide
// authentication information during authentication process.
type Authenticator interface {
	// Init method gets called by framework during an application start.
	Init(appCfg *config.Config) error

	// GetAuthenticationInfo method gets called when authentication happens for
	// user provided credentials.
	GetAuthenticationInfo(authcToken *AuthenticationToken) (*AuthenticationInfo, error)
}
```

<div class="alert alert-info-green">
<p><strong>Security Tip:</strong></p>
<p>Security best practice is to give generic login failure messages to users because you do not want to aid an attacker trying to break into your system.</p>
</div>

## How to check Subject is Authenticated on view files

aah provides function `isauthenticated` to check whether subject is authenticated or not.

```html
{{ if isauthenticated . }}
<a href="/logout">Logout</a>
{{ end }}
```

**Know more** about authorization view template funcs [here](authorization.html#template-view-function).

## OOTB Auth Schemes

### Form Auth Scheme (aka Form based Auth)

aah provides easy to use form-based auth. Following is the configuration. You have to implement `authc.Authenticator` and `authz.Authorizer` then register it the config. Refer to [Example - Form-based Auth](/tutorial/form-based-auth.html)

Configuration goes under section `security.auth_schemes { ... }`.

```bash
# HTTP Form Auth Scheme
# It is custom defined name, this is used in the routes `auth` attribute.
form_auth {
  # Auth scheme name.
  # Currently supported values are `form`, `basic` and `generic`.
  #
  # It is required value, no default.
  scheme = "form"

  # aah calls `Authenticator` to get the Subject's authentication
  # information. Then aah validates the credential using password
  # encoder.
  #
  # It is required value, no default.
  authenticator = "security/AuthenticationProvider"

  # aah calls `Authorizer` only after successful authentication; to get
  # Subject's authorization information such as Roles, Permissions.
  #
  # It is required value, no default.
  authorizer = "security/AuthorizationProvider"

  # Password encoder is used to encode the given credential and then compares
  # it with application provided credential.
  #
  # Doc: https://docs.aahframework.org/password-encoders.html
  #
  # Default value is `bcrypt`.
  #password_encoder = "bcrypt"

  # Field names are used to extract `AuthenticationToken` from the HTTP request.
  field {
    # Default value is `username`
    #identity = "username"

    # Default value is `password`
    #credential = "password"
  }

  # URLs is used when appropriate.
  url {
    # Login page URL, implement your login page and configure here.
    #
    # Default value is `/login.html`.
    #login = "/login.html"

    # Login submit URL, used to submit login credential for authentication.
    # Basically login form submit values to this URL as POST request.
    #
    # This URL can be accessed via `{{ rurl . aah_login_submit }}` on view file.
    #
    # Default value is `/login`.
    #login_submit = "/login"

    # Login failure, any failure during authentication process. aah
    # sends user to this URL and logs detailed information in the log.
    #login_failure = "/login.html?error=true"

    # Default page URL after the successful authentication. By default aah
    # redirects the user to requested page after authentication. Otherwise it
    # sends to this URL.
    #
    # Default value is `/`.
    #default_target = "/"

    # Always redirect to default URL, regardless of the requested page/URL.
    #
    # Default value is `false`.
    #always_to_default = false
  }
}
```

#### How to Logout Subject

To logout the subject/user from application is as follows. It clear all the subject values and session.

```go
ctx.Subject().Logout()
```


### Basic Auth Scheme

aah supports basic auth scheme in two ways. Choose per use case.

  * `File Realm:` It is simple, all its need is basic auth scheme realm file. This is good for when you have known set of users, roles and permissions (roles and permissions are optional though).
  * `Dynamic Realm:` It is implementing interface `authc.Authenticator` and `authz.Authorizer` to provide authentication and authorization details. Refer to [Example - REST API Basic Auth](/tutorial/rest-api-basic-auth.html), it demonstrates dynamic realm implementation.

Configuration goes under section `security.auth_schemes { ... }`.

```bash
# HTTP Basic Auth Scheme
# It is custom defined name, this is used in the routes `auth` attribute.
basic_auth {
  # Auth scheme name.
  # Currently supported values are `form`, `basic` and `generic`.
  # It is required value, no default.
  scheme = "basic"

  # Realm name is used for `Www-Authenticate` HTTP header.
  #
  # Note: Modern browsers are not respecting this values now a days.
  # However aah framework does its due diligence.
  realm_name = "Protected"

  # Basic auth realm file path. Use absolute path or
  # environment variable to provide path.
  #
  # It is required value, no default.
  file_realm = "/path/to/basic-realm-file.conf"

  # NOTE: Use either file realm or dynamic realm

  # aah calls `Authenticator` to get the Subject's authentication
  # information. Then framework validates the credential using password
  # encoder.
  #
  # It is required value when `file_realm` attribute is not configured,
  #no default.
  authenticator = "security/AuthenticationProvider"

  # aah calls `Authorizer` only after successful authentication to get
  # Subject's authorization information such as Roles and Permissions.
  #
  # It is required value when `file_realm` attribute is not configured,
  # no default.
  authorizer = "security/AuthorizationProvider"

  # Password encoder is used to encode the given credential and then compares
  # it with application provide credential.
  #
  # Doc: https://docs.aahframework.org/password-encoders.html
  #
  # Default value is `bcrypt`.
  #password_encoder = "bcrypt"
}
```

#### Basic Auth Scheme - File Realm format

Repeat this configuration block/section for every user. Roles and Permissions attributes are optional.

```bash
<username> {
  # Password is require value.
  password = "$2y$10$2A4GsJ6SmLAMvDe8XmTam.MSkKojdobBVJfIU7GiyoM.lWt.XV3H6"

  # Roles attribute is optional.
  roles = ["role1", "role2"]

  # Permissions attribute is optional.
  permissions = [
    "permission1:read",
    "permission2:read,write",
  ]
}
```

**For example:**

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
}
```

### Generic Auth Scheme

In generic auth scheme -

  * Interface `authc.Authenticator` is responsible for credential validation and return valid `authc.AuthenticationInfo` value to the framework. If subject doesn’t exist or credential doesn't match, it should `non-nil` error.
      - Such as `authc.ErrAuthenticationFailed` or `authc.ErrSubjectNotExists`; then aah would responds caller with `401 Unauthorized`.

<div class="alert alert-info-blue">
<p><strong>Note:</strong> Generic auth scheme has more possibilities. Credential validation is not done by aah.</p>
</div>

Configuration goes under section `security.auth_schemes { ... }`.

```bash
# Generic Auth Scheme
# It is custom defined name, this is used in the routes `auth` attribute.
generic_auth {
  # Auth scheme name.
  # Currently supported values are `form`, `basic` and `generic`.
  # It is required value, no default.
  scheme = "generic"

  # aah calls `Authenticator` to get the Subject's authentication
  # information. The credential validation is not done by framework.
  # It is interface implementation responsibility.
  #
  # It is required value, no default.
  authenticator = "security/AuthenticationProvider"

  # aah calls `Authorizer` only after successful authentication to get
  # Subject's authorization information such as Roles and Permissions.
  #
  # It is required value, no default.
  authorizer = "security/AuthorizationProvider"

  # Header names are used to extract `AuthenticationToken` from the HTTP request.
  header {
    # Default value is 'Authorization'
    #identity = "Authorization"

    # Optional credential header
    # Typically it's not used, however in the industry people do use it
    # Default value is empty string
    #credential = "X-AuthPass"
  }
}
```
