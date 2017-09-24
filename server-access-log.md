Title: Server Access Log
Desc: Server access log, aah framework logs the request and response details into access log file.
Keywords: server access log, access log, request log, response log
---
# Server Access Log

<span class="badge lb-sm">Since v0.7</span>  To manage aah server effectively it is necessary to know details about the request, response, processing time, client IP address, etc. aah framework provides the flexible and configurable server access log capabilities.

### Supported Access Log flag or attribute

* `clientip` - Client IP address aka Remote IP address
* `reqtime` - Time of the request is received by server. Local time in default format RFC3339 or you can provide your format
* `requrl` - Relative request URL not including query string
* `reqmethod` - Request HTTP method
* `reqproto` - Request Protocol. For e.g: `HTTP/1.1`, `HTTP/2.0`
* `reqid` - Request ID if present otherwise `-` is logged. Refer to config `request.id.header` in the `aah.conf`
* `reqhdr:<header_name>` - Request header, if present otherwise `-` is logged
* `querystr` - Request query string if present otherwise `-` is logged
* `resstatus` - Response Status Code
* `ressize` - Bytes sent, excluding response HTTP headers, otherwise it logs `-` if no bytes were sent
* `reshdr:<header_name>` - Response header, if present otherwise `-` is logged
* `restime` - Request processing time in `milliseconds`. The time elapsed between the request received by server and the last bytes were written on the wire by server
* `custom` - Add non-space string into log string

Access log processing handled in separate routine, it won't stand in way.

#### Default access log pattern

```cfg
%clientip %custom:- %reqtime %reqmethod %requrl %reqproto %resstatus %ressize %restime %reqhdr:referer
```

#### Sample Server Access Log of default pattern

<pre style="font-size:10px"><code>
::1 - 2017-07-21T14:56:15-07:00 GET / HTTP/2.0 200 12735 0.7020 "https://aahframework.org/"
::1 - 2017-07-21T14:56:15-07:00 GET /assets/css/aah-f2f8e6e.css HTTP/2.0 200 7408 0.3377 "https://aahframework.org/"
::1 - 2017-07-21T14:56:15-07:00 GET /assets/js/aah-f2f8e6e.js HTTP/2.0 200 522 0.2688 "https://aahframework.org/"
::1 - 2017-07-21T14:56:15-07:00 GET /assets/css/bootstrap.min.css HTTP/2.0 200 125523 4.1035 "https://aahframework.org/"
::1 - 2017-07-21T14:56:15-07:00 GET /assets/img/aah-logo-64x64.png HTTP/2.0 200 2404 0.2033 "https://aahframework.org/"
::1 - 2017-07-21T14:56:15-07:00 GET /assets/js/ie10-viewport-bug-workaround.js HTTP/2.0 200 651 0.2062 "https://aahframework.org/"
::1 - 2017-07-21T14:56:15-07:00 GET /assets/fonts/timeburnerbold.ttf HTTP/2.0 200 78170 1.4987 "https://aahframework.org/assets/css/aah-f2f8e6e.css"
</code></pre>

### Access Log Configuration

Add `access_log` section under the config `server` section in the `aah.conf`.

```cfg
server {
  # ...

  # To manage aah server effectively it is necessary to know details about the
  # request, response, processing time, client IP address, etc. aah framework
  # provides the flexible and configurable access log capabilities.
  access_log {
    # Enabling server access log
    # Default value is `false`
    enable = true

    # Absolute path to access log file or relative path.
    # Default location is application logs directory
    #file = "{{ .AppName }}-access.log"

    # Default server access log pattern
    #pattern = "%clientip %custom:- %reqtime %reqmethod %requrl %reqproto %resstatus %ressize %restime %reqhdr:referer"

    # Access Log channel buffer size
    # Default value is `500`
    #channel_buffer_size = 500

    # Include static files access log too.
    # Default value is `true`.
    #static_file = false
  }

  # ...
}
```

### Examples

Added respective flag into pattern config.

#### Logging User-Agent
```cfg
// header name in-casesensitive
reqhdr:User-Agent

# OR
reqhdr:user-agent
```

#### Logging Referer URL
```cfg
// header name in-casesensitive
reqhdr:referer
```
