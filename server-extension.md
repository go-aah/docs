Title: aah Server Extension Point
Desc: aah server exposes the App and Request life cycle stages as server events, i.e. called Server Extension Point. Function signature is same as events (aah.EventCallbackFunc signature).
Keywords: server extension point, aah server extensions, extension point, server events, callback, events, hooks
---
# aah Server Extension Point

aah server exposes the App and Request life cycle stages as server events. It is called as Server Extension Point. Function signature is the same as events (`aah.EventCallbackFunc`).

aah server events are executed synchronously. Reference to [Event Emitter/Publisher](event-publisher.html).

### Table of Contents

  * [Application Extension Points](#application-extension-points)
      - [OnInit](#event-oninit)
      - [OnStart](#event-onstart)
      - [OnPreShutdown](#event-onpreshutdown)
      - [OnPostShutdown](#event-onpostshutdown)
  * [HTTP Engine Extension Points](#http-engine-extension-points)
      - [OnRequest](#event-onrequest)
      - [OnPreAuth](#event-onpreauth)
      - [OnPostAuth](#event-onpostauth)
      - [OnPreReply](#event-onprereply)
      - [OnHeaderReply](#event-onheaderreply)
      - [OnPostReply](#event-onpostreply)
  * [WebSocket Engine Extension Points](/websocket.html#events)

## Application Extension Points

Application extension points by default, a given function is executed as an added sequence unless `priority` is specified.

## Event: OnInit

Event `OnInit` is published once the `aah.AppConfig()` is loaded. At this stage, only `aah.conf` config is initialized. App Variables, Routes, i18n, Security, View Engine, Logs and so on will be initialized after this event.

**Supports Multiple:** Yes

```go
// As an anonymous func
func init() {
  aah.OnInit(func(e *Event) {
		// logic comes here
	})
}

// Or define a func and supply it [recommended approach, name gets logged in log]
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

Event `OnStart` is published just before the start of `aah Server`. The application is completely initialized at this stage. The server is yet to be started.

**Supports Multiple:** Yes

```go
func connectDatabase(e *aah.Event)  {
  // logic comes here
}

func connectRedis(e *aah.Event) {
  // logic comes here
}

func refreshCache(e *aah.Event) {
  // logic comes here
}

func init() {
  // By default, a given function is executed as an added sequence
  // unless `priority` is specified.
  aah.OnStart(connectDatabase)
  aah.OnStart(refreshCache, 3) // with priority
  aah.OnStart(connectRedis, 2) // with priority
}
```

## Event: OnPreShutdown

Event `OnPreShutdown` is published when application receives OS Signals `SIGINT` or `SIGTERM` and before the triggering graceful shutdown. After this event, aah triggers graceful shutdown with config value of `server.timeout.grace_shutdown`.

**Supports Multiple:** Yes

```go
func announceImGonnaShutdown()  {
  // announce it
}

func init()  {
  // By default, a given function is executed as an added sequence
  // unless `priority` is specified.
  aah.OnPreShutdown(announceImGonnaShutdown)
}

```

## Event: OnPostShutdown

Event `OnPostShutdown` is published just after the successful grace shutdown of aah server and then application does clean exit.

**Supports Multiple:** Yes

```go
func disconnectDatabase(e *aah.Event)  {
  // logic comes here
}

func disconnectRedis(e *aah.Event) {
  // logic comes here
}

func flushCache(e *aah.Event) {
  // logic comes here
}

func init() {
  // By default, a given function is executed as an added sequence
  // unless `priority` is specified.
  aah.OnPostShutdown(flushCache)
  aah.OnPostShutdown(disconnectDatabase)
  aah.OnPostShutdown(disconnectRedis)
}
```

## HTTP Engine Extension Points

### Event: OnRequest

Event `OnRequest` is published on each incoming request to the aah server.

<div class="alert alert-info-blue">
<p><strong>Note:</strong>
<ul>
  <li>At this point, the request <code>Route</code> is yet to be populated/evaluated.</li>
  <li>The <code>aah.Context</code> object passed to the extension functions is decorated with  <code>ctx.SetURL()</code> and <code>ctx.SetMethod()</code> methods. Calls to these methods will have an impact on how the request is routed.</li>
</ul>
</p>
</div>

```go
func init() {
  aah.AppHTTPEngine().OnRequest(func(e *aah.Event)  {
    ctx := e.Data.(*aah.Context)

    // logic comes here
  })
}
```

### Event: OnPreAuth

<span class="badge lb-sm">Since v0.7</span> Event `OnPreAuth` is published just before the Authentication and Authorization.

```go
func init() {
  aah.AppHTTPEngine().OnPreAuth(func(e *aah.Event)  {
    ctx := e.Data.(*aah.Context)

    // logic comes here
  })
}
```

### Event: OnPostAuth

<span class="badge lb-sm">Since v0.7</span> Event `OnPostAuth` is published once the Authentication and Authorization info gets populated into Subject.

```go
func init() {
  aah.AppHTTPEngine().OnPostAuth(func(e *aah.Event)  {
    ctx := e.Data.(*aah.Context)

    // logic comes here
  })
}
```

### Event: OnPreReply

Event `OnPreReply` is published just before writing a reply/response on the wire. At this point, the response writer is clean. i.e. Headers, Cookies, Redirects, Status Code and Response Body are not written.

<div class="alert alert-info-blue">
<p><strong>Note:</strong> <code>OnPreReply</code> will not get published when; <code>Reply().Done()</code> was called</p>
</div>

```go
func init() {
  aah.AppHTTPEngine().OnPreReply(func(e *aah.Event)  {
    ctx := e.Data.(*aah.Context)

    // logic comes here
  })
}
```

### Event: OnHeaderReply

<span class="badge lb-sm">Since v0.11.0</span> Event `OnHeaderReply` is published before writing HTTP header `Status`. At this point, all the headers except the header `Status` get written on the `http.ResponseWriter`.

<div class="alert alert-info-blue">
<p><strong>Note:</strong> <code>OnHeaderReply</code> is not called when; <code>Reply().Done()</code> and <code>Reply().Redirect(...)</code> was called</p>
</div>

```go
func init() {
  aah.AppHTTPEngine().OnHeaderReply(func(e *aah.Event)  {
    hdr := e.Data.(http.Header)

    // Header instance is the direct reference to http.ResponseWritter
  	// Any changes reflect immediately :)
  	//
  	// logic comes here
  })
}
```

### Event: OnPostReply

Event `OnPostReply` is published right after the response gets written on the wire. We can do nothing about the response, however the context has valuable information such as response bytes size, response status code, etc.

<div class="alert alert-info-blue">
<p><strong>Note:</strong> <code>OnPostReply</code> will not be published when; <code>Reply().Done()</code> and <code>Reply().Redirect(...)</code> was called</p>
</div>

```go
func init() {
  aah.AppHTTPEngine().OnPostReply(func(e *aah.Event)  {
    ctx := e.Data.(*aah.Context)

    // logic comes here
  })
}
```
