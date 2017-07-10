Title: Server Access Log
Desc: Server access log, aah framework logs the request and response details into log file.
Keywords: server access log, access log, request log
---
# Server Access Log

To manage aah server effectively it is necessary to know details about the request, response, processing time, client IP address, etc. aah framework provides the flexible and configurable server access log capabilities.

### Supported Access Log flag or attribute

* `clientip` - Client IP address aka Remote IP address
* `reqtime` - Time of the request is received by server. Local time in default format RFC3339 or you can provide your format
* `requrl` - Relative request URL not including query string
* `reqmethod` - Request HTTP method
* `reqid` - Request ID if present otherwise `-` is logged. Refer to config `request.id.header` in the `aah.conf`
* `reqhdr:<header_name>` - Request header, if present otherwise `-` is logged
* `querystr` - Request query string if present otherwise `-` is logged
* `resstatus` - Response Status Code
* `ressize` - Bytes sent, excluding response HTTP headers, otherwise it logs `-` if no bytes were sent
* `reshdr:<header_name>` - Response header, if present otherwise `-` is logged
* `restime` - Request processing time (in milliseconds), time elapsed between the request received by server and the last bytes were written on the wire by server

Access log processing handled in separate routine, it won't stand in way.

### Access Log Configuration

Add `access_log` section under config `request` section in the `aah.conf`.

```
request {

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
    #pattern = "%clientip %reqtime %restime %resstatus %ressize %reqmethod %requrl"

    # Access Log channel buffer size
    # Default value is `500`
    #channel_buffer_size = 500
  }
}
```

### Examples

Added respective flag into pattern config.

#### Logging User-Agent
```
// header name in-casesensitive
reqhdr:User-Agent

# OR
reqhdr:user-agent
```

#### Logging Referer URL
```
// header name in-casesensitive
reqhdr:referer
```
