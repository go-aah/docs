Title: aah Controllers
Desc: Controller is responsible for handling incoming request, validate user inputs, call appropriate application logic behind and returns the response to that request.
Keywords: controller, request, response, reply, view arg, cookies, extending controller
---
# Controllers

`Controller` is responsible for handling incoming request, validate user inputs, calls appropriate application logic behind and returns the response to that request. The response would be different content types based on request such as `HTML`, `JSON`, `XML`, `Text`, `Binary`, `File`, etc.

### Table of Contents

  * [How to create a Controller?](#how-to-create-a-controller-in-aah)
  * [How to create a Nested Controller?](#how-to-create-a-nested-controller)
  * [How to create a Controller Action?]()
  * [Controller provides access to](#controller-provides-access-to)

## How to create a Controller?

A `Controller` is type `struct` that embeds the `*aah.Context` (Note: this is not a standard package `context.Context`). 

```go
// AppController embeds *aah.Context
type AppController struct {
  *aah.Context
}

// UserController embeds *aah.Context
type UserController struct {
  *aah.Context
}
```

## How to create a Nested Controller?

aah supports nested controllers, the important difference is while nesting controller in another controller it would be `non-pointer` embed.

```go
// AppController embeds *aah.Context
type AppController struct {
  *aah.Context
}

// UserController embeds AppController as nested.
type UserController struct {
  AppController
}
```

## How to create a Controller Action?



## Controller provides access to

  * [Request and Response](request-and-response.html) - Use [Reply builder](reply.html) to compose response.
  * [Subject](security-subject.html) Instance (aka User) - `ctx.Subject()`
  * [Session](session.html) Instance
  * [I18n](i18n.html) messages
  * [Route URLs](#route-urls)
  * [Reply Builder](reply.html)


## Route URLs

aah provides a simple way to create route URL using `route name` defined in routes.conf.

Method Name | Description
----------- | -----------
RouteURL | Generates the URL for given route name and arguments (optional)
RouteURLNamedArgs | Generates the URL for given route name and `map` of named arguments (required). Also remaining provided values added as URL Query parameters.

### Examples 

```go
// let's say route named `login` has path `/login.html`
RouteURL("login")

// Output
"//<host-address>/login.html"


// let's say route named `user_info` has path `/v1/users/:userId`
RouteURL("user_info", 100001)

// Output
"//<host-address>/v1/users/100001"


// let's say route named 'product_info' has path `/product/:productId`
RouteURLNamedArgs("product_info", map[string]interface{}{
  "id": "5564HFGFG56207",
  "lang": "es",
})

// Output
"//<host-address>/product/5564HFGFG56207?lang=es"
```
