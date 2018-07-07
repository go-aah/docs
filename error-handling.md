Title: aah Error Handling
Desc: aah is versatile and seamless about handling errors, validating data and generating error responses at various levels.
Keywords: error handling, centralized error handling, default error handler
---
# aah Error Handling

aah is versatile and seamless about handling errors. It is made for modern Web and API applications. It propagates all types of errors- Validation, Authentication, Authorization, Route Not Found and Content-Negotiation.

The propagation order is listed below -

  * Controller level error handler <span class="badge lb-xs">Since v0.10</span>
  * Centralized error handler <span class="badge lb-xs">Since v0.8</span>
  * Default error handler

Benefits of aah error handling:

  * Handle errors in each controller locally
  * Achieve application level error handling
  * Send custom HTTP code with various response types after handling errors
  * Filter appropriate errors and send notification emails, slacks, etc in response.
  * Log error informations and send messages to external systems such as Kibana, Prometheus, Splunk, Sentry and Airbrake.

<div class="alert alert-info-green">
<p><strong>Tip:</strong> Easy to handle errors at each level. It is also amenable to propagate further if needed.</p>
</div>

### Table of Contents

  * [aah Error Struct and aah Errors](#aah-error-struct-and-aah-errors)
  * [Defining Controller Level Error Handler](#defining-controller-level-error-handler)
  * [Registering Centralized Error Handler](#registering-centralized-error-handler)
  * [Default Error Handler](#default-error-handler)
  * [Reply().Error(err)](#reply-error-err)
  * [Sample: Easy to read error stacktrace](#sample-easy-to-read-error-stacktrace)

## aah Error Struct and aah Errors

Let us take a look at aah error object structure and aah errors.

#### `aah.Error` Struct

```go
// Error structure is used to represent the error information in aah framework.
type Error struct {
  Reason  error       `json:"-" xml:"-"`
  Code    int         `json:"code,omitempty" xml:"code,omitempty"`
  Message string      `json:"message,omitempty" xml:"message,omitempty"`
  Data    interface{} `json:"data,omitempty" xml:"data,omitempty"`
}
```
<br>

#### aah errors

For framework generated errors, field `Reason` holds an appropriate text from any one of these common errors.

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
	ErrNotAuthenticated           = errors.New("aah: not authenticated")
	ErrAccessDenied               = errors.New("aah: access denied")
	ErrAuthenticationFailed       = errors.New("aah: authentication failed")
	ErrAuthorizationFailed        = errors.New("aah: authorization failed")
	ErrSessionAuthenticationInfo  = errors.New("aah: session authentication info")
	ErrUnableToGetPrincipal       = errors.New("aah: unable to get principal")
	ErrGeneric                    = errors.New("aah: generic error")
	ErrValidation                 = errors.New("aah: validation error")
	ErrRenderResponse             = errors.New("aah: render response error")
	ErrWriteResponse              = errors.New("aah: write response error")
)
```


## Defining Controller Level Error Handler

Controller level error handler can be implemented using interface `aah.ErrorHandler` on controller. aah propagates all controller specific errors to this handler.

```go
// ErrorHandler is an interface to implement controller level error handling
type ErrorHandler interface {
	// HandleError method is to handle controller specific errors
	//
	//  - Returns `true` if one or more errors are handled. aah just writes the reply on the wire.
	//
	//  - Return `false` if one or more errors could not be handled. aah propagates the error(s)
  // further onto centralized error handler. If not handled, then finally default
  // error handler takes control.
	HandleError(err *Error) bool
}
```

## Registering Centralized Error Handler

Implement error func `aah.ErrorHandlerFunc` in a suitable package and register via [`init.go`](/init.go-file.html) file.

```go
// ErrorHandlerFunc is a function type. It is used to define a centralized error handler
// for an application.
//
//  - Returns `true` when one or more errors are handled. aah just writes the reply on the wire.
//
//  - Returns `false' when one or more errors could not be handled. aah propagates the error(s)
// to default error handler.
type ErrorHandlerFunc func(ctx *Context, err *Error) bool
```

All application errors can be handled via this function- handle by `Request Host`, `Error Code`, etc; then it replies appropriate error(s).

<div class="alert alert-info-blue">
<p><strong>Note:</strong> If it is an application <code>panic</code>, then field <code>aah.Error.Data</code> holds the recovered value from panic.</p>
</div>

### For Illustration Purposes

```go
package util

// Implement error handler with `aah.ErrorHandlerFunc` func type.
// This is just an idea. Creativity can very well play here :)
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

// Register App Error Handler at `init.go` file
func init()  {
  aah.SetErrorHandler(util.AppErrorHandler)
}
```

## Default Error Handler

Default error handler is to handle all errors from the application and writes the reply on the wire based on `Content-Type`. If reply Content-Type is not set, then Content-Type is derived from header `Accept`; otherwise, it falls back to config value `render.default` from aah.conf. The default error handler is capable of writing response in `JSON`, `XML`, `HTML` and `Plain Text`.

Web application has special flow for HTML error page rendering. It generally works by naming the error view file, which is the same as HTTP status code.

The default error handler looks for the error view file `views/errors/<http-status-code>.<extension>`.

  * If found, then it renders that file.
      - See `views/errors/500.html` and `views/errors/404.html` for examples
  * If not found, then it uses framework default HTML error template.

<span class="badge lb-sm">Since v0.8</span> `aah new` command generates the web application with two error files (500.html, 404.html) under `views/errors`.

## Reply().Error(err)

`Reply().Error()` reply gets processed by Controller level, Centralized Error Handler then the reply is written on the wire.

It is highly recommended to use method `Error()` for all application error responses.

## Sample: Easy to read error stacktrace

```cfg
2018-03-10 15:35:51.111 ERROR aahwebsite sfo-aahweb-01 STACKTRACE:
This is test panic message

goroutine 148 [running]:
    FILE                                                                    FUNCTION                                        LINE NO
    -------------------------------------------------------------------------------------------------------------------------------
    /usr/local/opt/go@1.8/libexec/src/runtime/panic.go                      panic(...)                                      #489
    /Users/jeeva/go/src/github.com/go-aah/website/app/controllers/doc.go    controllers.(*DocController).VersionHome(...)   #73
    /usr/local/opt/go@1.8/libexec/src/reflect/value.go                      reflect.Value.call(...)                         #434
    /usr/local/opt/go@1.8/libexec/src/reflect/value.go                      reflect.Value.Call(...)                         #302
    /Users/jeeva/go/src/aahframework.org/aah.v0/middleware.go               aah.v0.invokeAction()                           #195
    /Users/jeeva/go/src/aahframework.org/aah.v0/middleware.go               aah.v0.ActionMiddleware(...)                    #143
    /Users/jeeva/go/src/aahframework.org/aah.v0/middleware.go               aah.v0.(*Middleware).Next(...)                  #78
    /Users/jeeva/go/src/aahframework.org/aah.v0/security.go                 aah.v0.AuthcAuthzMiddleware(...)                #96
    /Users/jeeva/go/src/aahframework.org/aah.v0/middleware.go               aah.v0.(*Middleware).Next(...)                  #78
    /Users/jeeva/go/src/aahframework.org/aah.v0/security.go                 aah.v0.AntiCSRFMiddleware(...)                  #251
    /Users/jeeva/go/src/aahframework.org/aah.v0/middleware.go               aah.v0.(*Middleware).Next(...)                  #78
    /Users/jeeva/go/src/aahframework.org/aah.v0/bind.go                     aah.v0.BindMiddleware(...)                      #121
    /Users/jeeva/go/src/aahframework.org/aah.v0/middleware.go               aah.v0.(*Middleware).Next(...)                  #78
    /Users/jeeva/go/src/aahframework.org/aah.v0/router.go                   aah.v0.CORSMiddleware(...)                      #284
    /Users/jeeva/go/src/aahframework.org/aah.v0/middleware.go               aah.v0.(*Middleware).Next(...)                  #78
    /Users/jeeva/go/src/aahframework.org/aah.v0/router.go                   aah.v0.RouteMiddleware(...)                     #39
    /Users/jeeva/go/src/aahframework.org/aah.v0/middleware.go               aah.v0.(*Middleware).Next(...)                  #78
    /Users/jeeva/go/src/aahframework.org/aah.v0/engine.go                   aah.v0.(*engine).ServeHTTP(...)                 #95
    /usr/local/opt/go@1.8/libexec/src/net/http/server.go                    http.serverHandler.ServeHTTP(...)               #2568
    /usr/local/opt/go@1.8/libexec/src/net/http/server.go                    http.(*conn).serve(...)                         #1825
    /usr/local/opt/go@1.8/libexec/src/net/http/server.go                    http.(*Server).Serve                            #2668

goroutine 1 [chan receive]:
    FILE                                                        FUNCTION      LINE NO
    ---------------------------------------------------------------------------------
    /Users/jeeva/go/src/github.com/go-aah/website/app/aah.go    main.main()   #169

goroutine 17 [syscall, locked to thread]:
    FILE                                                     FUNCTION           LINE NO
    -----------------------------------------------------------------------------------
    /usr/local/opt/go@1.8/libexec/src/runtime/asm_amd64.s    runtime.goexit()   #2197
```

**And `runtime.debug.strip_src_base` set to `true`**

```cfg
2018-03-10 15:34:24.126 ERROR aahwebsite sfo-aahweb-01 STACKTRACE:
This is test panic message

goroutine 112 [running]:
    FILE                                                            FUNCTION                                        LINE NO
    -----------------------------------------------------------------------------------------------------------------------
    #base-path#/runtime/panic.go                                    panic(...)                                      #489
    #base-path#/github.com/go-aah/website/app/controllers/doc.go    controllers.(*DocController).VersionHome(...)   #73
    #base-path#/reflect/value.go                                    reflect.Value.call(...)                         #434
    #base-path#/reflect/value.go                                    reflect.Value.Call(...)                         #302
    #base-path#/aahframework.org/aah.v0/middleware.go               aah.v0.invokeAction()                           #195
    #base-path#/aahframework.org/aah.v0/middleware.go               aah.v0.ActionMiddleware(...)                    #143
    #base-path#/aahframework.org/aah.v0/middleware.go               aah.v0.(*Middleware).Next(...)                  #78
    #base-path#/aahframework.org/aah.v0/security.go                 aah.v0.AuthcAuthzMiddleware(...)                #96
    #base-path#/aahframework.org/aah.v0/middleware.go               aah.v0.(*Middleware).Next(...)                  #78
    #base-path#/aahframework.org/aah.v0/security.go                 aah.v0.AntiCSRFMiddleware(...)                  #251
    #base-path#/aahframework.org/aah.v0/middleware.go               aah.v0.(*Middleware).Next(...)                  #78
    #base-path#/aahframework.org/aah.v0/bind.go                     aah.v0.BindMiddleware(...)                      #121
    #base-path#/aahframework.org/aah.v0/middleware.go               aah.v0.(*Middleware).Next(...)                  #78
    #base-path#/aahframework.org/aah.v0/router.go                   aah.v0.CORSMiddleware(...)                      #284
    #base-path#/aahframework.org/aah.v0/middleware.go               aah.v0.(*Middleware).Next(...)                  #78
    #base-path#/aahframework.org/aah.v0/router.go                   aah.v0.RouteMiddleware(...)                     #39
    #base-path#/aahframework.org/aah.v0/middleware.go               aah.v0.(*Middleware).Next(...)                  #78
    #base-path#/aahframework.org/aah.v0/engine.go                   aah.v0.(*engine).ServeHTTP(...)                 #95
    #base-path#/net/http/server.go                                  http.serverHandler.ServeHTTP(...)               #2568
    #base-path#/net/http/server.go                                  http.(*conn).serve(...)                         #1825
    #base-path#/net/http/server.go                                  http.(*Server).Serve                            #2668

goroutine 1 [chan receive]:
    FILE                                                FUNCTION      LINE NO
    -------------------------------------------------------------------------
    #base-path#/github.com/go-aah/website/app/aah.go    main.main()   #169

goroutine 17 [syscall, locked to thread]:
    FILE                               FUNCTION           LINE NO
    -------------------------------------------------------------
    #base-path#/runtime/asm_amd64.s    runtime.goexit()   #2197
```
