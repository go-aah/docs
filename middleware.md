# aah Middleware

aah framework provides a way to create user-defined `Middleware` for your application very similar to Go lang standard middleware.

### Table of Contents

  * [Writing Middleware](#writing-middleware)
  * [Abort the Middleware Flow](#abort-the-middleware-flow)
  * [Bring Go lang native middleware into aah](#bring-go-lang-native-middleware-into-aah)

## Writing Middleware

To create your own custom middleware, just comply to `aah.MiddlewareFunc`. Once you created the middleware function.

You can add your middleware into aah in three ways-

  * Using standard `func init()`
  * Using `OnInit` event
  * Using `OnStart` event

```go
func myCustomMiddleware1(ctx *Context, m *Middleware) {
  // do your logic before calling next middleware

  // continue the chain
  m.Next(ctx)

  // do your logic after the continues of middleware chain
}

func myCustomMiddleware2(ctx *Context, m *Middleware) {
  // do your logic before calling next middleware

  // continue the chain
  m.Next(ctx)

  // do your logic after the continues of middleware chain
}

// Adding a Middleware into aah
func init() {
  // executed in the order of middleware added
  aah.Middlewares(myCustomMiddleware1, myCustomMiddleware2)

  // OR
  aah.OnStart(func(e *aah.Event) {
    aah.Middlewares(myCustomMiddleware1, myCustomMiddleware2)
  })
}
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
  // because `Abort()` have called before
  m.Next(ctx)
}
```

## Bring Go lang native middleware into aah

aah framework expands the possibilities via reuse/existing middleware.

  * [http.Handler](https://golang.org/pkg/net/http/#Handler)
  * [http.HandlerFunc](https://golang.org/pkg/net/http/#HandlerFunc)
  * `func(http.ResponseWriter, *http.Request)`

```go
// sample
func printURL(w http.ResponseWriter, r *http.Request) {
  fmt.Println("URL:", r.URL.Path)
}

aah.Middlewares(aah.ToMiddleware(printURL))
```
