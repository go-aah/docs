Title: aah Server Extension Point
Desc: aah server exposes the App and Request life cycle stages as server events, i.e. called Server Extension Point. Function signature is same as events (aah.EventCallbackFunc signature).
Keywords: server extension point, aah server extensions, extension point, server events, callback, events
---
# aah Server Extension Point

aah go server exposes the App and Request life cycle stages as server events. It is called as Server Extension Point. Function signature is same as events (`aah.EventCallbackFunc`). By default given function executed as they are added sequence unless `priority` is specified.

aah Server events are executed synchronously.

<div class="alert alert-info-blue">
<p><strong>Note:</strong></p>
<p>For <code>OnRequest</code>, <code>OnPreAuth</code>, <code>OnPostAuth</code>, <code>OnPreReply</code>, <code>OnAfterReply</code> you can only register/add one event callback function.</p>
</div>

Reference to [Event Emitter/Publisher](event-publisher.html).

### List of Server Extension Points

  * [OnInit](#event-oninit)
  * [OnStart](#event-onstart)
  * [OnShutdown](#event-onshutdown)
  * [OnRequest](#event-onrequest)
  * [OnPreAuth](#event-onpreauth) <span class="badge lb-xs">Since v0.7</span>
  * [OnPostAuth](#event-onpostauth) <span class="badge lb-xs">Since v0.7</span>
  * [OnPreReply](#event-onprereply)
  * [OnAfterReply](#event-onafterreply)

## Event: OnInit

`OnInit` event is published right after the `aah.AppConfig()` is loaded. Only `aah.conf` config is initialized at this stage, such as App Variables, Routes, i18n, Security, View Engine, Logs are not yet initialized.

**Supports Multiple:** Yes

```go
// As a anonymous func
func init() {
  aah.OnInit(func(e *Event) {
		// logic goes here
	})
}

// Or define a func and supply it [recommended, name gets logged in log]
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

`OnStart` event is published right before the start of `aah go Server`. At this stage application is completely initialized, just server is not yet started.

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

`OnRequest` event is published for each incoming request to the aah go server.

  * At this point request `Route` is not yet populated/evaluated.

**Note:** The `aah.Context` object passed to the extension functions is decorated with the `ctx.SetURL()` and `ctx.SetMethod()` methods. Calls to these methods will have an impact how the request is routed.

**Supports Multiple:** No

```go
func init() {
  aah.OnRequest(func(e *aah.Event)  {
    ctx := e.Data.(*aah.Context)

    // logic goes here
  })
}
```

## Event: OnPreAuth

<span class="badge lb-sm">Since v0.7</span> `OnPreAuth` event is published right before the Authentication by security manager.

**Supports Multiple:** No

```go
func init() {
  aah.OnPreAuth(func(e *aah.Event)  {
    ctx := e.Data.(*aah.Context)

    // logic goes here
  })
}
```

## Event: OnPostAuth

<span class="badge lb-sm">Since v0.7</span> `OnPostAuth` event is published right after the Authentication and Authorization info gets populated into Subject by security manager.

**Supports Multiple:** No

```go
func init() {
  aah.OnPostAuth(func(e *aah.Event)  {
    ctx := e.Data.(*aah.Context)

    // logic goes here
  })
}
```

## Event: OnPreReply

`OnPreReply` event is published right before writing an reply/response on the wire. At this point response writer is clean nothing is written i.e. Headers, Cookies, Redirects, Status Code and Response Body not written.

**Note:** `OnPreReply` is not called when-

  * `Reply().Done()` was called

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

`OnAfterReply` event is published right after the response is written on the wire. Nothing we can do about the response, however context has a valuable information such as response bytes size, response status code, etc.

**Note:** `OnAfterReply` is not called when-

  * `Reply().Done()` was called
  * `Reply().Redirect(...)` was called

**Supports Multiple:** No

```go
func init() {
  aah.OnAfterReply(func(e *aah.Event)  {
    ctx := e.Data.(*aah.Context)

    // logic goes here
  })
}
```
