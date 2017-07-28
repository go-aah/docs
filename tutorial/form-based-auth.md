Title: Tutorial of Form-based Auth - Security
Desc: Tutorial of how to implement Form-based Auth using aah framework
Keywords: form-based auth, tutorial, security, aah framework
---
# Tutorial of Form-based Auth - Security

Goal of this tutorial is to demonstrate aah framework `Form-based Auth` security implementation. It is easy and robust implementation.

Before you begin, I would request you to take a moment to read [security design](/security-design.html) of aah, [Authentication](/authentication.html) and [Authorization](/authorization.html).

### How to get the aah tutorials source code?

```bash
go get -u -d github.com/go-aah/tutorials/...
```
Just focus on following files/directory:

  * `form-based-auth/app/controllers/*`
  * `form-based-auth/app/security/*`
  * `form-based-auth/config/security.conf`
  * `form-based-auth/views/pages/*`

### Let's see it in the action

```bash
aah run -i github.com/go-aah/tutorials/form-based-auth
```

#### Now visit this URL in your browser :)

  * http://localhost:8080

Now it will take you to the login page. From there it self explanatory. Happy coding!

Navigate around using these URL for all the demo users. And observe the application logs to see more information.

  * http://localhost:8080/manage/users.html
  * http://localhost:8080/admin/dashboard.html
  * http://localhost:8080/login.html

<center>**Happy coding! Spread the word of aah web framework for Go, Thank you!**</center>
