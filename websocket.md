Title: WebSocket for Real-time Apps
Desc: aah provides a flexible and easier way to implement WebSocket capabilities into the application. aah draws a thin line to enable maximum usability and provides WebSocket power to the aah user.
Keywords: websocket, real-time app, real-time communication, aah, aah framework
---
# WebSocket for Real-time Apps

aah provides a flexible and easier way to implement `WebSocket` capabilities into the application. aah draws a thin line to enable maximum usability and provides WebSocket power to the aah user.

As always, aah is structured, organized and maintainable. aah shines more with application growth.

  * Enable WebSocket under `server` section in `aah.conf`.
  * Add WebSocket's under package `<app-base-dir>/app/websockets`.
  * Add WebSocket's routing definition into `routes.conf`; it is similar to controllers, easy right.

### Table of Contents

  * [Configuration](#configuration)
  * [Creating WebSocket](#creating-websocket)
  * [Adding WebSocket Route](#adding-route)
  * [WebSocket Events](#events)

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
  # applicable for WebSocket; even if provided by mistake,
  # it won't affect the WebSocket route.
  #
  # Use `server.websocket.origin { ... }` for enforcing SAME-ORIGIN policy.
}
```

## Events

aah provides following events as part of the WebSocket lifecycle. It is published for each WebSocket connection made to the aah server.

As a recommended approach, create an event handler under appropriate package and register it in the file `init.go`.

### Event Callback Signature

```go
// EventCallbackFunc func type used for all WebSocket event callbacks.
func(eventName string, ctx *ws.Context)
```

### Snippet from app/init.go

```go
// WebSocket Events
// Doc: https://docs.aahframework.org/websocket.html#events
//
// Subscribing WebSocket events on app start.
func SubscribeWebSocketEvents(_ *aah.Event) {
  wse := aah.AppWSEngine()

  // Event: OnPreConnect
  //
  // Published before connection gets upgraded to WebSocket.
  // It provides a control of accepting incoming request or reject it
  // using ctx.Abort(errorCode)
  wse.OnPreConnect(websockets.HandleEvents)

  // Event: OnPostConnect
  //
  // Published right after the successful WebSocket connection which is
  // established with the aah server.
  wse.OnPostConnect(websockets.HandleEvents)

  // Event: OnPostDisconnect
  //
  // Published right after the WebSocket client got disconnected.
  // It could have occurred due to graceful disconnect, network related error, etc.
  wse.OnPostDisconnect(websockets.HandleEvents)

  // Event: OnError
  //
  // Published whenever error occurs in the lifecycle such as Origin Check failed,
  // WebSocket/WebSocket Action not found, WebSocket Action parameter parse error,
  // and WebSocket upgrade fails.
  //
  //`ctx.ErrorReason()` method can be called to know the reason for the error.
  wse.OnError(websockets.HandleEvents)
}

// SubscribeWebSocketEvents method gets registered on App Start event to
// subscribe WebSocket events from aah WebSocket Engine.
//
aah.OnStart(SubscribeWebSocketEvents)
```
