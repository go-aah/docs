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
  * [How to do Authorization check?](#how-to-do-authorization-check)
      - [In Application](#in-application)
      - [In View/Template file](#in-view-template-file)

### Elements of Authorization

Authorization has three core elements that referenced quite a bit in aah- `permissions`, `roles`, and `users`.

## Permissions Defined

Permissions are the most atomic level of a security policy and they are statements of functionality. Permissions represent what can be done in your application. A well formed permission describes a resource types and what actions are possible when you interact with those resources. Can you `open` a `door`? Can you `read` a `file`? Can you `delete` a `customer record`? Can you `push` a `button`?

Common actions for data-related resources are create, read, update, and delete, commonly referred to as CRUD.

It is important to understand that permissions do not have knowledge of `who` can perform the actions- they are just statements of `what` actions can be performed.

### Levels of permission granularity

The permissions above all specify an actions (open, read, delete, etc) on a resource (door, file, customer record, etc). In aah, you can define a permission to `any depth` you like. Here are a few common permission levels in order of granularity.

  * **Resource Level** - This is the broadest and easiest to build. A user can edit customer records or open doors. The resource is specified but not a specific instance of that resource.
  * **Instance Level** - The permission specifies the instance of a resource. A user can edit the customer record for Apple or open the kitchen door.
  * **Attribute Level** - The permission now specifies an attribute of an instance or resource. A user can edit the address on the Apple customer record.

<div class="alert alert-info-blue">
<p><strong>Note:</strong>
<ul>
  <li>aah supports ACL, RBAC, ABAC, or custom (name your own). This is the capabilities of aah security design and implementation.</li>
  <li>Construct the permission string the way application want; as long as <code>authz.Authorizer</code> knows how to work with it. aah <code>Permission</code> is powerful and intuitive, learn more <a href="security-permissions.html">Understanding Permissions in aah</a>.</li>
  <li>aah supports dynamically add/remove roles and permissions at runtime with `Subject` instance. It means, you won’t have to force application users to logout and login back in order to get their new roles/permissions.</li>
</ul>
 </p>
</div>

## Roles Defined

In the context of Authorization, Roles are effectively a collection of permissions used to simplify the management of permissions and users. So users can be assigned roles instead of being assigned permissions directly, which can get complicated with larger user bases and more complex applications. So, for example, a bank application might have an `administrator` role or a `bank teller` role.

There are two types of roles that you need to be aware of and aah supports both.

### Implicit Roles

Most people view roles as what we define as an implicit role where your application `implies` a set of permissions because a user has a particular role as opposed to the role explicitly being assigned permissions or your application checking for those permissions. Role checks in code are generally a reflection of an implicit role. You can view patient data because you have the `administrator` role. You can create an account because you have the `bank teller` role. The fact that these names exist does not have a correlation to what the software can actually do. Most people use roles in this manner. It is easiest but it can create a lot of maintenance and management problems for all the but the simplest application.

### Explicit Roles

An explicit role has permissions `explicitly` assigned to it and therefore is an `explicit` collection of permissions. Permission checks in code are a reflection of an explicit role. You can view patient data because because you have the `view patient` data permission as part of your `administrator` role. You can create an account because you have the `create account` permission as part of your `bank teller` role. You can perform these actions, not because of some implicit role name based on a string but because the corresponding permission was explicitly assigned to your role.

The big benefits of explicit roles are _easier manageability_ and _lower maintenance_ of your application. If you ever need to add, remove, or change a role, you can do so without touching your source code.

## Users Defined

A user is the `who` of an application. In aah, though; the concept of a user is really the [Subject](security-subject.html) instance. aah use word `Subject` instead of user because user usually implies a human being and in aah a Subject can be anything interacting with your application– whether it be a human or a service.

Users are allowed to perform certain actions in your application through their association with roles or direct permissions. So you are able to open a customer record because you've been assigned the _open customer record_ permission, either through a role you’ve been assigned or through a direct permission assignment.

For more information on Users, aka Subjects, please check out the [Understanding Subject in aah](security-subject.html) documentation.

<div class="alert alert-info-blue">
<p><strong>Note:</strong></p>
<p>Ultimately, <code>authz.Authorizer</code> interface implementation is what communicates with data source (RDBMS, NoSQL, LDAP, etc). So <code>authorizer</code> is what will tell aah framework whether or not roles or permissions exist. aah provides  full control over how authorization model works.</p>
</div>

## How to do Authorization check?

### In Application

aah supports Authorization check by Configuration and `Go` code.

### Via Configuration

<span class="badge lb-sm">Since v0.11.0</span> Define roles and permissions per route in `routes.conf`; aah does the Authorization check automatically using provided configuration for the Route. On failure, it calls [Error Handling flow](error-handling.html).

<div class="alert alert-info-blue">
<p><strong>Note:</strong> Child Route inherits parent <code>authorization { ... }</code> config if not defined.</p>
</div>

```bash
# Authorization (access rights/privileges)
authorization {
  # Satisfy value is used to evaluate the result of `roles` and `permissions` attribute.
  #
  # Possible values are
  #   - `either` => either roles or permissions should satisfy for Subject
  #   - `both`   => both roles and permissions should satisfy for Subject
  #
  # Default value is `either`.
  satisfy = "either"

  # Roles (Optional config)
  # Result of role function is evaluated as AND.
  #
  # Supported functions are `hasrole`, `hasanyrole`, `hasallroles`.
  # Roles are comma(,) separated values.
  roles = [
    "hasrole(manager)",
    "hasanyrole(role1, role2, role3)"
  ]

  # Permissions (Optional config)
  # Doc: https://docs.aahframework.org/security-permissions.html
  #
  # Result of permission function is evaluated as AND.
  #
  # Supported functions are `ispermitted`, `ispermittedall`.
  # Permissions are pipe(|) separated values.
  permissions = [
    "ispermitted(newsletter:read,write)",
    "ispermittedall(newsletter:read,write | newsletter:12345)"
  ]
}
```

### Via Go code

The user can check the roles and permissions programmatically on `Go` code with structures like `if` and `else` blocks.

Firstly, get access to the current [Subject/User](security-subject.html). Then, call appropriate authorization methods with `Subject` instance.

```go
subject := ctx.Subject()
```

**Authorization Methods**

Method | Description
------ | -----------
HasRole | Returns true if Subject has the specified role, otherwise false.
HasAnyRole | Returns true if Subject has any-one of the specified roles, otherwise false.
HasAllRoles | Returns true if the Subject has all of the specified roles, otherwise false.
IsPermitted | Returns true if Subject is permitted to perform an action or to access a resource summarized by the specified permission string, otherwise false.
IsPermittedAll | Returns true if the Subject implies all of the specified permission strings, otherwise false.

**For Example-**

```go
// Checks if Subject has role `administrator`.
if ctx.Subject().HasRole("administrator") {
  // allow access
} else {
  // don't allow access
}

// Checks if Subject has permission to read and edit newsletter.
if ctx.Subject().IsPermitted("newsletter:read,write") {
  // allow access
} else {
  // don't allow access
}
```

### In View/Template file

aah provides templates/view functions to perform authorization in a flexible way on view files.

  * `hasrole`
  * `hasanyrole`
  * `hasallroles`
  * `ispermitted`
  * `ispermittedall`

**For Example:**

Checks if Subject has permission to manage users. If true, then shows the link.

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
## Attribution

This documentation includes content from [Shiro security library](https://shiro.apache.org).
