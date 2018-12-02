Title: aah Request and Response
Desc: Insights into aah Request and Response capabilities.
Keywords: request, response
---
# aah Request and Response

This document provides an insights into aah `Request` and `Response` capabilities. So that it could be utilized effectively.

### Table of Contents

  * [Request](#request)
  * [Response](#response)

## Request

aah [Request](https://godoc.org/aahframe.work/ahttp#Request) extends form of standard request struct. It provides necessary values (derived and processed per RFCs).

### Fields

Field Name | Description
----------- | -----------
Scheme | Request protocol value `https` or `http`, it's a inferred value. [Know more](https://godoc.org/aahframe.work/ahttp#Scheme)
Host | Value of correct Host source from HTTP request
Proto | Value of current HTTP request protocol. (e.g. HTTP/1.1, HTTP/2.0)
Method | Value of HTTP verb name, such as GET, POST, etc
Path | Request URL Path e.g. `/app/login.html`
Header | HTTP request headers
URLParams | URL path parameters values that was defined routes.conf
IsGzipAccepted | Inferred value as bool, whether HTTP client supports Gzip compression or not.

### Methods

Method Name | Description
----------- | -----------
AcceptContentType | Negotiated value of HTTP Header `Accept`. The resolve order is URL extension, Accept header per `RFC7231` and Vendor Types per `RFC4288`
AcceptEncoding | Negotiated value of HTTP Header the `Accept-Encoding` per `RFC7231`
ContentType | Parsed value of HTTP header `Content-Type` per RFC1521
Body | Returns the HTTP request body `io.ReadCloser`
SaveFile | Saves an uploaded request multipart file for the given key into given destination file
Locale | Negotiated value of HTTP Header `Accept-Language` per `RFC7231`
ClientIP | Returns remote Client IP address aka Remote IP. It parses in the order of given set of headers otherwise it uses default header set `X-Forwarded-For`, `X-Real-IP`, `X-Appengine-Remote-Addr` and finally `http.Request.RemoteAddr`
Cookie | Returns a named cookie from HTTP request otherwise error
Cookies | Returns all the cookies from HTTP request
IsJSONP | Returns true if request URL query string has `callback=<function_name>`
IsAJAX | Returns true if request header `X-Requested-With` is `XMLHttpRequest` otherwise false
URL | Returns request URL instance
Referer | Returns value of HTTP 'Referrer' (or 'Referer') header.
UserAgent | Returns value of HTTP 'User-Agent' header.
Unwrap | Returns the underlying *http.Request instance of Go HTTP server, direct interaction with raw object is not encouraged.

<div class="alert alert-info-green">
<p>It is highly recommended to use Auto Parse and Bind feature on controller action arguments. Non-controller (like middleware, etc.) location use following method appropriately.</p>
</div>

<br>

Method Name | Description
----------- | -----------
PathValue | Returns value for given Path param key otherwise empty string. For eg.: `/users/:userId` => `PathValue("userId")`
QueryValue | Returns value for given URL query param key otherwise empty string
QueryArrayValue | Returns array value for given URL query param key otherwise empty string slice
FormValue | Returns value for given form key otherwise empty string
FormArrayValue | Returns array value for given form key otherwise empty string slice
FormFile | Returns the first file for the provided form key otherwise returns error. It is caller responsibility to close the file

## Response

aah resposne writer implements interfaces `http.ResponseWriter`, `http.CloseNotifier`, `http.Flusher`, `http.Hijacker`, `http.Pusher` and `io.Closer`.

And captuers following details -

  * `Status()` - returns response status code
  * `BytesWritten()` - returns the total number of bytes written into response. Does not include response header bytes size.
  * `Unwrap()` - returns the underlying `ResponseWriter`

<div class="alert alert-info-green">
<p>It is highly recommended to use <a>Reply Builder</a> to compose response.</p>
</div>
