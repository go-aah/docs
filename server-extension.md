# aah Server Extension

aah server exposes the App and Request life cycle stages as server events, i.e. called Server Extension Point. Function signature is same as events (`aah.EventCallbackFunc` signature). By default given function executed as they are added sequence unless `priority` is specified.

aah Server events are executed in a synchronous way.

**Note:** For `OnRequest`, `OnPreReply`, `OnAfterReply` you can only register/add one event callback function.

Reference to [Event Emitter/Publisher](event-publisher.html).

### List of Extension Points

  * [OnInit](#event-oninit)
  * [OnStart](#event-onstart)
  * [OnShutdown](#event-onshutdown)
  * [OnRequest](#event-onrequest)
  * OnPreAuth **`upcoming`**
  * OnPostAuth **`upcoming`**
  * [OnPreReply](#event-onprereply)
  * [OnAfterReply](#event-onafterreply)

## Event: OnInit

`OnInit` event is published right after the `aah.AppConfig()` is initialized. Only `aah.conf` config is initialized at this stage, such as App Variables, Routes, i18n, Security, View Engine, Logs are not initialized.

**Supports Multiple:** Yes

```go
// As a anonymous func
func init() {
  aah.OnInit(func(e *Event) {
		// logic goes here
	})
}

// Or define a func and supply it
func loadUserConfig(e *aah.Event)  {
  // loading user config from /etc/myapp/myapp.conf
  // merge it to aah.AppConfig().Merge(...)
}

func init() {
  aah.OnInit(loadUserConfig)

  // OR
  aah.OnInit(loadUserConfig, 10) // with priority
}
```

## Event: OnStart

`OnStart` event is published right before the start of `aah Server`. At this stage entire application is initialized, just server is not yet started.

**Supports Multiple:** Yes

```go
func connectDatabase(e *aah.Event)  {
  // logic goes here
}

func connectRedis(e *aah.Event) {
  // logic goes here
}

func refreshCache(e *aah.Event) {
  // logic goes here
}

func init() {
  // By default given function executed as they are added sequence
  // unless `priority` is specified.
  aah.OnStart(connectDatabase)
  aah.OnStart(refreshCache, 3) // with priority
  aah.OnStart(connectRedis, 2) // with priority
}
```

## Event: OnShutdown

On receiving OS Signals `SIGINT` or `SIGTERM`, aah server does graceful shutdown based on config value `server.timeout.grace_shutdown` and then it publishes the `OnShutdown` event.

**Supports Multiple:** Yes

```go
func disconnectDatabase(e *aah.Event)  {
  // logic goes here
}

func disconnectRedis(e *aah.Event) {
  // logic goes here
}

func flushCache(e *aah.Event) {
  // logic goes here
}

func init() {
  // By default given function executed as they are added sequence
  // unless `priority` is specified.
  aah.OnShutdown(flushCache)
  aah.OnShutdown(disconnectDatabase)
  aah.OnShutdown(disconnectRedis)
}
```

## Event: OnRequest

`OnRequest` event is published for each incoming request to the aah server.

  * At the point request `Route` is not yet populated/evaluated

**Special Case:** The `aah.Context` object passed to the extension functions is decorated with the `ctx.SetURL()` and `ctx.SetMethod()` methods. Calls to these methods will impact how the request is routed.

**Supports Multiple:** No

```go
func init() {
  aah.OnRequest(func(e *aah.Event)  {
    ctx := e.Data.(*aah.Context)

    // logic goes here
  })
}
```

## Event: OnPreReply

`OnPreReply` event is published right before writing an reply/response on the wire. At this point response writer is clean nothing is written i.e. Rendering, Headers, Cookies, Status Code and Response Body.

**Note:** `OnPreReply` is not called when-

  * Static file request
  * `Reply().Done()` is called
  * `Reply().Redirect(...)` is called

**Supports Multiple:** No

```go
func init() {
  aah.OnPreReply(func(e *aah.Event)  {
    ctx := e.Data.(*aah.Context)

    // logic goes here
  })
}
```

## Event: OnAfterReply

`OnAfterReply` event is published right after the response is written on the wire. Nothing you can do about the response, however it has valuable information such as response bytes size, response status code, etc.

**Note:** `OnAfterReply` is not called when-

  * Static file request
  * `Reply().Done()` is called
  * `Reply().Redirect(...)` is called

  **Supports Multiple:** No

  ```go
  func init() {
    aah.OnAfterReply(func(e *aah.Event)  {
      ctx := e.Data.(*aah.Context)

      // logic goes here
    })
  }
  ```
