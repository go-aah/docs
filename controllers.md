Title: Controllers
Desc: Controller is responsible for handling incoming request, validate user inputs, call appropriate application logic behind and returns the response for that request.
Keywords: controller, request, response, reply, view arg, cookies, extending controller
---
# Controllers

`Controller` is responsible for handling incoming request, validate user inputs, call appropriate application logic behind and returns the response for that request. The response would be different content types based on request such as `HTML`, `JSON`, `XML`, `Text`, `Binary`, `File`, etc.

### Table of Contents

  * [How to create a Controller in aah?](#how-to-create-a-controller-in-aah)
  * [Extending aah Controller](#extending-aah-controller)

## How to create a `Controller` in aah?

A `Controller` is any `struct` type that embeds the `aah.Context` (Note: this is not a standard package `context.Context`). The controller gets access to -

  * [Request](#request)
  * [Response](#response)
  * [Reply Builder](#reply-builder)
  * [Subject](#subject)
  * [Session](#session)
  * [i18n Message](#i18n-message)
  * [Route URL](#route-url)
  * [View Arg](#view-arg)
  * [Is Static Route](#is-static-route)

```go
// Examples

// App controller
type AppController struct {
  *aah.Context
}

// User Controller
type UserController struct {
  *aah.Context
}

// OR

// Indirectly using `aah.Context` in User Controller
type UserController struct {
  AppController
}
```

## Request

Request contains processed incoming request details such as `Params`, `Content Type`, `Locale`, `Client IP` (aka Remote IP/Address) `Accept Content Type`, `Accept Encoding`, `Raw` request object, etc.

  * To learn more about `Request` attributes and methods [click here](request-and-response.html).
  * To learn more about `Params` attributes and methods [click here](https://godoc.org/aahframework.org/ahttp.v0#Params)

## Response

Response implements interface of `http.CloseNotifier`, `http.Flusher`, `http.Hijacker`, `http.Pusher` and handy methods.

**Note:**

  * It is recommend to use [`Reply`](reply.html) builder to compose your response.
  * If you're using `cxt.Res` directly to send reply/response, don't forget to call `Reply().Done()` so that framework will not intervene with your response.

## Reply Builder

aah provides intuitive way to compose response using `Reply()` builder in chained method calls. To learn more [click here](reply.html)

## Subject

A aah Subject instance represents both security state and operations for a single application user. These operations include:

  * Authorization (access control)
  * Session access
  * Logout

Read more about [Subject](security-subject.html),

## Session

In aah default session mode is `stateless`, of-course via [configuration](security-config.html#mode) you can enable `stateful`. Framework does `HMAC` sign and `AES` encryption to secure session data.

It is perfect for Web and API application-

  * `Web` requires to persist state info between the HTTP request
  * `API` doesn't requires state info between the HTTP request.

To learn more about `Session` object [click here](session.html).

## i18n Message

i18n messages is for internationalization and localization. To learn more [click here](i18n.html).

## Route URL

aah provides a convenient way to create reverse route URL via two methods.

  * `RouteURL` - method generates the URL for given route name (defined in the `routes.conf`) and arguments.
  * `RouteURLNamedArgs` - method generates the URL for given route name (defined in the `routes.conf`) and `map` of named arguments. Remaining provided values added as URL Query parameters.

```go
// let's say route named `login` has path `/login.html`
RouteURL("login")

// Output
"//<host-name:port-no-if-present>/login.html"


// let's say route named `user_info` has path `/v1/users/:userId`
RouteURL("user_info", 100001)

// Output
"//<host-name:port-no-if-present>/v1/users/100001"


// let's say route named 'product_info' has path `/product/:productId`
RouteURLNamedArgs("product_info", map[string]interface{}{
  "id": "5564HFGFG56207",
  "lang": "es",
})

// Output
"//<host-name:port-no-if-present>/product/5564HFGFG56207?lang=es"
```

## View Arg

`AddViewArg` method adds the given `key` and `value` into View Arguments. These values are accessible on templates. Chained call is possible. Also you can supply bunch of values via `Reply().Ok().HTML(aah.Data{ ... })`.

```go
u.AddViewArg("Username", "Jeevanandam M.").
  AddViewArg("UserId", "e5f0396dfc504a949d9f9e2c511a779c")
```

## Is Static Route

aah.Context enables you to identify the current incoming request is static or application route using `IsStaticRoute()` method. For e.g: if you have register custom `NotFound` handler in the route config, this method is very handy.

```go
// true - static file route
// false - application route
a.IsStaticRoute()
```

## Extending aah Controller

A `Controller` is any `struct` type that embeds the `aah.Context` directly or indirectly. You can extend the controller for your need.

***Note: when extending a controller via indirectly, embedding is not a pointer.***

**Example**
```go
// App controller
type AppController struct {
  *aah.Context
}

// User Controller
type UserController struct {
  AppController
}
```
