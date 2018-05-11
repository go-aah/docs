Title: Security Terminology
Desc: Understand how security terminology reflected in the aah framework will make life easier.
Keywords: security, terminology
---
# aah Security Terminology

Please just take few minutes to read and understand this - `it is really really important`. The terms and concepts here are referred to everywhere in the documentation and it will greatly simplify your understanding of aah security in general.

Security can be really confusing because of the terminology used. This document will make life easier by clarifying some core concepts and you’ll see how nicely the aah framework API reflects them:


### Authentication

Authentication is the process of verifying a Subject’s identity - essentially proving that someone really is who they say they are. When an authentication attempt is successful the application can trust that the subject is guaranteed to be who the application expects.

### Authorization

Authorization (aka Access Control), is the process of determining if a Subject/User is allowed to do something or not. It is usually accomplished by inspecting and interpreting a Subject’s roles and permissions and then allowing or denying access to a requested resource or function.

### Hash

A Hash function is a one-way, irreversible conversion of an input source, sometimes called the message, into an encoded hash value, sometimes called the message digest. It is often used for passwords, digital fingerprints, or data with an underlying byte array.

### Permission
A Permission, at least as aah framework interprets it, is a statement that describes raw functionality in an application and nothing more. Permissions are the lowest-level constructs in security policies. They define only `What` the application can do. They do not describe `Who` is able to perform the actions. A Permission is only a statement of behavior, nothing more.

Some examples of permissions:

  * Open a file
  * View the ‘/user/list’ web page
  * Print documents
  * Delete the ‘jack’ user

### Principal

A `Principal` is any identifying attribute of an application user (Subject). An `identifying attribute` can be anything that makes sense to your application - a username, a surname, a given name, a social security number, a user ID, etc. That’s it - nothing crazy.

aah framework also references something I call a Subject’s primary principal. A `Primary principal` is any principal that uniquely identifies the Subject across the entire application. Ideal primary principals are things like a username or a user ID that is a RDBMS user table primary key. There is only one primary principal for users (Subject's) in an application.

### Role

The definition of a `Role` can vary based on who you talk to. In many applications it is nebulous concept at best that people use to implicitly define security policies. aah framework prefers to interpret a `Role` as simply a named collection of Permissions. That’s it - an application unique name aggregating one or more Permission declarations.

This is a more concrete definition than the implicit one used by many applications. If you choose to have your data model reflect aah framework assumption, you’ll find you will have much more power in controlling security policies.

### Session

A Session is a stateful data context associated with a single Subject/User who interacts with a software system over a period of time. Data can be added/read/removed from the Session while the subject uses the application. Sessions are terminated when the Subject/User logs out of the application or when it times out due to inactivity.

### Subject

A Subject is just fancy security term that basically means a security-specific `view` of an application user. A Subject does not always need to reflect a human being though - it can represent an external process calling your application, or perhaps a daemon system account that executes something intermittently over a period of time (such as a cron job). It is basically a representation of any entity that is doing something with the application.

----
## Attribution

This document documentation includes content from [Shiro Security library](https://shiro.apache.org).
