Title: Subject
Desc: Understanding aah subject, user and principal
Keywords: subject, user, principals
---
# aah Subject

Without question, the most important concept in aah framework is the `Subject`. Subject is just a security term that means a security-specific `view` of an application user. A aah Subject instance represents both security state and operations for a single application user.

These operations include:

  * Authorization (access control)
  * Session access
  * Logout

aah originally wanted to call it `User` since that "just makes sense", but we decided against it.: too many applications have existing APIs that already have their own User `struct`, and aah didn't want to conflict with those. Also, in the security world, the term `Subject` is actually the recognized nomenclature.

aah framework API encourages a `Subject`-centric programming paradigm for applications. When coding application logic, most application developers want to know who the currently executing user is. While the application can usually look up any user via their own mechanisms (UserService, etc), when it comes to security, the most important question is "Who is the current user?"

Application code based on only the current user/Subject is much more natural and intuitive.

## The Current Subject

You can obtain the currently executing `Subject` by using

```go
ctx.Subject()
```

After you acquire the current `Subject`, what can you do with it?

### You can access Session

If you want to make things available to the user during their current session with the application, you can get their session:

```go
session := ctx.Subject().Session
session.Set("key", "value")

// OR
ctx.Session().Set("key", "value")
```

Ok, so by now, we have a logged in user. What else can we do?

##### Let's say who they are:

```go
// get all the principals
principals := ctx.Subject().AllPrincipals()
log.Info(principals)

// print their identifying primary principal:
primaryPrincipal := ctx.Subject().PrimaryPrincipal()
log.Info(primaryPrincipal)
```

##### Let's access the primary principal value (in this case username):

```go
username := primaryPrincipal.Value
log.Info("User", username, "logged in successfully.")
```

##### We can also test to see if they have specific role or not:

```go
if ctx.Subject().HasRole("schwartz") {
    log.Info("May the Schwartz be with you!" )
} else {
    log.Info( "Hello, mere mortal." )
}
```

##### We can also see if they have a [permission](security-permissions.html) to act on a certain type of entity:

```go
if ctx.Subject().IsPermitted("lightsaber:weild") {
    log.Info("You may use a lightsaber ring.  Use it wisely.")
} else {
    log.Info("Sorry, lightsaber rings are for schwartz masters only.")
}
```

Also, we can perform an extremely powerful `instance-level` [permission](security-permissions.html) check - the ability to see if the user has the ability to access a specific instance of a type:

```go
if ctx.Subject().IsPermitted("winnebago:drive:eagle5") {
    log.Info("You are permitted to 'drive' the 'winnebago' with license plate (id) 'eagle5'.  " +
                "Here are the keys - have fun!")
} else {
    log.Info("Sorry, you aren't allowed to drive the 'eagle5' winnebago!")
}
```

Piece of cake, right?

##### Finally, when the user is done using the application, they can log out:

```go
// removes all identifying info and invalidates their session at the end of the request.
ctx.Subject().Logout()
```

This simple API constitutes 90% of what aah framework end-users will ever have to deal with when using aah security.
