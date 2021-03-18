Title: aah Error Handling
Desc: aah is versatile and seamless about handling errors, validating data and generating error responses at various levels.
Keywords: error handling, centralized error handling, default error handler
---
# aah Error Handling

aah is versatile and seamless about handling errors. It is made for modern Web and API applications. It propagates all types of errors- Validation, Authentication, Authorization, Route Not Found and Content-Negotiation.

The propagation order is listed below -

  * Controller level error handler <span class="badge lb-xs">Since v0.10.0</span>
  * Centralized error handler <span class="badge lb-xs">Since v0.8.0</span>
  * Default error handler

Benefits of aah error handling:

  * Handle errors in each controller locally
  * Achieve application level error handling
  * Send custom HTTP code with various response types after handling errors
  * Filter appropriate errors and send notification emails, slacks, etc in response.
  * Log error informations and send messages to external systems such as Kibana, Prometheus, Splunk, Sentry, Airbrake, etc.

<div class="alert alert-info-green">
<p><strong>Tip:</strong> Easy to handle errors at each controller level. If needed, it is also feasiable to propagate errors further after handling.</p>
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

### `aah.Error` Struct

```go
// Error structure is used to represent the error information in aah framework.
type Error struct {
  Reason  error       `json:"-" xml:"-"`
  Code    int         `json:"code,omitempty" xml:"code,omitempty"`
  Message string      `json:"message,omitempty" xml:"message,omitempty"`
  Data    interface{} `json:"data,omitempty" xml:"data,omitempty"`
}
```

### aah errors

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

Implement error func `aah.ErrorHandlerFunc` in an appropriate package and register via [`init.go`](/init.go-file.html) file.

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

All application errors can be handled via this function- handle by `Request Host`, `Error Code`, etc; then reply appropriate error(s).

<div class="alert alert-info-blue">
<p><strong>Note:</strong> If it is application <code>panic</code>, then field <code>aah.Error.Data</code> holds the recovered value from panic.</p>
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
  app := aah.App()
  app.OnStart(func(e *aah.Event) {
    app.SetErrorHandler(util.AppErrorHandler)
  })
}
```

## Default Error Handler

Default error handler is to handle all errors from the application and writes the reply on the wire based on `Content-Type`. If reply Content-Type is not set, then Content-Type is derived from header `Accept`; otherwise, it falls back to config value `render.default` from aah.conf. The default error handler is capable of writing response in `JSON`, `XML`, `HTML` and `Plain Text`.

Web application has special flow for HTML error page rendering. It generally works by naming the error view file, which is the same as HTTP status code.

The default error handler looks for the error view file `views/errors/<http-status-code>.<extension>`.

  * If found, then it renders that file.
      - See `views/errors/500.html` and `views/errors/404.html` for examples
  * If not found, then it uses framework default HTML error template.

<span class="badge lb-sm">Since v0.8.0</span> `aah new` command generates the web application with two error files (500.html, 404.html) under `views/errors`.

## Reply().Error(err)

`Reply().Error()` reply gets processed by Controller level, Centralized Error Handler then the reply is written on the wire.

<div class="alert alert-info-green">
<p><strong>Tip:</strong> It is highly recommended to use method <code>Reply().Error()</code> for all application error responses. So that aah user could have extensible workflow of dealing application errors.</p>
</div>

## Sample: Easy to read error stacktrace

```cfg
2018-11-23 23:41:55.255 ERROR THUMBAI admin STACKTRACE:
This is test panic message

goroutine 58 [running]:
    FILE                                                                  FUNCTION                                          LINE NO
    -------------------------------------------------------------------------------------------------------------------------------
    /usr/local/Cellar/go/1.11.1/libexec/src/runtime/panic.go              panic(...)                                        #513
    /Users/jeeva/scm/thumbai/app/controllers/admin/proxy_controller.go    admin.(*ProxyController).List()                   #39
    /usr/local/Cellar/go/1.11.1/libexec/src/reflect/value.go              reflect.Value.call(...)                           #447
    /usr/local/Cellar/go/1.11.1/libexec/src/reflect/value.go              reflect.Value.Call(...)                           #308
    /Users/jeeva/go/pkg/mod/aahframe.work@v0.12.0/middleware.go           aahframe.work.ActionMiddleware(...)               #194
    /Users/jeeva/go/pkg/mod/aahframe.work@v0.12.0/middleware.go           aahframe.work.(*Middleware).Next(...)             #78
    /Users/jeeva/go/pkg/mod/aahframe.work@v0.12.0/security.go             aahframe.work.AuthcAuthzMiddleware(...)           #72
    /Users/jeeva/go/pkg/mod/aahframe.work@v0.12.0/middleware.go           aahframe.work.(*Middleware).Next(...)             #78
    /Users/jeeva/go/pkg/mod/aahframe.work@v0.12.0/security.go             aahframe.work.AntiCSRFMiddleware(...)             #318
    /Users/jeeva/go/pkg/mod/aahframe.work@v0.12.0/middleware.go           aahframe.work.(*Middleware).Next(...)             #78
    /Users/jeeva/go/pkg/mod/aahframe.work@v0.12.0/bind.go                 aahframe.work.BindMiddleware(...)                 #95
    /Users/jeeva/go/pkg/mod/aahframe.work@v0.12.0/middleware.go           aahframe.work.(*Middleware).Next(...)             #78
    /Users/jeeva/go/pkg/mod/aahframe.work@v0.12.0/router.go               aahframe.work.RouteMiddleware(...)                #29
    /Users/jeeva/go/pkg/mod/aahframe.work@v0.12.0/middleware.go           aahframe.work.(*Middleware).Next(...)             #78
    /Users/jeeva/go/pkg/mod/aahframe.work@v0.12.0/http_engine.go          aahframe.work.(*HTTPEngine).Handle(...)           #113
    /Users/jeeva/go/pkg/mod/aahframe.work@v0.12.0/aah.go                  aahframe.work.(*Application).ServeHTTP(...)       #769
    /usr/local/Cellar/go/1.11.1/libexec/src/net/http/server.go            http.serverHandler.ServeHTTP(...)                 #2741
    /usr/local/Cellar/go/1.11.1/libexec/src/net/http/server.go            http.(*conn).serve(...)                           #1847
    /usr/local/Cellar/go/1.11.1/libexec/src/net/http/server.go            http.(*Server).Serve                              #2851

goroutine 5 [syscall]:
    FILE                                                                FUNCTION               LINE NO
    --------------------------------------------------------------------------------------------------
    /usr/local/Cellar/go/1.11.1/libexec/src/runtime/sigqueue.go         signal.signal_recv()   #139
    /usr/local/Cellar/go/1.11.1/libexec/src/os/signal/signal_unix.go    signal.loop()          #23
    /usr/local/Cellar/go/1.11.1/libexec/src/os/signal/signal_unix.go    signal.init.0          #29

goroutine 135 [IO wait]:
    FILE                                                                        FUNCTION                                                  LINE NO
    ---------------------------------------------------------------------------------------------------------------------------------------------
    /usr/local/Cellar/go/1.11.1/libexec/src/runtime/netpoll.go                  poll.runtime_pollWait(...)                                #173
    /usr/local/Cellar/go/1.11.1/libexec/src/internal/poll/fd_poll_runtime.go    poll.(*pollDesc).wait(...)                                #85
    /usr/local/Cellar/go/1.11.1/libexec/src/internal/poll/fd_poll_runtime.go    poll.(*pollDesc).waitRead(...)                            #90
    /usr/local/Cellar/go/1.11.1/libexec/src/internal/poll/fd_unix.go            poll.(*FD).Accept(...)                                    #384
    /usr/local/Cellar/go/1.11.1/libexec/src/net/fd_unix.go                      net.(*netFD).accept(...)                                  #238
    /usr/local/Cellar/go/1.11.1/libexec/src/net/tcpsock_posix.go                net.(*TCPListener).accept(...)                            #139
    /usr/local/Cellar/go/1.11.1/libexec/src/net/tcpsock.go                      net.(*TCPListener).AcceptTCP(...)                         #247
    /usr/local/Cellar/go/1.11.1/libexec/src/net/http/server.go                  http.tcpKeepAliveListener.Accept(...)                     #3232
    /usr/local/Cellar/go/1.11.1/libexec/src/crypto/tls/tls.go                   tls.(*listener).Accept(...)                               #52
    /usr/local/Cellar/go/1.11.1/libexec/src/net/http/server.go                  http.(*Server).Serve(...)                                 #2826
    /usr/local/Cellar/go/1.11.1/libexec/src/net/http/server.go                  http.(*Server).ServeTLS(...)                              #2891
    /usr/local/Cellar/go/1.11.1/libexec/src/net/http/server.go                  http.(*Server).ListenAndServeTLS(...)                     #3048
    /Users/jeeva/go/pkg/mod/aahframe.work@v0.12.0/server.go                     aahframe.work.(*Application).startHTTPS()                 #236
    /Users/jeeva/go/pkg/mod/aahframe.work@v0.12.0/server.go                     aahframe.work.(*Application).Start()                      #126
    /Users/jeeva/go/pkg/mod/aahframe.work@v0.12.0/commands.go                   created by aahframe.work.(*Application).cliCmdRun.func1   #144
```

<br>

**And `runtime.debug.strip_src_base` set to `true`**

```cfg
2018-11-23 23:53:14.160 ERROR THUMBAI admin STACKTRACE:
This is test panic message

goroutine 200 [running]:
    FILE                                             FUNCTION                                          LINE NO
    ----------------------------------------------------------------------------------------------------------
    .../runtime/panic.go                             panic(...)                                        #513
    .../app/controllers/admin/proxy_controller.go    admin.(*ProxyController).List()                   #39
    .../reflect/value.go                             reflect.Value.call(...)                           #447
    .../reflect/value.go                             reflect.Value.Call(...)                           #308
    .../aahframe.work@v0.12.0/middleware.go          aahframe.work.ActionMiddleware(...)               #194
    .../aahframe.work@v0.12.0/middleware.go          aahframe.work.(*Middleware).Next(...)             #78
    .../aahframe.work@v0.12.0/security.go            aahframe.work.AuthcAuthzMiddleware(...)           #72
    .../aahframe.work@v0.12.0/middleware.go          aahframe.work.(*Middleware).Next(...)             #78
    .../aahframe.work@v0.12.0/security.go            aahframe.work.AntiCSRFMiddleware(...)             #318
    .../aahframe.work@v0.12.0/middleware.go          aahframe.work.(*Middleware).Next(...)             #78
    .../aahframe.work@v0.12.0/bind.go                aahframe.work.BindMiddleware(...)                 #95
    .../aahframe.work@v0.12.0/middleware.go          aahframe.work.(*Middleware).Next(...)             #78
    .../aahframe.work@v0.12.0/router.go              aahframe.work.RouteMiddleware(...)                #29
    .../aahframe.work@v0.12.0/middleware.go          aahframe.work.(*Middleware).Next(...)             #78
    .../aahframe.work@v0.12.0/http_engine.go         aahframe.work.(*HTTPEngine).Handle(...)           #113
    .../aahframe.work@v0.12.0/aah.go                 aahframe.work.(*Application).ServeHTTP(...)       #788
    .../net/http/server.go                           http.serverHandler.ServeHTTP(...)                 #2741
    .../net/http/server.go                           http.(*conn).serve(...)                           #1847
    .../net/http/server.go                           http.(*Server).Serve                              #2851

goroutine 5 [syscall]:
    FILE                            FUNCTION               LINE NO
    --------------------------------------------------------------
    .../runtime/sigqueue.go         signal.signal_recv()   #139
    .../os/signal/signal_unix.go    signal.loop()          #23
    .../os/signal/signal_unix.go    signal.init.0          #29

goroutine 119 [IO wait]:
    FILE                                     FUNCTION                                                  LINE NO
    ----------------------------------------------------------------------------------------------------------
    .../runtime/netpoll.go                   poll.runtime_pollWait(...)                                #173
    .../internal/poll/fd_poll_runtime.go     poll.(*pollDesc).wait(...)                                #85
    .../internal/poll/fd_poll_runtime.go     poll.(*pollDesc).waitRead(...)                            #90
    .../internal/poll/fd_unix.go             poll.(*FD).Accept(...)                                    #384
    .../net/fd_unix.go                       net.(*netFD).accept(...)                                  #238
    .../net/tcpsock_posix.go                 net.(*TCPListener).accept(...)                            #139
    .../net/tcpsock.go                       net.(*TCPListener).AcceptTCP(...)                         #247
    .../net/http/server.go                   http.tcpKeepAliveListener.Accept(...)                     #3232
    .../crypto/tls/tls.go                    tls.(*listener).Accept(...)                               #52
    .../net/http/server.go                   http.(*Server).Serve(...)                                 #2826
    .../net/http/server.go                   http.(*Server).ServeTLS(...)                              #2891
    .../net/http/server.go                   http.(*Server).ListenAndServeTLS(...)                     #3048
    .../aahframe.work@v0.12.0/server.go      aahframe.work.(*Application).startHTTPS()                 #236
    .../aahframe.work@v0.12.0/server.go      aahframe.work.(*Application).Start()                      #126
    .../aahframe.work@v0.12.0/commands.go    created by aahframe.work.(*Application).cliCmdRun.func1   #144
```
