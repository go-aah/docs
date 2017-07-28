Title: Tutorial of REST API Basic Auth - Security
Desc: Tutorial of how to implement REST API Basic Auth using aah framework
Keywords: basic auth, rest api, tutorial, security, aah framework
---
# Tutorial of REST API Basic Auth - Security

Goal of this tutorial is to demonstrate aah framework `Basic Auth` security implementation. It is easy and robust implementation. Perfect fit for Web and API application

Before you begin, I would request you to take a moment to read [security design](/security-design.html) of aah, [Authentication](/authentication.html) and [Authorization](/authorization.html).

aah framework supports basic auth in two ways. You can use either one.

  * Using File realm - This is good for when you have known set of users, roles and permissions (roles and permissions are optional though).
  * Dynamic way implementing interface `authc.Authenticator` and `authz.Authorizer`

### How to get the aah tutorials source code?

```bash
go get -u -d github.com/go-aah/tutorials/...
```
Just focus on following files/directory:

  * `rest-api-basic-auth/app/controllers/*`
  * `rest-api-basic-auth/app/security/*`
  * `rest-api-basic-auth/config/security.conf`

### Let's see it in the action

```bash
aah run -i github.com/go-aah/tutorials/rest-api-basic-auth
```

#### Use your favorite REST client to make a request

**Demo Users**

  * user1@example.com/welcome123
  * user2@example.com/welcome123
  * user3@example.com/welcome123 (user is in locked state)

While you're doing various requests also observe the application logs to see more information.

**API URLs**

  * http://localhost:8080/
  * http://localhost:8080/myuserinfo
  * http://localhost:8080/userinfomsg

<center>**Happy coding! Spread the word of aah web framework for Go, Thank you!**</center>
