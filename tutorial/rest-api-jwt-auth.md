Title: Tutorial of REST API JWT Auth - Security
Desc: Tutorial of how to implement REST API JWT Auth using aah framework
Keywords: jwt auth, rest api, tutorial, security, aah framework
---
# Tutorial of REST API JWT Auth - Security

Goal of this tutorial is to demonstrate aah framework's `Generic Auth` scheme to JSON Web Token (JWT) auth security implementation. It is easy and robust implementation.

Before you begin, I would request you to take a moment to read [security design](/security-design.html) of aah, [Authentication](/authentication.html) and [Authorization](/authorization.html).

Generic Auth has more possibilities, this tutorial implements JSON Web Token (JWT) using Generic auth scheme.

### How to get the aah tutorials source code?

```bash
go get -u -d github.com/go-aah/tutorials/...
```
Focus on following files/directory:

  * `rest-api-jwt-auth/app/controllers/*`
  * `rest-api-jwt-auth/app/security/*`
  * `rest-api-jwt-auth/config/security.conf`
  * `rest-api-jwt-auth/config/routes.conf`

### Let's see it in the action

```bash
aah run -i github.com/go-aah/tutorials/rest-api-jwt-auth
```

#### Use your favorite REST client to make a request

**Demo Users**

  * user1@example.com/welcome123
  * user2@example.com/welcome123
  * user3@example.com/welcome123 (user is in locked state)
  * admin@example.com/welcome123

While you're doing various requests also observe the application logs and API response to see more information.

**API Endpoints**

  * http://localhost:8080/ - Shows welcome message. (Anonymous access)
  * http://localhost:8080/v1/token - Issues JSON Web Token (JWT) for given username and password.
  * http://localhost:8080/v1/reportee/{user-email-address} - Responds user information for given user email address based on authorization. (Secured)

Get the token and pass that token via Header `Authorization: Bearer <token>`.

### Explanation

  * `controllers` package
      - Tutorial implements token `/v1/token` endpoint to issue a JSON Web Token (JWT).
      - Tutorial implements reportee endpoint to demonstrate Authentication and Authorization feature.
  * `security` package
      - Tutorial implements interface `authc.Authenticator` to validate JSON Web Token (JWT) and returns Subject's `authc.AuthenticationInfo` to Security Manager.
      - Tutorial implements interface `authz.Authorizer` to provide Subject's Roles and Permissions to Security Manager.
      - Implements JWT based configuration
  * `security.conf` has Generic Auth Scheme defined and JWT configuration.
  * `routes.conf` has `default_auth = "jwt_auth"` defined for all routes and respective `auth` attribute defined for certain route as appropriate.

<center>**Happy coding! Spread the word of aah web framework for Go, Thank you!**</center>
