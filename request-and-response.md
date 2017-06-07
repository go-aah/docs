Title: aah Request and Response
Desc: Learning and insight into aah Request and Response
Keywords: request, response
---
# aah Request and Response

This page gives you an insights about aah `Request` and `Response` capabilities. So that you can use it effectively.

### Request

aah Request object improved from standard request object. For typical application usage for e.g.: Parsing HTTP headers per RFC's (Content-type, Accept content-type, Locale, Gzip), identifying Request Scheme, etc.

#### Fields

  * <u>Scheme</u> - protocol value `https` or `http`; it's a derived value from `X-Forwarded-Proto` if present used as-is, `http` if TLS is nil and `https` if TLS is not nil.
  * <u>Host, Method, Path, Header</u> - it is typically same as standard one, provided here for conventional access.
  * <u>ContentType</u> - the parsed value of HTTP header `Content-Type` per `RFC1521`.
  * <u>AcceptContentType</u> - it is negotiated value from HTTP Header `Accept`. In the order of URL extension, Accept header per `RFC7231` and Vendor Types per `RFC4288`
  * <u>AcceptEncoding</u> - it is negotiated value from HTTP Header the `Accept-Encoding` per `RFC7231`.
  * <u>Locale</u> - it is negotiated value from HTTP Header `Accept-Language` per `RFC7231`.
  * <u>Params</u> - it contains values from Path, Form, Query and File.
  * <u>Payload</u> - it holds the value from HTTP request body in bytes slice for `Content-Type` JSON and XML otherwise nil.
  * <u>ClientIP</u> - remote client IP address aka Remote IP. Parsed in the order of `X-Forwarded-For`, `X-Real-IP` and finally `http.Request.RemoteAddr`.
  * <u>IsGzipAccepted</u> - whether client supported Gzip compression or not.
  * <u>Referer</u> - value of HTTP header `Referer` or `Referrer`
  * <u>UserAgent</u> - value of HTTP header `User-Agent`
  * <u>Raw</u> - standard Go HTTP request object.

#### Methods

  * <u>PathValue</u> - returns value for given Path param key otherwise empty string. For eg.: `/users/:userId` => `PathValue("userId")`.
  * <u>QueryValue</u> - returns value for given URL query param key otherwise empty string.
  * <u>QueryArrayValue</u> - returns array value for given URL query param key otherwise empty string slice.
  * <u>FormValue</u> - returns value for given form key otherwise empty string.
  * <u>FormArrayValue</u> - returns array value for given form key otherwise empty string slice.
  * <u>FormFile</u> - returns the first file for the provided form key otherwise returns error. It is caller responsibility to close the file.
  * <u>Cookie</u> - returns a named cookie from HTTP request otherwise error.
  * <u>Cookies</u> - returns all the cookies from HTTP request.
  * <u>IsJSONP</u> - returns true if request URL query string has `callback=function_name`.
  * <u>IsAJAX</u> - returns true if the request header `X-Requested-With` is `XMLHttpRequest` otherwise false.

### Response

aah Response Writer is improved from standard response writer and compliant with `http.ResponseWriter`. So you can simply pass on `ctx.Res` anywhere it's needed.

aah `ahttp.ResponseWriter` captures following details:

  * `Status()` - returns response status code.
  * `BytesWritten()` - returns the total number of bytes written into response.
  * `Unwrap()` - returns the standard `ResponseWriter` if needed.

And Implements following standard interfaces from `http` library.

  * `http.CloseNotifier`
  * `http.Flusher`
  * `http.Hijacker`
  * `http.Pusher`
  * `io.Closer`


**Happy coding :)**
