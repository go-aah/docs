# aah Controllers

`Controller` is responsible for handling incoming request, validate user inputs, call appropriate application logic behind and returns the response for that request. The response would be different content types based on request such as `HTML`, `JSON`, `XML`, `Text`, etc.

### Table of Contents

  * [How to create a Controller in aah framework?](#how-to-create-a-controller-in-aah-framework)
  * [Extending aah Controller](#extending-aah-controller)

## How to create a `Controller` in aah framework?

A `Controller` is any `struct` type that embeds the `aah.Context`. The controller gets access to -

  * [Request](#request)
  * [Response](#response)
  * [Reply Builder](#reply-builder)
  * [Session](#session)
  * [i18n Message](#i18n-message)
  * [Reverse URL](#reverse-url)
  * [View Arg](#view-arg)
  * [Cookies](#cookies)

```go
// Examples

// App controller
type App struct {
  *aah.Context
}

// User Controller
type User struct {
  *aah.Context
}
```

## Request

Request contains processed incoming request details such as `Params`, `Content Type`, `Locale`, `Client IP` (aka Remote IP/Address) `Accept Content Type`, `Accept Encoding`, `Raw` request object, etc. To learn more [click here](request.html).

## Response

Response implements interface of `http.CloseNotifier`, `http.Flusher`, `http.Hijacker`, for go1.8 and above `http.Pusher`) and handy methods.

**Note:**

  * It is recommend to use [`Reply`](reply-aka-response.html) builder to compose your response.
  * If you're using `cxt.Res` directly to send reply/response, don't forget to call `Reply().Done()` so that framework will not intervene with your response.

## Reply Builder

aah provides intuitive way to compose response using `Reply()` builder in chained method calls. To learn more [click here](reply-aka-response.html)

## Session

In aah framework by default session mode is `stateless`, of-course via [configuration](security-config.html#mode) you can enable `stateful`. Framework does `HMAC` sign and `AES` encryption to secure session data.

It is perfect for Web and API application-

  * `Web` requires to persist state info between HTTP request
  * `API` doesn't requires state between HTTP request.

To learn more about `Session` object [click here](session.html).

## i18n Message

i18n messages is for internalization and localization. To learn more [click here](i18n.html).

## Reverse URL

aah framework provides a convenient way to create reverse route URL via two methods.

  * `ReverseURL` - method generates the URL for given route name (defined in the routes.conf) and arguments.
  * `ReverseURLm` - method generates the URL for given route name (defined in the routes.conf) and `map` of named arguments. Remaining provided values added as URL Query parameters.

```go
// let's say route named `login` has path `/login.html`
ReverseURL("login")

// Output
"//<host-name:port-no-if-present>/login.html"


// let's say route named `user_info` has path `/v1/users/:userId`
ReverseURL("user_info", 100001)

// Output
"//<host-name:port-no-if-present>/v1/users/100001"


// let's say route named 'product_info' has path `/product/:productId`
ReverseURLm("product_info", map[string]interface{}{
  "id": "5564HFGFG56207",
  "lang": "es",
})

// Output
"//<host-name:port-no-if-present>/product/5564HFGFG56207?lang=es"
```

## View Arg

`AddViewArg` method adds the given `key` and `value` into `viewArgs`. These values are accessible on templates. Chained call is possible. Also you can supply bunch of values via `Reply().Ok().HTML(aah.Data{ ... })`.

```go
u.AddViewArg("Username", "Jeevanandam M.").
  AddViewArg("UserId", "e5f0396dfc504a949d9f9e2c511a779c")
```

## Cookies

Accessing request cookies by Name and get all the cookies.

  * `Cookie(name)` - returns the cookie for the given name otherwise not found error.
  * `Cookies()` - returns all the cookies from the request.

## Extending aah Controller

A `Controller` is any `struct` type that embeds the `aah.Context` directly or indirectly. You can extend the controller for your need.

***Note: when extending a controller via indirectly, embedding is not pointer.***

**Example**
```go
// App controller
type App struct {
  *aah.Context
}

// User Controller
type User struct {
  App
}
```
