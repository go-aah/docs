Title: aah Dump Request & Response
Desc: Server Dump - Request & Response in aah go server into file.
Keywords: dump, server dump, dump request, dump response, aah server
---
# Server Dump - Request & Response

aah provides easy to use dump/log HTTP request and response into log file, just enable it.  

Its not just a dump, Log is readable and very helpful for your `Debugging`. You can control dump log by headers, request body, response body, etc.

  * [Server Dump Log Configuration](#server-dump-log-configuration)
  * [Sample Dump of REST API tutorial app](#sample-dump-of-rest-api-tutorial-app)

<div class="alert alert-info-blue">
<p><strong>Note:</strong>
<ul>
  <li>It is not recommended to enable 'Request and Response dump' for <code>production use</code>. It may have application performance degrade.</li>
  <li>Keep an eye on <code>disk usage</code> when aah server dump is enabled.</li>
  <li>Server Dump log is not applicable for Static files delivery and non-readable content types.</li>
</ul>
</p>
</div>

## Server Dump Log Configuration

`dump_log` configuration goes under the config section `server { ... }` in the `aah.conf`.

```bash
# -------------------------------------------------------
# Dump Request & Response Details
# Such as URL, Proto, Headers, Body, etc.
# Note: Dump is not applicable for Static Files delivery.
# Doc: https://docs.aahframework.org/server-dump-log.html
# -------------------------------------------------------
dump_log {
  # Default value is `false`.
  enable = true

  # Absolute path to dump log file or relative path.
  # Default location is application logs directory
  #file = "{{ .AppName }}-dump.log"

  # Log Request body into dump log. aah dumps body for JSON, XML, Form
  # HTML and Plain Text content types.
  # Default value is `false`.
  #request_body = true

  # Log Request body into dump log. aah dumps body for JSON, XML, Form
  # HTML, and Plain Text content types.
  # Default value is `false`.
  #response_body = true
}
```

## Sample Dump of REST API tutorial app

This is just a sample dump log from REST API tutorial.

```cfg
=======================================================================

URI: http://localhost:8080/v1/posts
METHOD: POST
PROTO: HTTP/1.1
HEADERS:
    Accept: */*
    Accept-Encoding: gzip, deflate
    Cache-Control: no-cache
    Content-Length: 81
    Content-Type: application/json
    Postman-Token: aa278c41-2946-4cab-b4d5-746424a56b75
    User-Agent: PostmanRuntime/6.3.2
    X-Forwarded-For: ::1
    X-Request-Id: 59c56cf09bf234552e9218f4
BODY:
{
    "title": "Hey, my first post",
    "body": "\u003cp\u003eThis is my first blog post\u003c/p\u003e"
}

-----------------------------------------------------------------------

STATUS: 201 Created
BYTES WRITTEN: 15
HEADERS:
    Content-Type: application/json; charset=utf-8
    Server: aah-go-server
    X-Content-Type-Options: nosniff
    X-Frame-Options: DENY
    X-Request-Id: 59c56cf09bf234552e9218f4
BODY:
{
    "id": 2
}

=======================================================================

URI: http://localhost:8080/v1/posts
METHOD: GET
PROTO: HTTP/1.1
HEADERS:
    Accept: */*
    Accept-Encoding: gzip, deflate
    Cache-Control: no-cache
    Content-Type: application/json
    Postman-Token: a56e8af3-19e8-47db-b0c3-9baa68dbe92b
    User-Agent: PostmanRuntime/6.3.2
    X-Forwarded-For: ::1
    X-Request-Id: 59c56cf19bf234552e9218f5

-----------------------------------------------------------------------

STATUS: 200 OK
BYTES WRITTEN: 565
HEADERS:
    Content-Type: application/json; charset=utf-8
    Server: aah-go-server
    X-Content-Type-Options: nosniff
    X-Frame-Options: DENY
    X-Request-Id: 59c56cf19bf234552e9218f5
BODY:
{
    "posts": [
        {
            "id": 1,
            "title": "Hey, my first post",
            "body": "\u003cp\u003eThis is my first blog post\u003c/p\u003e",
            "created_at": "2017-09-22T13:05:01-07:00",
            "updated_at": "2017-09-22T13:05:01-07:00"
        },
        {
            "id": 2,
            "title": "Hey, my first post",
            "body": "\u003cp\u003eThis is my first blog post\u003c/p\u003e",
            "created_at": "2017-09-22T13:05:04-07:00",
            "updated_at": "2017-09-22T13:05:04-07:00"
        }
    ]
}

=======================================================================
```
