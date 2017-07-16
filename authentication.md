Title: Authentication
Desc: Understanding aah authentication design, implementation
Keywords: authentication, security, subject, principals, credentials, form auth, basic auth, api auth, oauth, jwt
---
# aah Authentication

Authentication is the process of identity verification – you are trying to prove a user is who they say they are. To do so, a user needs to provide some sort of proof of identity that your system understands and trust.

Familiarize yourself with aah framework security [Terminology](security-terminology.html), [Design](security-design.html) and [Permissions](security-permissions.html).

### Table of Contents

  * [Terminology you’ll need](#terminology-you-ll-need)
  * [How to Authenticate in aah framework](#how-to-authenticate-in-aah-framework)
  * [How to check Subject is Authenticated in view/template](#how-to-check-subject-is-authenticated-in-view-template)
  * [Auth Schemes](#auth-schemes)
      - [Form Authentication](#form-authentication)
      - [Basic Authentication](#basic-authentication)
      - [API Authentication](#api-authentication)


## Terminology you’ll need

  * **Subject** - Security specific user ‘view’ of an application user. It can be a human being, a third-party process, a server connecting to you application application, or even a cron job. Basically, it is anything or anyone communicating with your application.
  * **Principals** - A subjects identifying attributes. First name, last name, social security number, username.
  * **Credential** - secret data that are used to verify identities. Passwords, x509 certificates, etc.
  * **Authenticator** - An application implements the interface `authc.Authenticator` to provide authentication information to framework

## How to Authenticate in aah framework

  * Choose your auth schemes and configure in `security.conf`, out-of-the-box framework supports
      - [Form Authentication](#form-authentication)
      - [Basic Authentication](#basic-authentication)
      - [REST API Authentication](#api-authentication) - it has many possibilities such as JWT, OAuth, etc.
  * Implement `authc.Authenticator` interface, aah recommends the package name as `security`
  * Configure auth related settings in `routes.conf`
  * For Authorization refer to [authorization guide](authorization.html)

<div class="alert alert-info alert-info-light">
<p><strong>Security Tip:</strong></p>
<p>Security best practice is to give generic login failure messages to users because you do not want to aid an attacker trying to break into your system.</p>
</div>

## How to check Subject is Authenticated in view/template

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

### Form Authentication

aah provides easy to use form authentication. Following is the configuration.

```conf
security {
  auth_schemes {
    # HTTP Form Auth Scheme
    # It is custom defined name, this is used in the routes `auth` attribute.
    form_auth {
      # Auth scheme name.
      # Supported values are `form`, `basic` and `api`.
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


### Basic Authentication

aah framework supports basic auth in two ways. You can use either one.

  * Using File realm - This is good for when you have known set of users, roles and permissions.
  * Dynamic way using interface `authc.Authenticator` and `authz.Authorizer`

```conf
security {
  auth_schemes {
    # HTTP Basic Auth Scheme
    # It is custom defined name, this is used in the routes `auth` attribute.
    basic_auth {
      # Auth scheme name.
      # Supported values are `form`, `basic` and `api`.
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

#### Basic File Realm format/structure

Repeat this configuration for every user.

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

### API Authentication

**Note:** REST API authentication has more possibilities, so credential validation is not done by framework. That is left up to `authc.Authenticator` implementation. It is kept open for extensions such as JWT, OAuth, etc.

For example, Authenticator is responsible for credential validation and return `authc.AuthenticationInfo` to the framework. If subject is not exists or credential doesn't match return `nil`; then framework responds with `401 Unauthorized`.

```conf
security {
  auth_schemes {
    # REST API Authentication Scheme
    # It is custom defined name, this is used in the routes `auth` attribute.
    api_auth {
      # Auth scheme name.
      # Supported values are `form`, `basic` and `api`.
      # It is required value, no default.
      scheme = "api"

      # Framework calls `Authenticator` to get the Subject's authentication
      # information. The credential validation is not done by framework, it is
      # left interface implementation.
      # It is required value, no default.
      authenticator = "security/Authentication"

      # Framework calls `Authorizer` to get Subject's authorization information,
      # such as Roles and Permissions. Then it populates the Subject instance.
      # It is required value, no default.
      authorizer = "security/Authorization"

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
