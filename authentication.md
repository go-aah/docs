Title: Authentication
Desc: Understanding aah authentication design, implementation
Keywords: authentication, security, subject, principals, credentials, form auth, basic auth, jwt, oauth, generic
---
# aah Authentication

<span class="badge lb-sm">Since v0.7</span> Authentication is the process of identity verification – you are trying to prove a user is who they say they are. To do so, a user needs to provide some sort of proof of identity that your system understands and trust.

Familiarize yourself with aah framework security [Terminology](security-terminology.html), [Design](security-design.html) and [Permissions](security-permissions.html).

### Table of Contents

  * [Terminology you’ll need](#terminology-you-ll-need)
  * [How to: Authentication in aah framework](#how-to-authentication-in-aah-framework)
  * [How to: Check Subject is Authenticated in view/template](#how-to-check-subject-is-authenticated-in-view-template)
  * [Auth Schemes](#auth-schemes)
      - [Form-based Auth](#form-based-auth)
      - [Basic Auth](#basic-auth)
      - [Generic Auth](#generic-auth)
  * [Password Encoders](#password-encoders)


## Terminology you’ll need

  * **Subject** - Security specific user ‘view’ of an application user. It can be a human being, a third-party process, a server connecting to you application application, or even a cron job. Basically, it is anything or anyone communicating with your application.
  * **Principals** - A subjects identifying attributes. First name, last name, social security number, username.
  * **Credential** - secret data that are used to verify identities. Passwords, x509 certificates, etc.
  * **Authenticator** - An application implements the interface `authc.Authenticator` to provide authentication information to framework

## How to: Authentication in aah framework

By nature of aah security implementation, you can have one or more `Auth Scheme` defined and can be mapped in the `routes.conf` per route basis.

  * Choose your auth schemes and configure in `security.conf`, out-of-the-box framework supports
        - [Form-based Auth](#form-based-auth)
        - [Basic Auth](#basic-auth)
        - [Generic Auth](#generic-auth) - it has many possibilities such as JWT, OAuth, etc.
  * Implement `authc.Authenticator` interface, aah recommends the package name as `security`
  * Configure auth related settings in `routes.conf`
  * For Authorization refer to [authorization guide](authorization.html)

<div class="alert alert-info-blue">
<p><strong>Security Tip:</strong></p>
<p>Security best practice is to give generic login failure messages to users because you do not want to aid an attacker trying to break into your system.</p>
</div>

## How to: Check Subject is Authenticated in view/template

aah provides function `isauthenticated` to check whether subject is authenticated or not.

```go
{{ if isauthenticated . }}
<a href="/logout">Logout</a>
{{ end }}
```

## Auth Schemes

Auth schemes are configured in `security.conf` and you can define one or more auth schemes. Configuration goes under-

```conf
security {
  auth_schemes {
    ...
  }
}
```

### Form-based Auth

aah provides easy to use form-based auth. Following is the configuration. You have to implement `authc.Authenticator` and `authz.Authorizer` then register it the config. Refer to [Tutorial - Form-based Auth](/tutorial/form-based-auth.html)

```conf
security {
  auth_schemes {
    # HTTP Form Auth Scheme
    # It is custom defined name, this is used in the routes `auth` attribute.
    form_auth {
      # Auth scheme name.
      # Supported values are `form`, `basic` and `generic`.
      # It is required value, no default.
      scheme = "form"

      # Framework calls `Authenticator` to get the Subject's authentication
      # information. Then framework validates the credential using password
      # encoder.
      # It is required value, no default.
      authenticator = "security/Authentication"

      # Framework calls `Authorizer` to get Subject's authorization information,
      # such as Roles, Permissions. Then it populates the Subject instance.
      # It is required value, no default.
      authorizer = "security/Authorization"

      # Password encoder is used to encode the given credential and then compares
      # it with application provide credential.
      # Currently supported hashing is `bcrypt`, additional hash types (upcoming).
      password_encoder {
        # Default value is `bcrypt`.
        #type = "bcrypt"
      }

      # Field names are used to extract `AuthenticationToken` from request.
      field {
        # Default value is `username`
        #identity = "username"

        # Default value is `password`
        #credential = "password"
      }

      # URLs is used when appropriate.
      url {
        # Login page URL, implement your login and configure here.
        # Default value is `/login.html`.
        #login = "/login.html"

        # Login submit URL, used to submit login credential for authentication.
        # Basically login form submits its values to this URL as POST request.
        # Default value is `/login`.
        #login_submit = "/login"

        # Login failure, any failure during authentication process. Framework
        # sends user to this URL and logs detailed information in the log.
        #login_failure = "/login.html?error=true"

        # Default page URL after the successful authentication. By default framework
        # redirects the user to requested page after authentication. Otherwise it
        # sends to this URL.
        # Default value is `/`.
        #default_target = "/"

        # Always redirect to default URL, regardless of the request page.
        # Default value is `false`.
        #always_to_default = false
      }
    }
  }
}
```

#### Configure login submit URL as virtual route

You have to configure login submit URL as Virtual routes in the `routes.conf`.

```conf
login_submit {
  path ="/login"
  method = "POST"
  controller = "VirtualFormController"
}
```

#### Logout Subject

To logout the subject/user from application is as follows. It clear all the subject values and session.

```go
ctx.Subject().Logout()
```


### Basic Auth

aah framework supports basic auth in two ways. You can use either one.

  * Using File realm - This is good for when you have known set of users, roles and permissions (roles and permissions are optional though).
  * Dynamic way implementing interface `authc.Authenticator` and `authz.Authorizer`

Refer to [Tutorial - REST API Basic Auth](/tutorial/rest-api-basic-auth.html). You can doing for web app too for certain routes. This is an example for REST API application.

```conf
security {
  auth_schemes {
    # HTTP Basic Auth Scheme
    # It is custom defined name, this is used in the routes `auth` attribute.
    basic_auth {
      # Auth scheme name.
      # Supported values are `form`, `basic` and `generic`.
      # It is required value, no default.
      scheme = "basic"

      # Realm name is used for `Www-Authenticate` HTTP header.
      # Note: Modern browsers are not respecting this values now a days.
      # However aah framework does its due diligence.
      realm_name = "Protected"

      # Basic auth realm file path. You can use absolute path or
      # environment variable to provide path.
      # No default value.
      file_realm = "/path/to/basic-realm-file.conf"

      ## NOTE: you can use either file realm or dynamic

      # Framework calls `Authenticator` to get the Subject's authentication
      # information. Then framework validates the credential using password
      # encoder.
      # It is required value when `file_realm` not configured, no default.
      authenticator = "security/Authentication"

      # Framework calls `Authorizer` to get Subject's authorization information,
      # such as Roles and Permissions. Then it populates the Subject instance.
      # It is required value when `file_realm` not configured, no default.
      authorizer = "security/Authorization"

      # Password encoder is used to encode the given credential and then compares
      # it with application provide credential.
      # Currently supported hashing is `bcrypt`, additional hash types (upcoming).
      password_encoder {
        # Default value is `bcrypt`.
        #type = "bcrypt"
      }
    }
  }
}
```

#### Basic Auth - File Realm format/structure

Repeat this configuration block/section for every user.

```conf
<username> {
  # Password is require value.
  password = "$2y$10$2A4GsJ6SmLAMvDe8XmTam.MSkKojdobBVJfIU7GiyoM.lWt.XV3H6"

  # Roles attribute is optional value.
  roles = ["role1","role2"]

  # Permissions attribute is optional value.
  permissions = [
    "permission1",
    "permission2",
  ]
}
```

For example:
```conf
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

### Generic Auth

<div class="alert alert-info-blue">
<p><strong>Note:</strong></p>
<p>Generic auth has more possibilities, so credential validation is not done by framework. That is left up to you on <code>authc.Authenticator</code> implementation. It's kept open for extensions such as JWT, OAuth, etc.</p>
</div>

**For example:** Authenticator is responsible for credential validation and return `authc.AuthenticationInfo` to the framework. If subject doesn’t exist or credential doesn't match, it returns appropriate error such as `authc.ErrAuthenticationFailed` or `authc.ErrSubjectNotExists`; then framework responds caller with `401 Unauthorized`.

```conf
security {
  auth_schemes {
    # Generic Auth Scheme
    # It is custom defined name, this is used in the routes `auth` attribute.
    generic_auth {
      # Auth scheme name.
      # Currently supported values are `form`, `basic` and `generic`.
      # It is required value, no default.
      scheme = "generic"

      # Framework calls `Authenticator` to get the Subject's authentication
      # information. The credential validation is not done by framework, it is
      # left interface implementation.
      # It is required value, no default.
      #authenticator = "security/Authentication"

      # Framework calls `Authorizer` to get Subject's authorization information,
      # such as Roles and Permissions. Then it populates the Subject instance.
      # It is required value, no default.
      #authorizer = "security/Authorization"

      # Header names are used to extract `AuthenticationToken` from request.
      header {
        # Default value is 'Authorization'
        #identity = "Authorization"

        # Optional credential header
        # Typically it's not used, however in the industry people do use it
        # Default value is empty string
        #credential = "X-AuthPass"
      }
    }
  }
}
```

## Password Encoders

aah crypto implementation housed in package [acrypto](https://godoc.org/aahframework.org/security.v0/acrypto). All password encoder implements interface `PasswordEncoder`.

Supported Hashing types:

  * `bcrypt`
  * `scrypt` (upcoming)
  * `pbkdf2` (upcoming)
