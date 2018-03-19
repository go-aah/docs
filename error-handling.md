Title: Error Handling
Desc: aah provides flexible error handling and flow to handle application validation, response errors at various levels.
Keywords: error handling, centralized error handling, default error handler
---
# aah Error Handling

aah error handling is made for modern Web and API application. It propagates all the errors [Validation Error, Authentication, Authorization, Route Not Found, Content-Negotiation, etc.] in the below mentioned order. You have complete control over each error happening within your application and how it should be treated, etc.

The propagation order is -

  * Controller level error handler <span class="badge lb-xs">Since v0.10</span>
  * Centralized error handler <span class="badge lb-xs">Since v0.8</span>
  * Default error handler

Benefits of aah flexible error handling, you could -

  * Handle error at each controller locally
  * Handle error at application level
  * Handle error then send custom HTTP code with various response types.
  * Filter/process appropriate error then send notification email, slack, etc.
  * Log error information also send it to external systems. For e.g: Kibana, Prometheus, Splunk, Sentry, Airbrake, etc.

Seamlessly integrated across within framework and its flow.

<div class="alert alert-info-green">
<p><strong>Tip:</strong> You could handle error at each level, also possible to propagate to further above if needed.</p>
</div>

### Table of Contents

  * [aah Error Struct and aah Errors](#aah-error-struct-and-aah-errors)
  * [Defining Controller Level Error Handler](#defining-controller-level-error-handler)
  * [Registering Your Centralized Error Handler](#registering-your-centralized-error-handler)
  * [Default Error Handler](#default-error-handler)
  * [Reply().Error(err)](#reply-error-err)

## aah Error Struct and aah Errors

Let's take look at aah error object structure and aah errors.

#### `aah.Error` Struct
```go
// Error structure used to represent the error details in the aah framework.
type Error struct {
  Reason  error       `json:"-" xml:"-"`
  Code    int         `json:"code,omitempty" xml:"code,omitempty"`
  Message string      `json:"message,omitempty" xml:"message,omitempty"`
  Data    interface{} `json:"data,omitempty" xml:"data,omitempty"`
}
```
<br>
For framework generated errors, field `Reason` would be from these errors -
```go
var (
	ErrPanicRecovery              = errors.New("aah: panic recovery")
	ErrDomainNotFound             = errors.New("aah: domain not found")
	ErrRouteNotFound              = errors.New("aah: route not found")
	ErrStaticFileNotFound         = errors.New("aah: static file not found")
	ErrControllerOrActionNotFound = errors.New("aah: controller or action not found")
	ErrInvalidRequestParameter    = errors.New("aah: invalid request parameter")
	ErrContentTypeNotAccepted     = errors.New("aah: content type not accepted")
	ErrContentTypeNotOffered      = errors.New("aah: content type not offered")
	ErrHTTPMethodNotAllowed       = errors.New("aah: http method not allowed")
	ErrAccessDenied               = errors.New("aah: access denied")
	ErrAuthenticationFailed       = errors.New("aah: authentication failed")
	ErrGeneric                    = errors.New("aah: generic error")
	ErrValidation                 = errors.New("aah: validation error")
)
```


## Defining Controller Level Error Handler

It's very easy, just implement interface `aah.ErrorHandler` into your controller. aah would propagate all the errors happening within that controller to this handler.

```go
// ErrorHandler is interface for implement controller level error handling
type ErrorHandler interface {
	// HandleError method is to handle error on your controller
	//
	//  - Return `true`, if you have handled your errors, aah just writes the reply on the wire.
	//
	//  - Return `false`, you may or may not handled the error, aah would propagate the error
  // further onto centralized error handler, if not handled and then finally default
  // error handler would take place.
	HandleError(err *Error) bool
}
```

## Registering Your Centralized Error Handler

Just implement this error func `aah.ErrorHandlerFunc` in an appropriate package and register via [`init.go`](/init.go-file.html) file.

```go
// ErrorHandlerFunc is function type, it used to define centralized error handler
// for your application.
//
//  - Return `true`, if you have handled your errors, aah just writes the reply on the wire.
//
//  - Return `false`, you may or may not handled the error, aah would propagate the error
// further to default error handler.
type ErrorHandlerFunc func(ctx *Context, err *Error) bool
```

Then you could handle all application errors via this function. Such as handle by `Request Host`, `Error Code`, etc. Then reply appropriate error response.

<div class="alert alert-info-blue">
<p><strong>Note:</strong> If its was an application <code>panic</code> then field <code>aah.Error.Data</code> holds the recovered value from panic.</p>
</div>

### Just for an example

```go
package util

// Implement your error handler with `aah.ErrorHandlerFunc` func type.
// This is just an idea, Go with your creativity :)
func AppErrorHandler(ctx *aah.Context, err *aah.Error) bool {
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

// Register your App Error Handler at `init.go` file
func init()  {
  aah.SetErrorHandler(util.AppErrorHandler)
}
```

## Default Error Handler

Default error handler is to handle all errors from the application and writes the reply on the wire based on `Content-Type` (if reply Content-Type is not set then Content-Type is derived from header `Accept` otherwise it fallbacks to config value `render.default` from aah.conf). It is capable of writing response in `JSON`, `XML`, `HTML` and `Plain Text`.

Web application has special flow for HTML error page rendering, it typically works by naming your error view file the same as HTTP status code. Default error handler:

  * First it does lookup under `views/errors/<http-status-code>.<extension>` if found then it render that file.
      - For example: `views/errors/500.html`, `views/errors/404.html`
  * Otherwise it uses framework default HTML error template.

<span class="badge lb-sm">Since v0.8</span> `aah new` command generates the web application with two error files (500.html, 404.html) under `views/errors`.

## Reply().Error(err)

`Reply().Error()` reply is gets processed by Controller level, Centralized Error Handler then your reply is written on the wire.

It is recommended to use method `Error()` for all the application error responses.