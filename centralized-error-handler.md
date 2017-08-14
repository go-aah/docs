Title: Centralized Error Handler
Desc: aah provides Centralized error handling, it flexible and unified flow to handle application errors in the application.
Keywords: centralized error handler, centralized error handling, error handler
---
# aah Centralized Error Handler

aah framework provides Centralized Error Handling for your application. Framework utilizes this error handler across for all the HTTP error responses. Such as internal server error (panic), authentication, route not found, content-negotiation, etc. Benefits of centralized error handler:

  * You can handle and process the error then send customized HTTP response.
  * You can filter/process appropriate error and send notification to respective team, etc.
  * You can log the error information and send it to external systems, for e.g: Kibana, Prometheus, Splunk, Sentry, Airbrake, etc.


aah error handler is best for modern Web and API application. You can use custom error handler for your application also possible to fallback on default error handler. Seamlessly integrated in the request & response life cycle.

  * [aah Error Struct and Error Handler](#aah-error-struct-and-error-handler)
  * [Registering Custom Error Handler](#registering-custom-error-handler)
  * [Default Error Handler](#default-error-handler)
  * [Reply().Error(err)](#reply-error-err)

## aah Error Struct and Error Handler

Let's look at aah error object structure, error handler interface.

#### `aah.Error` Struct
```go
// Error structure used to represent the error details in the aah framework.
type Error struct {
	Code    int         `json:"code,omitempty" xml:"code,omitempty"`
	Message string      `json:"message,omitempty" xml:"message,omitempty"`
	Data    interface{} `json:"data,omitempty" xml:"data,omitempty"`
}
```

#### `aah.ErrorHandler` func type
```go
// ErrorHandler is function type used to register centralized error handling
// in aah framework.
type ErrorHandler func(ctx *aah.Context, err *aah.Error) bool
```

## Registering Custom Error Handler

You can easily register your custom handler into aah and handle by `Request Host`, `Error Code`, etc. Then reply appropriate error response.

  * Return `true` it means you have handled the error. Framework just writes the reply on the wire.
  * Return `false` it means you may or may not handled the error it fallbacks to default error handler.

_Note: If its a `panic`, the `Error.Data` holds the recovered value from panic._

```go
// Implement your error handler with `aah.ErrorHandler` func type.
// This is just an idea, Go with your creativity :)
func myCustomErrorHandler(ctx *aah.Context, err *aah.Error) bool {
  switch ctx.Req.Host {
  case "mydomain.com":
    switch err.Code {
    case 400:
      // handle bad request
    case 401:
      // handle unauthenticated request
    case 403:
      // handle permission issues
    case 500:
      // handle internal server issues

      //...
    }
  case "subdomain.mydomain.com":
    switch err.Code {
    case 400:
      // handle bad request
    case 401:
      // handle unauthenticated request
    case 403:
      // handle permission issues
    case 500:
      // handle internal server issues

      //...
    }
  }

  return true
}

// Register your custom Handler
func init()  {
  aah.SetErrorHandler(myCustomErrorHandler)
}
```

## Default Error Handler

Default error handler is to handle all errors from the application and writes the reply on the wire based on `Content-Type` (if reply Content-Type is not set then Content-Type is drevied from header `Accept` otherwise it fallbacks to config value `render.default` from aah.conf). It is capable of writing response in `JSON`, `XML`, `HTML` and `Plain Text`.

Web application has special flow for HTML error page rendering, it typically works by naming your error view file the same as HTTP status code. Default error handler:

  * First it does lookup under `views/errors/<http-status-code>.<extension>` if found then it render that file.
      - For example: `views/errors/500.html`, `views/errors/404.html`
  * Otherwise it uses default HTML error template from framework.

<span class="badge lb-sm">Since v0.8</span> `aah new` command generates the web application with two error files (500.html, 404.html) under `views/errors`.

## Reply().Error(err)

`Reply().Error()` reply is gets processed by Centralized Error Handler then reply is written on the wire.

It is recommended to use method `Error()` for all the application error responses; you will get all the benefits/possibilities as mentioned above.
