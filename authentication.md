Title: Authentication
Desc: Understanding aah authentication design, implementation
Keywords: authentication, security, subject, principals, credentials, form auth, basic auth, jwt, oauth, generic
---
# aah Authentication

<span class="badge lb-sm">Since v0.7</span> Authentication is the process of identity verification – system trying to validate an subject/user is who they say they are. To do so, a subject/user needs to provide some sort of proof of identity that your system understands and trust.

Familiarize yourself with aah framework security [Terminology](security-terminology.html), [Design](security-design.html) and [Permissions](security-permissions.html).

### Table of Contents

  * [Terminology to know](#terminology-to-know)
  * [Authentication Schemes](#authentication-schemes)
  * [Authenticator Interface](#authenticator-interface)
  * [How to check Subject is Authenticated on view files?](#how-to-check-subject-is-authenticated-on-view-files)
  * [How to Logout Subject?](#how-to-logout-subject)
  * [Password Encoders](password-encoders.html)


## Terminology to know

Term | Description
---- | -----------
Subject | Security specific user `view` of an application user. It can be a human being, a third-party process, a server connecting to an application, or even a cron job. Basically, it is anything or anyone communicating with application.
Principals | A subjects identifying attributes. First name, last name, email address, username, social security number, etc.
Credential | Secret data that are used to verify identities. Passwords, x509 certificates, etc.
Authenticator | An application implements the interface `authc.Authenticator` to provide authentication information for authenticating subject/user.

## Authentication Schemes

aah supports one or more Authentication Scheme (aka Auth Scheme) for the application. Auth schemes are configured in `security.conf`, configuration goes under -

```bash
# -----------------------------------------------------------------------------
# app name - Security Configuration
#
# Refer documentation to explore and customize the configurations.
#
# Doc: https://docs.aahframework.org/security-config.html
# -----------------------------------------------------------------------------
security {
  auth_schemes {
    # auth scheme config goes here
  }
}
```

Those defined auth scheme can be mapped per route basis in `routes.conf` via attributes called `default_auth` and `auth`.

aah provides ready to use Auth Scheme -

  * [Form Auth Scheme](auth-schemes/form.html)
  * [Basic Auth Scheme](auth-schemes/basic.html)
  * [OAuth2 Auth Scheme](auth-schemes/oauth2.html)
  * [Generic Auth Scheme](auth-schemes/generic.html)

## Authenticator Interface

Based on chosen auth scheme, aah user have to implement interface `security/authc.Authenticator` to provide `security/authc.AuthenticationInfo`. For e.g.: it used in Form, Basic, Generic auth scheme.

```go
// Authenticator interface is used to provide authentication information of application
// during a login.
type Authenticator interface {
	// Init method gets called by aah during an application start.
	Init(appCfg *config.Config) error

	// GetAuthenticationInfo method called by auth scheme to get subject's authentication
	// info for given authentication token.
	GetAuthenticationInfo(authcToken *AuthenticationToken) (*AuthenticationInfo, error)
}
```

<div class="alert alert-info-green">
<p><strong>Security Best Practice:</strong></p>
<p>Always give generic login failure messages to users because you do not want to aid an attacker trying to break into your system.</p>
</div>

## How to check Subject is Authenticated on view files

aah provides function `isauthenticated` to check whether subject is authenticated or not.

```html
{{ if isauthenticated . }}
<a href="/logout">Logout</a>
{{ end }}
```

**Know more** about authorization view template functions [here](authorization.html#template-view-function).

### How to Logout Subject?

To logout the subject/user from application is as follows. It clear all the subject values and session.

```go
ctx.Subject().Logout()
```
