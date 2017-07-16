Title: Security Design & Implementation
Desc: Understanding aah framework security design and implementation.
Keywords: security design, architecture, security implementation
---
# aah Security Design & Implementation

aah framework's design goals are to simplify application security by being intuitive and easy to use. aah core design models how most people think about application security - in the context of someone (or something) interacting with an application.

Software applications are usually designed based on user stories. That is, you'll often design user interfaces or service APIs based on how a user would (or should) interact with the software. For example, you might say, "If the user interacting with my application is logged in, I will show them a button they can click to view their account information. If they are not logged in, I will show a sign-up button."

This example statement indicates that applications are largely written to satisfy user requirements and needs. Even if the ‘user’ is another software system and not a human being, you still write code to reflect behavior based on who (or what) is currently interacting with your software.

aah framework reflects these concepts in its own design. By matching what is already intuitive for software developers, aah framework remains intuitive and easy to use in practically any application.

aah framework security module is `aahframework.org/security.vX`.

## Design

At high level aah framework security implementations are all highly modular in design. It simplifies and enables flexible configuration with pluggability. The SecurityManager implementation does not do much at all. Instead, the SecurityManager implementations mostly act as a lightweight 'container', delegating almost all behavior to nested/wrapped components.

**Components:** [SecurityManager](#securitymanager), [SessionManager](#sessionmanager), [Subject](#subject)

**Interfaces:** [Schemer](#schemer), [Authenticator](#authenticator), [Authorizer](#authorizer), [PasswordEncoder](#passwordencoder)

### SecurityManager

aah framework encourages a `security.Subject`-centric programming approach, most application developers will rarely, if ever, interact with the SecurityManager directly. Even so, it is still important to know how the SecurityManager functions.

SecurityManager implementations, this includes:

  * Authentication
  * Authorization
  * Session Management
  * Auth Scheme coordination
  * Subject creation
  * Logout

### SessionManager

The `SessionManager` knows how to create and manage user Session lifecycles to provide a robust Session experience for users. The `session.Storer` interface provides an capability to use any backend of your choice for the session store. Out-of-the-box aah framework provides cookie and file store.

### Subject

A `security.Subject` security-specific 'view' of the entity (user, 3rd-party service, cron job, etc) currently interacting with the application.

### Schemer

The interface `security.Schemer` is way aah framework provides pluggable and configurable auth scheme for the application. You can configure one or more schemes and map it in the routes.

```go
Schemer interface {
	// Init method gets called by framework during an application start.
	Init(appCfg *config.Config, keyName string) error

	// Scheme method returns the auth scheme name. For e.g.: form, basic, api.
	Scheme() string

	// SetAuthenticator method is used to set user provided Authentication implementation.
	SetAuthenticator(authenticator authc.Authenticator) error

	// SetAuthorizer method is used to set user provided Authorization implementation.
	SetAuthorizer(authorizer authz.Authorizer) error

	// DoAuthenticate method called by SecurityManager to get Subject authentication
	// information.
	DoAuthenticate(authcToken *authc.AuthenticationToken) (*authc.AuthenticationInfo, error)

	// DoAuthorizationInfo method called by SecurityManager to get Subject authorization
	// information.
	DoAuthorizationInfo(authcInfo *authc.AuthenticationInfo) *authz.AuthorizationInfo

	// ExtractAuthenticationToken method called by SecurityManager to extract idenity details
	// from the HTTP request.
	ExtractAuthenticationToken(r *ahttp.Request) *authc.AuthenticationToken
}
```

`scheme.FormAuth`, `scheme.BasicAuth` and `scheme.ApiAuth` direct implementation of interface `scheme.Schemer`.


### Authenticator

The interface `authc.Authenticator` is your pluggable authentication implementation for the application.

```go
// Authenticator interface is implemented by user application to provide
// authentication information during authentication process.
type Authenticator interface {
	// Init method gets called by framework during an application start.
	Init(cfg *config.Config) error

	// GetAuthenticationInfo method gets called when authentication happens for
	// user provided credentials.
	GetAuthenticationInfo(authcToken *AuthenticationToken) *AuthenticationInfo
}
```

### Authorizer

The interface `authz.Authorizer` is your pluggable authorization information of Subject.

```go
// Authorizer interface is gets implemented by user application to provide Subject's
// (aka 'application user') access control information.
type Authorizer interface {
	// Init method gets called by framework during an application start.
	Init(cfg *config.Config) error

	// GetAuthorizationInfo method gets called after authentication is successful
	// to get Subject's aka User access control information such as roles and permissions.
	GetAuthorizationInfo(authcInfo *authc.AuthenticationInfo) *AuthorizationInfo
}
```

### PasswordEncoder

The interface `PasswordEncoder` is used to implement user password encoding such as `bcrypt`, `scrypt`, `pbkdf2`, `sha1`, `sha256`, etc.

**Note:**

Currently aah framework supports `bcrypt` password encoder, remaining encoders are **`upcoming`**.

```go
// PasswordEncoder interface is used to encode and compare given hash and password
// based chosen hashing type. Such as `bcrypt`, `scrypt`, `pbkdf2`, `sha1`, `sha256`, `
// sha512` and `md5`.
//
// Currently `bcrypt` is supported by aah framework, remaining encoders are `upcoming`.
//
// Caution: If you're using an unsecure hashing it may not be secured for your
// application. Consider using `bcrypt`, `scrypt`, or `pbkdf2`. Good read about
// hashing security - https://crackstation.net/hashing-security.htm
type PasswordEncoder interface {
	Compare(hash, password []byte) bool
}
```

----
## Acknowledgements

This page documentation includes content from [Shiro Security library](https://shiro.apache.org) and [hapijs framework](https://hapijs.com/).
