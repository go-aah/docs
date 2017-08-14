Title: External JSON Library
Desc: aah provides an option to register external JSON library of your choice.
Keywords: external json library, third party json library, json library
---
# Registering External JSON Library

<span class="badge lb-sm">Since v0.8</span> aah provides an option to register external/third party JSON library. By default aah framework uses Go built-in library.

This [idea](https://github.com/go-resty/resty/issues/76) is from my one of the Go library [resty](https://github.com/go-resty/resty). You can register standard JSON compatible library into `aah`; such as [json-iterator](https://github.com/json-iterator/go), [ffjson](https://github.com/pquerna/ffjson), etc.

### Example of registering json-iterator library

```go
import "github.com/json-iterator/go"

func init()  {
  aah.JSONMarshal = jsoniter.Marshal
	aah.JSONUnmarshal = jsoniter.Unmarshal
	aah.JSONMarshalIndent = jsoniter.MarshalIndent
}
```

By default, `jsoniter` do not sort the map keys like standard library. If you want 100% compatibility, register as follows:
```go
import "github.com/json-iterator/go"

func init() {
	jjson := jsoniter.ConfigCompatibleWithStandardLibrary
	aah.JSONMarshal = jjson.Marshal
	aah.JSONUnmarshal = jjson.Unmarshal
	aah.JSONMarshalIndent = jjson.MarshalIndent
}
```

### Example of registering ffjson library

```go
import "github.com/pquerna/ffjson/ffjson"

func init() {
  aah.JSONMarshal = ffjson.Marshal
  aah.JSONUnmarshal = ffjson.Unmarshal
}
```
