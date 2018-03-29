Title: Tutorial of REST API Basic Auth - Security
Desc: Tutorial of how to implement REST API Basic Auth using aah framework
Keywords: basic auth, rest api, tutorial, security, aah framework
---
# Tutorial of REST API Basic Auth - Security

Goal of this tutorial is to demonstrate aah framework `Basic Auth` security implementation. It is easy and robust implementation. Perfect fit for Web and API application

Before you begin, I would request you to take a moment to read [security design](/security-design.html) of aah, [Authentication](/authentication.html) and [Authorization](/authorization.html).

aah framework supports basic auth in two ways. You can use either one.

  * File realm - This is good for when you have known set of users, roles and permissions (roles and permissions are optional though).
  * Dynamic way of implementing interfaces `authc.Authenticator` and `authz.Authorizer`

### How to get the aah tutorials source code?

```bash
go get -u -d github.com/go-aah/tutorials
```

<br>
Focus on following files/directory:
```cfg
  rest-api-basic-auth/app/controllers/info.go
  rest-api-basic-auth/app/security/*
  rest-api-basic-auth/config/security.conf
  rest-api-basic-auth/config/routes.conf
```

### Explanation

  * `controllers` package
      - Implements reportee endpoint to demonstrate Authentication and Authorization feature.
  * `security` package
      - Implements interface `authc.Authenticator` to provide Subject's `authc.AuthenticationInfo` to Security Manager. Then Security Manager does the credentials validation.
      - Implements interface `authz.Authorizer` to provide Subject's Roles and Permissions to Security Manager.
      - Implemented security interfaces `security/BasicAuthenticationProvider` & `security/BasicAuthorizationProvider` gets registered in `security.conf`
  * `security.conf` has Basic Auth Scheme configuration and header name configuration.
  * `routes.conf` has `default_auth = "basic_auth"` defined for all routes and respective `auth` attribute defined for certain route as appropriate.

### Let's see it in the action

```cfg
aah run -i github.com/go-aah/tutorials/rest-api-basic-auth
```

#### Use your favorite REST client to make a request

**Demo Users**

  * user1@example.com/welcome123
  * user2@example.com/welcome123
  * user3@example.com/welcome123 (user is in locked state)

While you're doing various requests also observe the application logs and API response to see more information.

**API Endpoints**

  * http://localhost:8080/ - Shows welcome message. (Anonymous access)
  * http://localhost:8080/v1/reportee/:email - Responds user information for given user email address based on authorization. (Secured)

<br><br>
