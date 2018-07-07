Title: WebSocket for Real-time Apps
Desc: aah provides a flexible and easier way to implement WebSocket capabilities into the application. aah draws a thin line to enable maximum usability and provides WebSocket power to the aah user.
Keywords: websocket, real-time app, real-time communication, aah, aah framework
---
# WebSocket for Real-time Apps

aah provides a flexible and easier way to implement `WebSocket` capabilities into the application. aah enables maximum usability and provides WebSocket power to the user.

As always, aah is structured, organized and maintainable. aah shines more with application growth.

  * Enable WebSocket under `server` section in `aah.conf`.
  * Add WebSocket's under package `<app-base-dir>/app/websockets`.
  * Add WebSocket's routing definition into `routes.conf`; it is similar to controllers, easy right.

### Table of Contents

  * [Configuration](#configuration)
  * [Creating WebSocket](#creating-websocket)
  * [Custom ID](#custom-id)
  * [Adding WebSocket Route](#adding-route)
  * [Sending Header on Connection Upgrade](#sending-header-on-connection-upgrade)
  * [WebSocket Events](#events)
  * [Request Lifecycle](/request-life-cycle.html#websocket-request)

## Configuration

As you know, one of the notable feature of aah is configuration. This configuration goes under section `server { ... }`.

```bash
# --------------------------------------------------------------------------
# WebSocket Configurations
# Doc: https://docs.aahframework.org/websocket.html#configuration
# --------------------------------------------------------------------------
websocket {
  # Default value is `false`.
  enable = true

  # Origin Check Configuration
  # It is useful when having only browser based clients. It adds another layer
  # of security protection.
  origin {
    # When origin check is enabled, aah does check against HTTP Header
    # `Origin` value.
    # Default value is `false`.
    check = false

    # Origin whitelist
    # No default values.
    #whitelist = [
    #  "https://websocket.org",
    #]
  }
}
```

## Creating WebSocket

It is simple, similar to aah's Controller.  [![GoDoc](https://godoc.org/aahframework.org/ws.v0?status.svg)](https://godoc.org/aahframework.org/ws.v0)

```go
// HotNewsWebSocket holds an implementation of News Broadcast.
type HotNewsWebSocket struct {
  *ws.Context
}

// ByTopic method handles topic related clients.
func (h *HotNewsWebSocket) ByTopic(topic string)  {
  for {
    // Implementation goes here
    // Refer to simple chat example to get an idea
    //
  }
}

// and so on ...
```

## Custom ID

aah support custom ID generation for each WebSocket connection. By default aah creates GUID using MongoDb Object ID alogrithm.

```go
// IDGenerator func type used to implement custom WebSocket connection ID.
// By default aah generates GUID using MongoDB Object ID algorithm.
type IDGenerator func(ctx *Context) string

// Setting ID generator via
aah.AppWSEngine().SetIDGenerator(MyCustomIDGenerator)
```

## Adding Route

Similar to aah's Controller routes definition, it is easy. The flexibility of routes definition is also supported in WebSocket.

<div class="alert alert-info-blue">
<p><strong>Note:</strong> aah understands WebSocket route definition mainly by the following two attributes.
<ul>
  <li><code>method = "WS"</code></li>
  <li><code>websocket = "HotNewsWebSocket"</code></li>
</ul>
</p>
</div>

```bash
by_topic {
  path = "/hot-news/:topic"
  method = "WS"
  websocket = "HotNewsWebSocket"
  action = "ByTopic"

  # By default, 'anti_csrf_check', 'cors' and 'max_body_size' configs are not
  # applicable for WebSocket; even if provided, it won't affect the WebSocket route.
  #
  # Use `server.websocket.origin { ... }` for enforcing SAME-ORIGIN policy.
}
```

## Sending Header on Connection Upgrade

aah sends `ctx.Header` values during a successful WebSocket connection upgrade.

**Note:** Incoming request headers are accessible via `ctx.Req.Header`.

```go
// Adding header into `ws.Context`
// Use event `ws.OnPreConnect` to add header into context.
func AddHeaderIntoContext(eventName string, ctx *ws.Context)  {
  ctx.Header.Set("My-Header", "Header value")
}
```

## Events

aah provides following events as part of the [WebSocket lifecycle](/request-life-cycle.html#websocket-request). It is published for each WebSocket connection made to the aah server.

As a recommended approach, create an event handler in appropriate package and register it in the file `app/init.go`.

Event | Description
----- | -----------
OnPreConnect | Published just before connection gets upgraded to WebSocket. It provides a control of accepting incoming request or reject it using `ctx.Abort(errorCode)`
OnPostConnect | Published just after the successful WebSocket connection is established with the aah server.
OnPostDisconnect | Published just after the WebSocket client got disconnected. It could have occurred due to graceful disconnect, network related error, etc.
OnError | Published whenever error occurs in the lifecycle such as Origin Check failed, WebSocket or WebSocket Action not found, WebSocket Action parameter parse error, and WebSocket upgrade fails. <br><br>Call method `ctx.ErrorReason()` to know the reason for the error.

### Event Callback Signature

```go
// EventCallbackFunc func type used for all WebSocket event callbacks.
func(eventName string, ctx *ws.Context)
```

### Code Snippet from app/init.go

```go
// WebSocket Events
// Doc: https://docs.aahframework.org/websocket.html#events
//
// Subscribing WebSocket events on app start.
func SubscribeWebSocketEvents(_ *aah.Event) {
  wse := aah.AppWSEngine()

  // Custom ID Generator
  wse.SetIDGenerator(websockets.MyCustomIDGenerator)

  // Event: OnPreConnect
  //
  wse.OnPreConnect(websockets.HandleEvents)

  // Event: OnPostConnect
  //
  wse.OnPostConnect(websockets.HandleEvents)

  // Event: OnPostDisconnect
  //
  wse.OnPostDisconnect(websockets.HandleEvents)

  // Event: OnError
  //
  wse.OnError(websockets.HandleEvents)
}

// SubscribeWebSocketEvents method gets registered on App Start event to
// subscribe WebSocket events from aah WebSocket Engine.
//
aah.OnStart(SubscribeWebSocketEvents)
```
