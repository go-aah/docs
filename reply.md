Title: Reply Builder (aka Response Builder)
Desc: Framework provides Reply builder to compose your response effectively. You can do Chained call.
Keywords: reply builder, reply, response, html, json, jsonp, text, bytes, file, status code, cookie, redirect
---
# Reply Builder (aka Response Builder)

Framework provides `Reply` builder to compose your response effectively. You can do Chained call.

### Table of Contents

  * [Response Status Codes](#response-status-codes)
  * [Response Content](#response-content)
  * [Redirect](#redirect)
  * [Replying HTTP Headers](#replying-http-headers)
  * [Cookie](#cookies)
  * [Disable Gzip](#disable-gzip)
  * [Done()](#done)
  * [Sample](#samples)

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

ohh, I do not find status method, just use `Reply().Status(http.StatusPartialContent)` or `Reply().Status(206)`

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
`DisableGzip` method provides an option to disable `gzip` compression for a particular response.

```go
Reply().DisableGzip()
```

## Done()
Done method indicates to framework that reply has already been sent via `aah.Context.Res` and that no further action is needed.

**Note:** Framework doesn't intervene with response if `Done()` method is called.

## Samples
```go
// Replying JSON with status 200
// Default status code is 200
c.Reply().Ok().JSON(data)
// OR
c.Reply().JSON(data)

// Replying file as an attachment
c.Reply().FileDownload("/User/jeeva/songs/nice.mp3", "name-nice.mp3") // default status code is 200 OK

// Replying redirect login page using `route name`
c.Reply().Redirect(c.ReverseURL("user_login"))

// Replying HTML response with cookie and view arguments
c.Reply().
  Cookie(&http.Cookie{
    //....
  }).
  HTML(data)

// Replying HTML response with custom layout and data
c.Reply().HTMLl("master-docs.html", data)
```
