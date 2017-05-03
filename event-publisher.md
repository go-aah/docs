Title: Event Publisher/Emitter
Desc: Event Publisher provides an idiomatic asynchronous event-driven architecture that able to Publish asynchronous and synchronous events. Subscribe and Unsubscribe to those events. aah Server events built around this Event Publisher.
Keywords: events, event publisher, event emitter, subscribe event, unsubscribe event, event callback
---
# Event Publisher/Emitter

Event Publisher provides an idiomatic asynchronous event-driven architecture that able to Publish asynchronous and synchronous events. Subscribe and Unsubscribe to those events.

aah Server events built around this Event Publisher.

On each each named event you can attach one or more callbacks to it. The call is have to be compliant with `aah.EventCallback` and callback is `aah.EventCallbackFunc`.

### Table of Contents

  * [Event Structure](#event-structure)
  * [Subscribe to Event](#subscribe-to-event)
  * [Unsubscribe from Event](#unsubscribe-from-event)
  * [Publish Event Asynchronously](#publish-event-asynchronously)
  * [Publish Event Synchronously](#publish-event-synchronously)


## Event Structure

```go
// Event type holds the details of event generated.
Event struct {
	Name string
	Data interface{}
}
```

## Subscribe to Event
```go
// compliant with aah.EventCallbackFunc
func productUpdated(e *aah.Event)  {
  // logic goes here
}

subscribe := &EventCallback{
  Callback: productUpdated,
  CallOnce: false, // always called when event is published
}

aah.SubscribeEvent("ProductUpdated", subscribe)

// OR

// subscribe using callback func
aah.SubscribeEventf("ProductUpdated", productUpdated)
```

## Unsubscribe from Event
```go
// compliant with aah.EventCallbackFunc
func productUpdated(e *aah.Event)  {
  // logic goes here
}

subscribe := EventCallback{Callback: productUpdated}

// you have subscribe it before

aah.UnsubscribeEvent("ProductUpdated", subscribe)

// OR

// unsubscribe by callback func
aah.UnsubscribeEvent("ProductUpdated", productUpdated)
```

## Publish Event Asynchronously
```go
aah.PublishEvent("UserEdited", data)
```

## Publish Event Synchronously
```go
aah.PublishEventSync("UserEdited", data)
```
