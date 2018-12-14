Title: aah Controllers
Desc: Controller is responsible for handling incoming request, validate user inputs, call appropriate application logic behind and returns the response to that request.
Keywords: controller, request, response, reply, view arg, cookies, extending controller
---
# Controllers

`Controller` is responsible for handling incoming request, validate user inputs, calls appropriate application logic behind and returns the response to that request. The response would be different content types based on request such as `HTML`, `JSON`, `XML`, `Text`, `Binary`, `File`, etc.

### Table of Contents

  * [How to create a Controller?](#how-to-create-a-controller-in-aah)
  * [How to create a Nested Controller?](#how-to-create-a-nested-controller)
  * [How to create a Controller Action?](#how-to-create-a-controller-action)
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

Creating an action in the controller means defining an exported method on targeted controller `struct`, define route mapping in the [routes.conf](routes-config.html#routes-configuration) and aah provides flexible way to obtain/bind [request values](request-parameters-auto-bind.html) on action method.

**Examples:** Describes the REST action defintions, once you get an understanding of definitions you could define any combinations per use case(s).

```go
// Info method returns the user information for given user identifier.
//
// Example Request Info:
//    Route Path: /:version/users/:userId
//    Request URL: /v1/users/jeeva@myjeeva.com
func (c *UserController) Info(version, userId string) {
  c.Log().Infof("API Version: %s", version)
  c.Log().Infof("User ID: %s", userId)

  // Build response per use case content-type and its values.
  // Here composing JSON reply.
  c.Reply().Ok().JSON(aah.Data{
    "version": version,
    "user_id": userId,
  })
}

// Create method creates a new user on the system with given request payload.
// 
// Example Request Info:
//    Route Path: /:version/users
//    Route Method: POST
//    Request URL: /v1/users
func (c *UserController) Create(version string, newUser *models.CreateUserRequest) {
  c.Log().Infof("API Version: %s", version)

  // aah parse and bind values into variable `newUser` based on `Content-Type`
  // such as application/json, application/xml, application/x-www-form-urlencoded, etc.
  //
  // Doc: https://docs.aahframework.org/request-parameters-auto-bind.html
  c.Log().Infof("Request Body: %#v", newUser)

  // Build response per use case content-type and its values.
  // Here composing JSON reply.
  //
  // Default response code is 200 OK
  c.Reply().JSON(aah.Data{
    "version": version,
    "create_user_request": newUser, 
  })
}

// Update method updates an user info on the system with given request payload.
// 
// Example Request Info:
//    Route Path: /:version/users/:userId
//    Route Method: PUT
//    Request URL: /v1/users/jeeva@myjeeva.com
func (c *UserController) Update(version, userId string, user *models.UpdateUserRequest) {
  c.Log().Infof("API Version: %s", version)

  // aah parse and bind values into variable `user` based on `Content-Type`
  // such as application/json, application/xml, application/x-www-form-urlencoded, etc.
  //
  // Doc: https://docs.aahframework.org/request-parameters-auto-bind.html
  c.Log().Infof("Request Body: %#v", user)

  // Build response per use case content-type and its values.
  // Here composing JSON reply.
  //
  // Default response code is 200 OK
  c.Reply().JSON(aah.Data{
    "version": version,
    "user_id": userId,
    "update_user_request": user, 
  })
}
```

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
