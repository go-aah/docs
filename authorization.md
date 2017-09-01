Title: Authorization
Desc: Understanding aah authorization design, roles, permissions, implementation
Keywords: authorization, roles, permissions, security, subject, principals
---
# aah Authorization

<span class="badge lb-sm">Since v0.7</span> Authorization, or access control, is the function of specifying access rights to resources. In other words, `who` has access to `what`.

Examples of authorization checks are:

  * Is the user allowed to look at this webpage
  * Edit this data
  * View this button
  * Or print to this printer?

Those are all decisions determining what a user has access to.

### Table of Contents

  * [Permissions Defined](#permissions-defined)
      - [Levels of permission granularity](#levels-of-permission-granularity)
  * [Roles Defined](#roles-defined)
      - [Implicit Roles](#implicit-roles)
      - [Explicit Roles](#explicit-roles)
  * [Users Defined](#users-defined)
  * [How to perform Authorization in aah](#how-to-perform-authorization-in-aah)
      - [Programmatic Authorization](#programmatic-authorization)
          - [Role Check](#role-check)
          - [Permission Check](#permission-check)
      - [Template/View function](#template-view-function)
      - Way to configure URL/Route Path based roles/permissions check **`Upcoming`**


### Elements of Authorization

Authorization has three core elements that referenced quite a bit in aah- `permissions`, `roles`, and `users`.

## Permissions Defined

Permissions are the most atomic level of a security policy and they are statements of functionality. Permissions represent what can be done in your application. A well formed permission describes a resource types and what actions are possible when you interact with those resources. Can you `open` a `door`? Can you `read` a `file`? Can you `delete` a `customer record`? Can you `push` a `button`?

Common actions for data-related resources are create, read, update, and delete, commonly referred to as CRUD.

It is important to understand that permissions do not have knowledge of `who` can perform the actions- they are just statements of `what` actions can be performed.

### Levels of permission granularity

The permissions above all specify an actions (open, read, delete, etc) on a resource (door, file, customer record, etc). In aah framework, you can define a permission to `any depth` you like. Here are a few common permission levels in order of granularity.

  * **Resource Level** - This is the broadest and easiest to build. A user can edit customer records or open doors. The resource is specified but not a specific instance of that resource.
  * **Instance Level** - The permission specifies the instance of a resource. A user can edit the customer record for Apple or open the kitchen door.
  * **Attribute Level** - The permission now specifies an attribute of an instance or resource. A user can edit the address on the Apple customer record.

For more information on `Permissions` please check out the [Understanding Permissions in aah](security-permissions.html) documentation.

## Roles Defined

In the context of Authorization, Roles are effectively a collection of permissions used to simplify the management of permissions and users. So users can be assigned roles instead of being assigned permissions directly, which can get complicated with larger user bases and more complex applications. So, for example, a bank application might have an `administrator` role or a `bank teller` role.

There are two types of roles that you need to be aware of and aah framework will support both.

### Implicit Roles

Most people view roles as what we define as an implicit role where your application `implies` a set of permissions because a user has a particular role as opposed to the role explicitly being assigned permissions or your application checking for those permissions. Role checks in code are generally a reflection of an implicit role. You can view patient data because you have the `administrator` role. You can create an account because you have the `bank teller` role. The fact that these names exist does not have a correlation to what the software can actually do. Most people use roles in this manner. It is easiest but it can create a lot of maintenance and management problems for all the but the simplest application.

### Explicit Roles

An explicit role has permissions `explicitly` assigned to it and therefore is an `explicit` collection of permissions. Permission checks in code are a reflection of an explicit role. You can view patient data because because you have the `view patient` data permission as part of your `administrator` role. You can create an account because you have the `create account` permission as part of your `bank teller` role. You can perform these actions, not because of some implicit role name based on a string but because the corresponding permission was explicitly assigned to your role.

The big benefits of explicit roles are _easier manageability_ and _lower maintenance_ of your application. If you ever need to add, remove, or change a role, you can do so without touching your source code.

**`Upcoming`** In aah framework, you’ll also be able to dynamically add, remove, or change roles at runtime and your authorization checks will always have up to date values. This means you won’t have to force users to log out and log back in order to get their new permissions.

## Users Defined

A user is the `who` of an application. In aah framework, though, the concept of a user is really the [Subject](security-subject.html) instance. aah use word `Subject` instead of user because user usually implies a human being and in aah a Subject can be anything interacting with your application– whether it be a human or a service.

Users are allowed to perform certain actions in your application through their association with roles or direct permissions. So you are able to open a customer record because you've been assigned the _open customer record_ permission, either through a role you’ve been assigned or through a direct permission assignment.

For more information on Users, aka Subjects, please check out the [Understanding Subject in aah](security-subject.html) documentation.

<div class="alert alert-info-blue">
<p><strong>Note:</strong></p>
<p>Ultimately, your <code>authz.Authorizer</code> interface implementation is what communicates with your data source (RDBMS, NoSQL, LDAP, etc). So your <code>authorizer</code> is what will tell aah framework whether or not roles or permissions exist. You have full control over how your authorization model works.</p>
</div>

## How to perform Authorization in aah

Authorization in aah framework can be handled in three ways.

  * **Programmatically** - You can perform authorization checks in your `Go` code with structures like `if` and `else` blocks.
  * **Template/View function** - You can control view page output based on roles and permissions
  * **`Upcoming`** URL Path roles/permissions - You can control URL path access based on roles and permissions

### Programmatic Authorization

Checking for permissions and roles, programmatically in your `Go` code is the traditional way of handling authorization. Here's how you can perform a permission check or role check in aah framework.

#### Role Check

This is an example of how you do a role check programmatically in your application. We want to check if a user has the `administrator` role and if they do, then we'll allow access, otherwise we won't allow it.

First we get access to the current user, the [Subject](security-subject.html). Then we pass the _administrator_ to the Subject's `.HasRole()` method. It will return `true` or `false`.

You have these methods, you can use it appropriately as required:

  * HasRole
  * HasAnyRole
  * HasAllRoles

``` go
// checking the role
if ctx.Subject().HasRole("administrator") {
    // allow access
} else {
    // don't allow access
}
```
Now a role based check is quick and easy to implement but it has a major drawback. It is implicit.

What if you just want to add, remove, or redefine a role later? You'll have to open your source code and change all your role checks to reflect the change in your security model. You’ll have to shut down the application,  open the code, test it, and then restart it every time.

In very simple applications this is probably good enough but for larger apps this can be a major problem throughout the life of your application and drive a large maintenance cost for your software.

#### Permission Check

This is an example of how you do security checks by permission. We want to check if a user has permission to print to `laserjet3000n` and if they do, then we’ll allow print, otherwise we won’t allow it. This is an example of an instance level permission or instance level authorization.

Again, first you get access to the current user, the [Subject](security-subject.html).  In this case, the domain is `printer`, the resource is `laserjet3000n`, and the action is `print`. Then we pass `printer:print:LaserJet4400n` to the Subject's `.IsPermitted()` method. It will return `true` or `false`.

```go
if ctx.Subject().IsPermitted("printer:print:LaserJet4400n") {
  // allow access
} else {
  // don't allow access
}
```

You can construct the permission string the way you want as long as your `authz.Authorizer` knows how to work with it. aah `Permission` is powerful and intuitive. If you’d like to learn more about them then check out the [Understanding Permissions in aah](security-permissions.html) documentation.


### Template/View function

aah provides following templates/view function for authorization. You have more flexibility.

  * `hasrole`
  * `hasanyrole`
  * `hasallroles`
  * `ispermitted`
  * `ispermittedall`

```html
<html>
<body>
  {{ if ispermitted . "users:manage" }}
    <a href="/manage-users.html">Manage Users</a>
  {{ end }}
</body>
</html>
```

----
## Acknowledgement

This page documentation includes content from [Shiro security library](https://shiro.apache.org).
