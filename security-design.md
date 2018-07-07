Title: Security Design & Implementation
Desc: Understanding aah framework security design and implementation.
Keywords: security design, security architecture, security implementation
---
# aah Security Design & Implementation

aah's design goals are to simplify application security by being intuitive and easy to use. aah core design models how most people think about application security - in the context of someone (or something) interacting with an application.

Software applications are usually designed based on user stories. That is, you'll often design user interfaces or service APIs based on how a user would (or should) interact with the software. For example, you might say, "If the user interacting with my application is logged in, I will show them a button they can click to view their account information. If they are not logged in, I will show a sign-up button."

This example statement indicates that applications are largely written to satisfy user requirements and needs. Even if the ‘user’ is another software system and not a human being, you still write code to reflect behavior based on who (or what) is currently interacting with your software.

aah reflects these concepts in its own design. By matching what is already intuitive for software developers, aah remains intuitive and easy to use in practically any application.

aah security module is `aahframework.org/security.vX`.

## Design

At high level aah security implementations are all highly modular in design. It simplifies and enables flexible configuration with pluggability. The SecurityManager implementation does not do much at all. Instead, the SecurityManager implementations mostly act as a lightweight 'container', delegating almost all behavior to nested/wrapped components.

**Components:** [SecurityManager](#securitymanager), [SessionManager](#sessionmanager), [Subject](#subject)

**Interfaces:** [Schemer](#schemer), [Authenticator](#authenticator), [PrincipalProvider](#principalprovider), [Authorizer](#authorizer), [PasswordEncoder](#passwordencoder)

### SecurityManager

aah encourages a `security.Subject`-centric programming approach, most application developers will rarely, if ever, interact with the SecurityManager directly. Even so, it is still important to know how the SecurityManager functions.

SecurityManager implementations, this includes:

  * Authentication
  * Authorization
  * Session Management
  * Auth Scheme coordination
  * Subject creation
  * Logout

### SessionManager

The `SessionManager` knows how to create and manage user Session lifecycles to provide a robust Session experience for users. The `session.Storer` interface provides an capability to use any backend of your choice for the session store. Out-of-the-box aah provides cookie and file store.

### Subject

A `security.Subject` security-specific 'view' of the entity (user, 3rd-party service, cron job, etc) currently interacting with the application.

### Schemer

The interface `scheme.Schemer` is a way to provides pluggable auth scheme for the application. You can configure one or more schemes and map it in the routes.conf per route basis also with default auth scheme if route doesn't have specific.

Ready to use schemes are `scheme.{FormAuth, BasicAuth, OAuth2Auth, GenericAuth}` direct implementation of interface `scheme.Schemer`.

```go
// Schemer interface is used to create Auth Scheme for aah.
type Schemer interface {
	// Init method gets called by aah during an application start.
	//
	// `keyName` is value of security auth scheme key.
	// 		For e.g.:
	// 			security.auth_schemes.<keyname>
	Init(appCfg *config.Config, keyName string) error

	// Key method returns auth scheme configuration KeyName.
	// For e.g: `security.auth_schemes.<keyname>`.
	Key() string

	// Scheme method returns auth scheme name. For e.g.: form, basic, oauth2, generic, etc.
	Scheme() string

	// DoAuthenticate method called by aah SecurityManager to get Subject authentication
	// information.
	DoAuthenticate(authcToken *authc.AuthenticationToken) (*authc.AuthenticationInfo, error)

	// DoAuthorizationInfo method called by aah SecurityManager to get
	// Subject's authorization information if successful authentication.
	DoAuthorizationInfo(authcInfo *authc.AuthenticationInfo) *authz.AuthorizationInfo

	// ExtractAuthenticationToken method called by aah SecurityManager to
	// extract identity details from the HTTP request.
	ExtractAuthenticationToken(r *ahttp.Request) *authc.AuthenticationToken
}
```

### Authenticator

The interface `authc.Authenticator` is pluggable authentication info provider for the application.

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

### PrincipalProvider

The interface `authc.PrincipalProvider` is pluggable Subject's principals provider for third party authentication. For e.g.: OAuth2.

```go
// PrincipalProvider interface is used to provide Subject's principals
// where authentication is done third party, for e.g. OAuth2, etc.
type PrincipalProvider interface {
	// Init method gets called by aah during an application start.
	Init(appCfg *config.Config) error

	// Principal method called by auth scheme to get Principals.
	//
	// 	For e.g: keyName is the auth scheme configuration KeyName.
	// 		 security.auth_schemes.<keyname>
	Principal(keyName string, v ess.Valuer) ([]*Principal, error)
}
```

### Authorizer

The interface `authz.Authorizer` is pluggable authorization info provider for Subject.

```go
// Authorizer interface is used to provide authorization info (roles and permissions)
// after successful authentication.
type Authorizer interface {
	// Init method gets called by aah during an application start.
	Init(cfg *config.Config) error

	// GetAuthorizationInfo method called by auth scheme after authentication
  // successful to get Subject's (aka User) access control information
  // such as roles and permissions.
	GetAuthorizationInfo(authcInfo *authc.AuthenticationInfo) *AuthorizationInfo
}
```

### PasswordEncoder

The interface `PasswordEncoder` is used to implement user password encoding such as `bcrypt`, `scrypt`, and `pbkdf2`.

```go
// PasswordEncoder interface is used to encode and compare given hash and password
// based chosen hashing type. Such as `bcrypt`, `scrypt`, and `pbkdf2`.
//
// Caution: If you're using an unsecure hashing, your application is exposed to security
// issues. Consider using `bcrypt`, `scrypt`, or `pbkdf2`. Good read about
// hashing security - https://crackstation.net/hashing-security.htm
type PasswordEncoder interface {
	Compare(hash, password []byte) bool
}
```

----
## Attribution

This documentation includes content from [Shiro Security library](https://shiro.apache.org) and [hapijs framework](https://hapijs.com/).
