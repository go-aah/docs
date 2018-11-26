Title: Middlewares
Desc: aah framework provides a way to create user-defined Middleware for application very similar to Go lang standard middleware. Bring Go lang native middleware into aah.
Keywords: middleware, writing middleware, abort middleware, native middleware into aah
---
# Middlewares

aah provides a way to create user-defined `Middleware` for your application very similar to Go lang standard middleware.

### Table of Contents

  * [Writing Middleware](#writing-middleware)
  * [Abort the Middleware Flow](#abort-the-middleware-flow)
  * [Bring Go lang native middleware into aah](#bring-go-lang-native-middleware-into-aah)

## Writing Middleware

To create custom middleware, just comply to `aah.MiddlewareFunc`. Once you created the middleware function.

**Steps to add user-defined middleware(s) into aah**

  * Create a middleware in appropriate package
  * Go to file `<app-base-dir>/app/init.go`
  * Look for `HTTPEngine().Middlewares(`
  * Add your middleware into the chain

```go
func MyCustomMiddleware1(ctx *Context, m *Middleware) {
  // do your logic before calling next middleware

  // continue the chain
  m.Next(ctx)

  // do your logic after the continues of middleware chain
}

func MyCustomMiddleware2(ctx *Context, m *Middleware) {
  // do your logic before calling next middleware

  // continue the chain
  m.Next(ctx)

  // do your logic after the continues of middleware chain
}

// Add it on file `<app-base-dir>/app/init.go`
app.HTTPEngine().Middlewares(
  aah.RouteMiddleware,
  aah.CORSMiddleware,
  aah.BindMiddleware,
  aah.AntiCSRFMiddleware,
  aah.AuthcAuthzMiddleware,
  MyCustomMiddleware1,
  MyCustomMiddleware2,
  aah.ActionMiddleware,
)
```

## Abort the Middleware Flow

You can abort the middleware flow in two ways.

### Way One
```go
func customMiddleware(ctx *Context, m *Middleware) {
  // do your logic before and have control flag to
  // whether to call next middleware or not
  goodToProceed := false

  // continue the chain
  if goodToProceed {
    m.Next(ctx)
  }
}
```

### Way Two
```go
func customMiddleware(ctx *Context, m *Middleware) {
  // do your complex application logic and conditions
  // ...
  ctx.Abort()
  // ...


  // Even though `Next(..)` method is called, framework won't proceed further
  // because `Abort()` have been called before
  m.Next(ctx)
}
```

## Bring Go lang native middleware into aah

aah expands the possibilities via reuse/existing middlewares.

  * [http.Handler](https://golang.org/pkg/net/http/#Handler)
  * [http.HandlerFunc](https://golang.org/pkg/net/http/#HandlerFunc)
  * `func(http.ResponseWriter, *http.Request)`

```go
// PrintRequestURL middleware
func PrintRequestURL(w http.ResponseWriter, r *http.Request) {
  fmt.Println("URL:", r.URL.Path)
}

// Add it on file `<app-base-dir>/app/init.go`
app.HTTPEngine().Middlewares(
  aah.RouteMiddleware,
  aah.CORSMiddleware,
  aah.BindMiddleware,
  aah.AntiCSRFMiddleware,
  aah.AuthcAuthzMiddleware,
  aah.ToMiddleware(PrintRequestURL),
  aah.ActionMiddleware,
)
```
