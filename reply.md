Title: Reply Builder (aka Response Builder)
Desc: Framework provides Reply builder to compose your response effectively. You can do Chained call.
Keywords: reply builder, reply, response, html, json, jsonp, text, bytes, file, status code, cookie, redirect
---
# Reply Builder

Framework provides `Reply` builder (aka Response Builder) to compose your response effectively. You can do Chained call.

### Table of Contents

  * [Response Status Codes](#response-status-codes)
  * [Response Content](#response-content)
  * [Redirect](#redirect)
  * [Replying HTTP Headers](#replying-http-headers)
  * [Cookie](#cookies)
  * [Disable Gzip](#disable-gzip)
  * [Done()](#done)
  * [Just Few Samples](#just-few-samples)
  * [Registering External JSON Library into aah](#registering-external-json-library-into-aah) <span class="badge lb-xs">since v0.8</span>

## Response Status Codes
As per RFC7231, provides method for frequently used ones.

  * `Ok()`
  * `Created()`
  * `Accepted()`
  * `NoContent()`
  * `MovedPermanently()`
  * `Found()`
  * `TemporaryRedirect()`
  * `BadRequest()`
  * `Unauthorized()`
  * `Forbidden()`
  * `NotFound()`
  * `MethodNotAllowed()`
  * `Conflict()`
  * `InternalServerError()`
  * `ServiceUnavailable()`

ohh, the response status I need is not in the above list, no problem; just use `Reply().Status(http.StatusPartialContent)` or `Reply().Status(206)`

## Response Content
Rich reply methods for the response.

  * `HTML(data)`
  * `HTMLl(layout, data)`
  * `HTMLf(filename, data)`
  * `HTMLlf(layout, filename, data)`
  * `JSON(data)`
  * `JSONP(data, callback)`
  * `XML(data)`
  * `Text(str)`
  * `Text(str, args)`
  * `Readfrom(reader)`
  * `File(file)`
  * `FileDownload(file, targetName)` - Content-Disposition is attachment
  * `FileInline(file, targetName)` - Content-Disposition is inline
  * `Binary(bytes)`

## Redirect
  * `Redirect(url)`
  * `RedirectSts(url, statusCode)`

## Replying HTTP Headers
  * `Header(hdr, value)`
  * `HeaderAppend(hdr, value)`

## Cookies
  * `Cookie(cookie)`

## Disable Gzip

By default framework doesn't apply Gzip compression for Status Codes 204, 304 and if client doesn't support gzip. On top of it framework provides `DisableGzip` method as an option to disable `gzip` compression for a particular response.

```go
Reply().DisableGzip()
```

## Done()
Done method indicates that reply has already been sent via `aah.Context.Res` and that no further action is needed.

**Note:** Framework doesn't intervene with response if `Done()` method was called.

## Just Few Samples

Refer to [Response Status Codes](#response-status-codes) and [Response Content](#response-content) to know more.

```go
// Replying JSON with Status Code 200 OK
Reply().Ok().JSON(data)

// Default Status Code is 200 OK, so you can do
Reply().JSON(data)

// Replying JSON with Status Code 400 BAD REQUEST
Reply().BadRequest().JSON(data)

// Replying file as an attachment from absolute path
// Default Status Code is 200 OK
Reply().FileDownload("/User/jeeva/songs/nice.mp3", "name-nice.mp3")

// I just want serve file from controller
// If file param is relative path, framework resolves it from `static` directory
// like <app-base-dir>/static/reports/daily.pdf
Reply().File("reports/daily.pdf")

// I would like to send binary data
Reply().Binary(bytes)

// I would like to use Reader
Reply().Readfrom(reader)

// Replying redirect login page using `route name`
Reply().Redirect(c.ReverseURL("user_login"))

// Replying HTML response with cookie and view arguments
Reply().
  Cookie(&http.Cookie{
    //....
  }).
  HTML(data)

// Replying HTML response with custom layout and data
Reply().HTMLl("master-custom.html", data)
```

## Registering External JSON Library into aah

<span class="badge lb-sm">Since v0.8</span> You can register standard JSON compatible library into `aah`. By default framework uses standard one. Such as [json-iterator](https://github.com/json-iterator/go), [ffjson](https://github.com/pquerna/ffjson), etc.

```go
// Example of registering `json-iterator` library into aah
func init() {
	jjson := jsoniter.ConfigCompatibleWithStandardLibrary
	aah.JSONMarshal = jjson.Marshal
	aah.JSONUnmarshal = jjson.Unmarshal
	aah.JSONMarshalIndent = jjson.MarshalIndent
}
```
