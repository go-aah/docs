Title: Reply Builder (aka Response Builder)
Desc: aah provides powerful Reply builder to compose your HTTP response effectively. You can do chained call.
Keywords: reply builder, reply, response, html, json, jsonp, text, bytes, file, status code, cookie, redirect, custom
---
# Reply Builder

aah provides `Reply` builder (aka Response Builder) to compose your response effectively. You can do Chained call.

### Table of Contents

  * [Response Status Codes](#response-status-codes)
  * [Response Content](#response-content)
  * [Redirect](#redirect)
  * [Replying HTTP Headers](#replying-http-headers)
  * [Cookie](#cookies)
  * [Disable Gzip](#disable-gzip)
  * [Done()](#done)
  * [Implementing Custom Rendering](#implementing-custom-rendering)
  * [Just Few Samples](#just-few-samples)
  * [Registering External JSON Library into aah](external-json-library.html) <span class="badge lb-xs">Since v0.8</span>

## Response Status Codes
As per RFC7231, `Reply()` provides method for frequently used ones.

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
  * `Error(err)` <span class="badge lb-xs">Since v0.8</span> [know more](centralized-error-handler.html#reply-error-err).
  * `Render(rdr)` - renders custom rendering implementation [know more](#)

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

<div class="alert alert-info-blue">
<p><strong>Note:</strong> Framework doesn't intervene with response if `Done()` method was called.</p>
</div>

## Implementing Custom Rendering
aah provides reply method called `Render` to supply your own implementation of rendering. You can do by implementing interface `aah.Render` or using adaptor `aah.RenderFunc`.

**Example of interface aah.Render**
```go
// CustomRender implements the interface `aah.Render`.
type CustomRender struct {
	// ... your fields goes here
}

func (cr *CustomRender) Render(w io.Writer) error {
  // implementation goes here
  // return error for any issues
  return nil
}

// Then you call render method in your controller action
Reply().Render(&CustomRender{
  // fields goes here
})
```

**Example of adaptor aah.RenderFunc**
```go
Reply().Render(aah.RenderFunc(func(w io.Writer) error {
  // implementation goes here
  // return error for any issues
  return nil
}))
```

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

// Replying Error - processed by Centralized Error Handler before writing a reply
Reply().Error({
  Code: http.StatusNotFound,
  Message: "Resource not found",
  Data: "relevant data if you would like to send", // this is interface{} type.
})
```
