Title: Reply Builder (aka Response Builder)
Desc: Framework provides `Reply` builder to compose your response effectively. You can do Chained call.
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
Replying various content types

  * `HTML(data)`
  * `HTMLl(layout, data)`
  * `HTMLlf(layout, filename, data)`
  * `JSON(data)`
  * `JSONP(data, callback)`
  * `XML(data)`
  * `Text(str)`
  * `Text(str, args)`
  * `Bytes(contentType, bytes)`
  * `File(filename, readCloser)` - as an attachment
  * `FileInline(filename, readCloser)` - as an inline

## Redirect
  * `Redirect(url)`
  * `Redirects(url, statusCode)`

## Replying HTTP Headers
  * `Header(hdr, value)`
  * `HeaderAppend(hdr, value)`

## Cookies
  * `Cookie(cookie)`

## Done()
Done method indicates to framework that reply has already been sent via `aah.Context.Res` and that no further action is needed.

**Note:** Framework doesn't intervene with response if `Done()` method is called.

## Samples
```go
// Replying JSON with status 200
c.Reply().
  Ok().
  JSON(data)

// Replying file as attachment
c.Reply().File("report.pdf", readCloser) // default status code is 200 OK

// Replying redirect login page using `route name`
c.Reply().Redirect(c.ReverseURL("user_login"))

// Replying HTML response with cookie and view arguments
c.Reply().
  Cookie(&http.Cookie{
    //....
  }).
  HTML(data)

// Replying HTML response with custom layout and data
c.Reply().HTMLl("master-docs", data)
```
