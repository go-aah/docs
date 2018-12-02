Title: Event Publisher/Emitter
Desc: Event Publisher provides an idiomatic asynchronous event-driven architecture that able to Publish asynchronous and synchronous events. Subscribe and Unsubscribe to those events. aah Server events built around this Event Publisher.
Keywords: events, event publisher, event emitter, subscribe event, unsubscribe event, event callback
---
# Event Publisher/Emitter

Event Publisher provides an idiomatic asynchronous event-driven architecture that able to Publish asynchronous and synchronous events. Subscribe and Unsubscribe to those events.

aah Server events built around this Event Publisher.

On each named event you can attach one or more callbacks to it. The callback have to be compliant with `aah.EventCallback` or callback func is `aah.EventCallbackFunc`.

### Table of Contents

  * [Event Structure](#event-structure)
  * [Subscribe to the Event](#subscribe-to-the-event)
  * [Unsubscribe from the Event](#unsubscribe-from-the-event)
  * [Publish Event Asynchronously](#publish-event-asynchronously)
  * [Publish Event Synchronously](#publish-event-synchronously)


## Event Structure

```go
// Event type holds the details of single event.
Event struct {
	Name string
	Data interface{}
}
```

## Subscribe to the Event
```go
// compliant with aah.EventCallbackFunc
func productUpdated(e *aah.Event)  {
  // logic goes here
}

subscribe := &EventCallback{
  Callback: productUpdated,
  CallOnce: false, // always called when event is published
}

aah.App().SubscribeEvent("ProductUpdated", subscribe)

// OR

// subscribe using callback func
aah.App().SubscribeEventFunc("ProductUpdated", productUpdated)
```

## Unsubscribe from the Event
```go
// compliant with aah.EventCallbackFunc
func productUpdated(e *aah.Event)  {
  // logic goes here
}

subscribe := EventCallback{Callback: productUpdated}

// you have subscribe it before

aah.App().UnsubscribeEvent("ProductUpdated", subscribe)

// OR

// unsubscribe by callback func
aah.App().UnsubscribeEventFunc("ProductUpdated", productUpdated)
```

## Publish Event Asynchronously
```go
aah.App().PublishEvent("UserEdited", data)
```

## Publish Event Synchronously
```go
aah.App().PublishEventSync("UserEdited", data)
```
