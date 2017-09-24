Title: aah Log Configuration
Desc: aah log configuration
Keywords: log configuration, logger configuration, log config
---
# aah Log Configuration

The configuration syntax is used by aah framework is very similar to HOCON syntax. To learn more about **[configuration syntax](configuration.html)**.

This document describes aah log configurations. Typically log configuration done at every environment profile level and you override it via external config file.

Reference to [App Config](app-config.html), [Routes Config](routes-config.html), [Security Config](security-config.html).

## log.receiver
Receiver is where the log values gets logged. Out-of-the-box framework supports `console` and `file` receivers. Also you can add `Hooks` into per aah logger instance, for sending log data to splunk, kibana, etc.

Default value is `console`.
```cfg
receiver = "file"
```

## log.level
Level indicates the logging levels like `ERROR`, `WARN`, `FATAL`, `PANIC`, `INFO`, `DEBUG` and `TRACE`. Config value can be in lowercase or uppercase.

Default value is `DEBUG`.
```cfg
level = "info"
```

## log.format
To define log entry output format. Supported formats are `text` and `json`.

Default value is `text`.
```cfg
format = "json"
```

## log.pattern
Pattern config is defined composable log pattern. Patten flag is used as `%flagname` or `%flagname:format`.

Supported log patterns are:
```cfg
FmtFlags is the list of log format flags supported by aah log library
Usage of flag order is up to format composition.
  level     - outputs ERROR, WARN, FATAL, PANIC, INFO, DEBUG, TRACE
  appname   - outputs Application Name
  insname   - outputs Application Instance Name
  reqid     - outputs Request ID from HTTP header
  principal - outputs Logged-In subject primary principal value
  time      - outputs local time as per format supplied
  utctime   - outputs UTC time as per format supplied
  longfile  - outputs full file name: /a/b/c/d.go
  shortfile - outputs final file name element: d.go
  line      - outputs file line number: L23
  message   - outputs given message along supplied arguments if they present
  fields    - outputs field values into log entry
  custom    - outputs string as-is into log entry
```
<div class="alert alert-info-blue">
<p><strong>Note:</strong> Log pattern is not applicable of JSON log format.</p>
</div>

Default value is `%time:2006-01-02 15:04:05.000 %level:-5 %message`.
```cfg
pattern = "%time:2006-01-02 15:04:05 %level:-5 %shortfile %line %custom:- %message"
```

## log.color
Log colored output, applicable only to **`console`** receiver type.

Default value is `true`.
```cfg
color = false
```    

## log.file
File config attribute is applicable only to **`file`** receiver type.

Default value is `aah-log-file.log`.
```cfg
file = "myapp.log"
```

## Section: rotate { ... }
Rotate config section is applicable only to **`file`** receiver type.

### log.rotate.policy
Policy is used to determine rotate policy. Currently it supports `daily`, `lines` and `size`.

Default value is `daily`.
```cfg
# daily rotation, it's default one.
rotate {
  policy = "daily"
}

# size based rotation
rotate {
  policy = "size"
  size = "512mb"
}

# line based rotation
rotate {
  policy = "lines"
  lines = 100000
}
```

### log.rotate.size
This is applicable only to if **`policy`** is `size`.

Default value is 512mb.
```cfg
size = "100mb"
```

### log.rotate.lines
This is applicable only to if **`policy`** is `lines`.

Default value is 100000.
```cfg
lines = 50000
```
